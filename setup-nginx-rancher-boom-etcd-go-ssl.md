### links
> this process links the aws credentials for the cluster and the boom-ops commands from efs to the node home directory, thereby making it retardedly simple to use various commands and services across cluster
    
    sudo mount -t nfs4 -o nfsvers=4.1 us-east-1a.fs-4011dc09.efs.us-east-1.amazonaws.com:/ efs
    ln -s efs/.aws .aws
    ln -s efs/.boom .boom

	
##### this is an optional backup process I'm working on using ephemeral keyring containers:
> I will be packaging credentials within the ephemeral containers and taring them up using encryption 
- build from a private Dockerfile with credentials stored in it, as environment variables.
- build ephemeral container, which creates keyrings with GnuPG inside it.
- export the keyring inside the container
- export the container state
- remove the container and srm delete the container state
- use duplicity to move the container to S3 using KMS SSE and GPG to encrypt the tar file
- store the keyring container's tarfile location in an etcd keystore or in boom
- use a master password for all gpg keystores

 
	### boom
	> shared devops commands in nice little groups, mounted on efs for simple usage across a big cluster
	
		gem install boom
	 
	### etcd
		docker run -d --name=etcd quay.io/coreos/etcd # mozafukin pterodactyls
		docker exec etcd /etcdctl set ptero dactyl
		docker exec etcd /etcdctl get ptero 
	    # should print: dactyl
		echo "alias ptero='docker exec etcd /etcdctl $1'" | tee -a ~/.bash_aliases
	
		# now you can do:
		pte set stega saurus
		pte get stega
		# prints: saurus
	
		# useful if you dig vim-spf13 jailed... you could volume mount with -v to make editing easier on host
		
		docker pull --name vim-box yonidavidson/vim-box-spf13
		alias vim="docker run -it yonidavidson/vim-box-spf13"
		
		# sometimes you do not want spf13 in a docker (actually most times)
		curl http://j.mp/spf13-vim3 -L -o - | sh

	
	# optionally install go-lang
		#!/bin/bash
		bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
		[[ -s "$HOME/.gvm/scripts/gvm" ]] && source "$HOME/.gvm/scripts/gvm"
		sudo apt-get install bison
		exit; #logout
		gvm version
		gvm listall
		gvm install go1.4
		gvm use go1.4
		go version
	
# nginx rProxy + SSL with Rancher
> this makes it a very quick process indeed to set up websites hosted on SSL using whatever docker service you want

provision dirs necessary to operate a vanilla nginx reverse proxy and bind as the data volumes

	sudo mkdir /usr/share/nginx; sudo mkdir /usr/share/nginx/html;  # shared path
	
	sudo mkdir -p /etc/nginx/ssl;  sudo ln -s /etc/nginx/ssl /etc/nginx/certs;  # link up incidental paths & make sure directory exists for certs
	
	sudo mkdir -p /etc/nginx/vhost.d; sudo touch /etc/nginx/vhost.d/default.conf # provison empty default conf for nginx
	
create rancher server instance as daemon with specific virtual port 8080 and using target domain pteros.com

	# docker run -d -v /var/lib/mysql:/var/lib/mysql --restart=always --name=rancher-server -p 8080:8080 -e VIRTUAL_HOST=pterops.com -e VIRTUAL_PORT=8080 -e "LETSENCRYPT_HOST=pterops.com" -e "LETSENCRYPT_EMAIL=stunts@gmail.com" rancher/server
	
provision the jwilder reverse proxy using aforementioned settings, port 80 will 301 redirect requests to SSL host automatically

	docker run -d --name=nginx-proxy --link=rancher-server --restart=always -p 80:80 -p 443:443 -v /etc/nginx/vhost.d -v /usr/share/nginx/html -v /etc/nginx/ssl:/etc/nginx/certs -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy
	
or do it without rancher-server

	docker run -d --name=nginx-proxy --restart=always -p 80:80 -p 443:443 -v /etc/nginx/vhost.d -v /usr/share/nginx/html -v /etc/nginx/ssl:/etc/nginx/certs -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy
	
provision the nginx LetsEncrypt helper container and attach it to the nginx-proxy instance

	sudo docker run -d --restart always --name ssl-proxy -v /etc/nginx/ssl:/etc/nginx/certs:rw --volumes-from nginx-proxy -v /var/run/docker.sock:/var/run/docker.sock:ro thomastweets/docker-letsencrypt-nginx-proxy-companion
