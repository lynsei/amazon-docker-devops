# Setup GPG

Let's be brave boys and girls:

Download gpg-keygen.py python script and run it.  It's slow but it's python after all.  Don't you know that slow things are good with encryption because that means you gather entropy!?

```
$ cd ~/; wget https://raw.githubusercontent.com/object-code/gpg-keygen/master/gpg-keygen.py; 
```

Deploy GPG using nifty python scripting methods:

```
$ sudo ~/gpg-keygen.py --t /root/.gnupg/whomever --step generateMasterKey  \
      --master-key-length 4096 \   
      --master-key-type RSA  \
      --identity-email derp@gmail.com  \
      --identity-comment Whatever  \
      --identity-name "Bob Dobbs"  \
      --master-key-expire 0
```      
      
> note:  4096 keys pose no real security advantage, but nevertheless.

-  Temporary directory for sensitive data will be '~/.gnupg/whomever'. 
-  Make sure you delete using 'srm' (secure-delete) once it's not needed! 

# Tidy Up

```
$ sudo mv /root/.gnupg/whomever/tmp/gpg-homedir/pubring.gpg ~/root/.gnupg/pubring.gpg 
$ sudo mv /root/.gnupg/whomever/tmp/gpg-homedir/pubring.gpg ~/root/.gnupg/secring.gpg
$ sudo mv /root/.gnupg/whomever/tmp/gpg-homedir/gpg.conf ~/root/.gnupg/gpg.conf
$ sudo mv /root/.gnupg/whomever/tmp/gpg-homedir/trustdb.gpg ~/root/.gnupg/trustdb.gpg
```

### Export your public key:
```
$ sudo gpg --armor --export derp@gmail.com

-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1

mQINBFbOe7kBEACaS8fTIxLGqcmRN3TsdJYMEy7DtBpJFx9o8SMfxk6oTLk1Rjkj
TSpMyXamJMkC4RwrYD4NvQOiUzUvO+gVBssxQvbU/TJLBOusuwAgdIScJZMF2hZG
UDGivOaqysSAmEiraUxtb7w7dGCtdftRLeGOhEat9ldVqFHBQ2UAS7otxXQHYcRH
lTLlvtedTq7mnML6yIdaS/jz5bGxFuWkwB1ojKGXeTJMd7nfhXmbnTfc2TCm6ezt
qeLwGx4ZDBU2iCDxszCRgoOdy4dUYiuMRv6iN1EsT9YG54AC764IVRx993szOYdC
wb97ahuahBlFwgUl8DOWWwAEslqsYH+Wg4+ysOfog5K8VDqkKzYzlj7y8BqhpiFd
z6S+R2QjufkRTWO5eJZzI8JYizkGo7Mo2d65dKLFV68w0V4F0FWdz1ffAinUDPxv
3GzQGus2fZOLm3oMA345UwaQXnf5PVkrSDp4qXxinNGKeU3+M/RO1LRn9975Watx
4hHg9cxA05SBoO4xet17Gy0Kv1FfbX6lQp/dFjO1e7KmLxuI1YlJBPNDgFzJWpZz
d8f69Pk5rOtEaTnPKU6BTkZTcOM2Hsw5VeoTHphAtw6RyF6QugitDs+mrsrY8bZ4
Kf802Lx2gO56QcbdqBqOJioJxPnkBKoz/rPlpAD4Ri+OsfKzgjg9OIuMeQARAQAB
tDRDaHJpc3RvcGhlciBELiBIb2dhbiAoT2JqZWN0Y29kZSkgPHN0dW50c0BnbWFp
bC5jb20+iQI3BBMBCgAhBQJWznu5AhsDBQsJCAcDBRUKCQgLBRYCAwEAAh4BAheA
AAoJEDFigqgY74rjQLsP/22tvu1OLKzstR1H61Hife6ejyvPKNyOSwvPzCFzMmdg
M2uVS7aMj466kw6bM9j3/dXi/vcxU7psxKud8QWwFbBJWPuMZVQ7ukH7Eo5FbV3y
0zlqjaW1UppYWnhFjpj5snMj3prloMhjJML/SRO3N8UaBBm72g9AyBSzDkR3r160
Zq3hlQES7jZPqWAGRANXh8+Es2oY793zGFzbhGVBSmyDdOud6sq+V1/7hf/7PkFc
5tK8Q+AZkNmPqpnlTTRpUMdSibqms1ej+b3a0x0qeMQacEjTWbOfB1vPgFFU2ri/
UHC3N0s0vKW0btLnt0xFtcHszs9d11qwYU2fFVPjmzp/IqRd/L0AVHrj4Zu/XoOA
g5iS++3hMctpS8s1hXb9kF+TVZO/Nsp0+Y9yQrXhQxmXilv6NxICfwY9t06hER8m
8ERd+i8rhC2OFsWjI6U5C61CRinHX0lOiOOboicDNIxx5U7SL+XM8cYnRMf8Xs26
lM8SgxFIn3bI4kt8MZ1ZsAo4Dl60o5HNw2JYQ18TwBBKbcN+2nNzdjXJbil5Q07r
96BEAZKQYIsM/kjg0r/kNadwMAOC5vFo+Yify2wJkhLVpnSTdDwO172ScSbtr2zO
KN1eNgyVmohN6qniZdZqvENi9XxyllSSDvEaXyOe7Y9Z2ZxNTPGbr6xndVNc+x2x
=Dyl8
-----END PGP PUBLIC KEY BLOCK-----

```


### Edit your keyring:
```
$ sudo gpg --interactive --edit-key derp@gmail.com
 
$ sudo gpg --list-keys
/root/.gnupg/pubring.gpg
------------------------
pub   4096R/18EF8AX9 2016-02-25
uid                  Bob Dobbs <derp@gmail.com>

gpg> help

```


### Assign a master pass:
```
gpg> passwd
This key is not protected.
Enter the new passphrase for this secret key.
.... (add in the password you want)
       
```

### Publish your Public Key
```                   
gpg> keyserver
Enter your preferred keyserver URL: https://pgp.mit.edu/

You need a passphrase to unlock the secret key for
user: "Bob Dobbs  <derp@gmail.com>"
4096-bit RSA key, ID 18EF8XSE3, created 2016-02-25
```

### Add photo of Kittens
```
 
gpg> addphoto

Enter JPEG filename for photo ID: 
^z   (suspend process)

devops.png  100%[==============>]  14.85K  --.-KB/s   in 0.004s 

$ fg

Are you sure you want to use it? (y/N) y
gpg: no photo viewer set
gpg: unable to display photo ID!
Is this photo correct (y/N/q)? y

```

### List your keys
```
gpg> list

pub  4096R/18EF8AE3  created: 2016-02-25  expires: never       usage: SC  
                     trust: ultimate      validity: ultimate
[ultimate] (1). Bob Dobbs  <derp@gmail.com>
[ unknown] (2)  [jpeg image of size 15508]


```

### Export them
> Note: This is where you need to care.  Always store on removable media or externally at secret location.  If you store the location, encrypt it.  If you use Amazon, store keys with KMS and use SSE EBS block stores.  If you want to go overboard, store keyrings in encrypted docker containers.  (I'm writing software to handle that and will be adding more tutorials soon)

```
gpg> export


gpg> fpr
pub   4096R/18EF8AE3 2016-02-25 "Bob Dobbs  <derp@gmail.com>"
 Primary key fingerprint: 50A8 A9EB F636 8DBA FC54  D208 3162 82A8 18EF 8AE3

gpg> showpref
[ultimate] (1)."Bob Dobbs  <derp@gmail.com>"
     Cipher: AES256, AES192, AES, CAST5, 3DES
     Digest: SHA512, SHA384, SHA256, SHA224, SHA1
     Compression: ZLIB, BZIP2, ZIP, Uncompressed
     Features: MDC, Keyserver no-modify
     Preferred keyserver: https://pgp.mit.edu/
[ unknown] (2)  [jpeg image of size 15508]
     Cipher: AES256, AES192, AES, CAST5, 3DES
     Digest: SHA512, SHA384, SHA256, SHA224, SHA1
     Compression: ZLIB, BZIP2, ZIP, Uncompressed
     Features: MDC, Keyserver no-modify

gpg> quit
```


# Armored ASC Export 

Let's export your public key as an armored ascii text blurb to put at the bottom of your emails (and log it in a file).
Note:  Again, do *not* store your private key locally.  

```
$ sudo gpg --export --armor derp@gmail.com >> ~/.gpgpublic        
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1

mQINBFbOe7kBEACaS8fTIxLGqcmRN3TsdJYMEy7DtBpJFx9o8SMfxk6oTLk1Rjkj
TSpMyXamJMkC4RwrYD4NvQOiUzUvO+gVBssxQvbU/TJLBOusuwAgdIScJZMF2hZG
UDGivOaqysSAmEiraUxtb7w7dGCtdftRLeGOhEat9ldVqFHBQ2UAS7otxXQHYcRH
lTLlvtedTq7mnML6yIdaS/jz5bGxFuWkwB1ojKGXeTJMd7nfhXmbnTfc2TCm6ezt
qeLwGx4ZDBU2iCDxszCRgoOdy4dUYiuMRv6iN1EsT9YG54AC764IVRx993szOYdC
wb97ahuahBlFwgUl8DOWWwAEslqsYH+Wg4+ysOfog5K8VDqkKzYzlj7y8BqhpiFd
z6S+R2QjufkRTWO5eJZzI8JYizkGo7Mo2d65dKLFV68w0V4F0FWdz1ffAinUDPxv
3GzQGus2fZOLm3oMA345UwaQXnf5PVkrSDp4qXxinNGKeU3+M/RO1LRn9975Watx
4hHg9cxA05SBoO4xet17Gy0Kv1FfbX6lQp/dFjO1e7KmLxuI1YlJBPNDgFzJWpZz
d8f69Pk5rOtEaTnPKU6BTkZTcOM2Hsw5VeoTHphAtw6RyF6QugitDs+mrsrY8bZ4
Kf802Lx2gO56QcbdqBqOJioJxPnkBKoz/rPlpAD4Ri+OsfKzgjg9OIuMeQARAQAB
tDRDaHJpc3RvcGhlciBELiBIb2dhbiAoT2JqZWN0Y29kZSkgPHN0dW50c0BnbW77
bC5jb20+iQI3BBMBCgAhBQJWznu5AhsDBQsJCAcDBRUKCQgLBRYCAwEAAh4BAheA
AAoJEDFigqgY74rjQLsP/22tvu1OLKzstR1H61Hife6ejyvPKNyOSwvPzCFzMmdg
M2uVS7aMj466kw6bM9j3/dXi/vcxU7psxKud8QWwFbBJWPuMZVQ7ukH7Eo5FbV3y
0zlqjaW1UppYWnhFjpj5snMj3prloMhjJML/SRO3N8UaBBm72g9AyBSzDkR3r160
Zq3hlQES7jZPqWAGRANXh8+Es2oY793zGFzbhGVBSmyDdOud6sq+V1/7hf/7PkFc
5tK8Q+AZkNmPqpnlTTRpUMdSibqms1ej+b3a0x0qeMQacEjTWbOfB1vPgFFU2ri/
UHC3N0s0vKW0btLnt0xFtcHszs9d11qwYU2fFVPjmzp/IqRd/L0AVHrj4Zu/XoOA
g5iS++3hMctpS8s1hXb9kF+TVZO/Nsp0+Y9yQrXhQxmXilv6NxICfwY9t06hER8m
8ERd+i8rhC2OFsWjI6U5C61CRinHX0lOiOOboicDNIxx5U7SL+XM8cYnRMf8Xs26
lM8SgxFIn3bI4kt8MZ1ZsAo4Dl60o5HNw2JYQ18TwBBKbcN+2nNzdjXJbil5Q07r
96BEAZKQYIsM/kjg0r/kNadwMAOC5vFo+Yify2wJkhLVpnSTdDwO172ScSbtr2zO
KN1eNgyVmohN6qniZdZqvENi9XxyllSSDvEaXyOe7Y9Z2ZxNTPGbr6xndVNc+x2y
=Dyl8
-----END PGP PUBLIC KEY BLOCK-----
```

##### another way to list keys:

```
$ sudo gpg --list-keys --verbose --fingerprint  >> ~/.gpgpublic
gpg: using PGP trust model
/root/.gnupg/pubring.gpg
------------------------
pub   4096R/18XS8AE3 2016-02-25
      Key fingerprint = 50A8 A9EX F636 8DBA FC54  D208 3162 82A8 18EF 8AE1
uid                  "Bob Dobbs  <derp@gmail.com>"
```

NOTE:  You can also choose to publish your public key on a website like https://pgp.mit.edu/  then just link to the key in the footer of your e-mails instead of publishing a long armored string


# Export Keys
> This is a bad practice, and NEVER do it locally.  Only export keys on a remote secret container or physically secured removable media.  Always encrypt them for storage using a master password.  Rotate your passwords. 

```
sudo gpg --export-secret-keys -a 22EX8AEA > srm_delete_me.asc
```
- Wherever you choose to store this, make sure it is 100% disassociated and take care in how your store the file
- Only store your private key on media you absolutely control and that is both obscured and secured physically or in a private cloud area that you store the location of securely, and encrypt using a master password.

# Delete your trail

Once you've copied your keys to an external disk or smartcard, SRM delete the secret keys and bash history please (don't forget!!!!)
sudo srm /root/.gnupg/secring.gpg


Why delete your private keys securely and keep them somewhere you bring with you or hide?

* They cannot be stolent or hacked this way
* You won't have gone through all this trouble for nothing.

Be smart and don't make kittens cry by storing your keys insecurly.
 

## Amazon KMS

> let's use KMS for key profiling and storage.  
 

### Setup your credentials:

> note:  I'm using an EFS mount in this example, which you don't need to do.

```
printf " \
[profile devops]  \n \
	region = us-west-1  \n \
	aws_access_key_id = **************** \n \
	aws_secret_access_key = *****************************************\n " \
| tee -a /home/ubuntu/EFS/.aws/credentials
```
### Start with:

 - ubuntu with docker installed
 - you might use etcd or boom for storing & managing or encrypte container key locations

 
As a personal preferenc, I create an aws.sh file that uses a docker containerized AWS CLI, so I don't need to install it.
This allows me to be lazy, which I'm great at!   ;)

Here's an example of how I might script an AWS-CLI.sh container for usage:
 
```
	#!/bin/bash
	# contents of my aws.sh script
	AWS_CONFIG_ENV=/home/ubuntu/EFS/.aws/credentials   
	INSTANCE=$(/usr/bin/curl -s http://169.254.169.254/latest/meta-data/instance-id)
	IMAGE=xueshanf/awscli:latest
	
	sudo docker pull $IMAGE
	sudo docker run -v /home/ubuntu/efs/cli:/root/.aws/ --env-file=$AWS_CONFIG_ENV $IMAGE /bin/bash -c "$1"
```
 
> Sometimes I use "[saws](https://github.com/donnemartin/saws)" instead of "aws" because it's more progressively truthful


##### Set it up like so:
 

````
saws> aws kms create-key --profile devops --region us-west-1   # use appropriate credential profile & region
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
 
saws> aws kms list-keys --output text --region us-west-1  # lists the key I just created

  KEYS    arn:aws:kms:us-west-1:929293844214112:key/0f0faaaa-e1a2-42d1-9c71-a373rrr3asss  0f0faaaa-e1a2-42d1-9c71-a373rrr3asss
```

Now I assign my master key to alias name "devopsWest" using the correct profile & region & targetKey data

```
saws> aws kms create-alias --alias-name alias/devopsWest --profile devops --region us-west-1 --target-key-id 0f0faaaa-e1a2-42d1-9c71-a373rrr3asssf
```

##### Validate the alias is listed:

```
saws> aws kms list-aliases --output text --profile devops --region us-west-1
ALIASES arn:aws:kms:us-west-1:286824693784:alias/devopsWest     alias/devopsWest        0f0faaaa-e1a2-42d1-9c71-a373rrr3asssf
```

# IAM Profiles with KMS

Using IAM profiles like the above allows you to perform key-sync commands with limited IAM profiles, thereby restricting and limiting access through your AWS resources to only KMS and EC2 (for instance).

If you are wondering where these keys can be found, they are under "Security Credentials, Encryption Keys" in your AWS account.
There you can assign users and administrators to particular keys in various regions, and you get a fancy "ARN" resource locator that looks like this:

`arn:aws:kms:us-east-1:212312384:key/5653271a-a406-4652-ac75-9def444128b2b7`

> Now you can SSE encrypt whatever you want, with the KMS key, like so:

```
$  printf "\n 
	sudo sh /home/ubuntu/.scripts/dockers/aws.sh \
	aws s3 sync ~/file_I_want_to_encrypt s3://yourBucketName  --profile profileName --sse aws:kms --sse-kms-key keyName 
' | boom pterops s3up.hist $1 \n " | sh  # note: boom just stores the command for future reference.
     ```
##### SRM with Openbsd PUFFER 

```
$  sudo srm -fm -P ~/.bash_history  
```


Congratulations, you win.
