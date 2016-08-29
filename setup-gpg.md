## Amazon KMS for storing master keys which unlock GnuPG/Keyrings 

> or at bare minimum, store the secret keys themselves.

My recommendation is to use a stateless docker container with something like Etcd, or 
AWS S3 Sync, or  use Bup/ Duplicity to provide incremental backup integrity across S3.

**worth noting:** aws-s3-sync via the cli doesn't do incremental backups.  

You might also decide to bundle your keyrings via the stateless docker as a tar using "docker export"
This will allow you to maintain state whenever it is convenient or when there is a change to the keyrings.
It's fairly simple to encrypt the tar files via Duplicity, and on the hardware level using KMS profiles & key aliasing.
(described below)

Another way is to simply use KMS or Etcd as the key storage faculty and ignore the keyring storage entirely.

The method for using KMS I describe below is more than for just encryption/ private key storage...
I use ETCD here to store useful shell commands & pass information between clusters easily. (like pork/boom)

First setup your Amazon Creds:  (or Etcd/ Duplicity over GPG, whatever it might be)
```
---------------------
printf " \
	AWS_ACCESS_KEY_ID=****************  \n \
	AWS_SECRET_ACCESS_KEY=*****************************************\n \
	AWS_DEFAULT_REGION=us-west-1 \n" \ 
| tee ./root/.aws/test.env
```

# propagate a profile based test.credentials for aws
# so you can then reference it when calling sync
```
printf " \
[profile devops]  \n \
	region = us-west-1  \n \
	aws_access_key_id = **************** \n \
	aws_secret_access_key = *****************************************\n " \
| tee -a ./test.credentials
```
# pre-requisites:
 - ubuntu with docker installed
 - you must like bears
 - you must like pterodactyls (relevant: http://theoatmeal.com/comics/ptero)

# in this example we combine them: 

```
$ ptero set bear odactyl
$ ptero get bear
odactyl  < returns that!
```
brilliant!  it's a key => value store, let's move on:

# I also create an aws.sh file that uses the docker container cli.
- this allows me to be lazy and rarely update stuff, while increasing security due to multipl layers of virutalization 

# a simple shell script to run aws cli via docker:
```
$ sudo cat /home/ubuntu/.scripts/dockers/aws.sh 
 
```

```
			#!/bin/bash
			# contents of my aws.sh script
			AWS_CONFIG_ENV=/root/.aws/credentials  #this is your creds as root (docker runs as root)
			INSTANCE=$(/usr/bin/curl -s http://169.254.169.254/latest/meta-data/instance-id)
			IMAGE=xueshanf/awscli:latest

			sudo docker pull $IMAGE
			sudo docker run -v /home/ubuntu/cli:/root/.aws/ --env-file=$AWS_CONFIG_ENV $IMAGE /bin/bash -c "$1"
```

# KMS key needed:
 - I used "saws" instead of "aws" because it's more progressively truthful
 - set it up like so:
 
saws> aws kms create-key --profile devops --region us-west-1   # use appropriate credential profile & region
````
				{
				    "KeyMetadata": {
				        "KeyId": "kja8a08f9fha0fha0f0a9ij0as9jd0as", # I pasted gibberish for effect
				        "Description": "", 
				        "Enabled": true, 
				        "KeyUsage": "ENCRYPT_DECRYPT", 
				        "KeyState": "Enabled", 
				        "CreationDate": 1461061812.742,  # creation date
				        "Arn": "arn:aws:kms:us-west-11010101:key/0f0faaaa-e1a2-42d1-9c71-a373rrr3asss5f",  # gibberish
				        "AWSAccountId": "929293844214112" # gibberish
				    }
				}
```
```
saws> aws kms list-keys --output text --region us-west-1  # lists the key I just created

  KEYS    arn:aws:kms:us-west-1:929293844214112:key/0f0faaaa-e1a2-42d1-9c71-a373rrr3asss  0f0faaaa-e1a2-42d1-9c71-a373rrr3asss
```
# now I assign my master key to alias name "devopsWest" using the correct profile & region & targetKey data
saws> aws kms create-alias --alias-name alias/devopsWest --profile devops --region us-west-1 --target-key-id 0f0faaaa-e1a2-42d1-9c71-a373rrr3asssf
```

# validate the alias is listed

```
saws> aws kms list-aliases --output text --profile devops --region us-west-1
ALIASES arn:aws:kms:us-west-1:286824693784:alias/devopsWest     alias/devopsWest        0f0faaaa-e1a2-42d1-9c71-a373rrr3asssf
```

# NOTES:
	* you could also store these keys in your etcd storage if you wished, as etcd can be clusterable easily using rancher/docker
	* I prefer to just use etcd as the key/value store for common devops commands, but I may try it out eventually as a master
	* right now I use kms to store master keys *critically* accessible only via IAM
	* I would rely on GnuPG for accessing the secret keys, if ever, because they can only be opened that way if the KMS master is required to open GnuPG rings, alobng with some form of salt or vector (mine get generated from a custom shell profile script and cached).


# IN SUMMARY:

# now you can execute sync commands using that kMS key without ever reading secret keys into memory, or decrypting them.  This is done by an alias.  Should you need to validate the user outside of IAM, then you could use the secret key verification for that.
# after you're done pushing keys into whatever key store you decide, make sure you delete your secret keys securely using the following technique:

# back up your bash history (as if it were the key to delete) ```
$  printf "\n 
	sudo sh /home/ubuntu/.scripts/dockers/aws.sh \
	aws s3 sync ~/.bash_history s3://pterops.com/g  --profile devops --sse aws:kms --sse-kms-key devopsWest --dryrun
' | ptero set s3up.hist $1 \n " | sh
     ```
# srm delete using openbsd PUFFER  -P ```
$  sudo srm -fm -P ~/.bash_history  
```
# now
