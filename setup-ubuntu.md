### setup ubuntu 14.04 trusty
	#!/bin/bashins
	sudo apt-get update
	sudo apt-get install apt-transport-https ca-certificates
	sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
	sudo cat /etc/apt/sources.list.d/docker.list # unnecessary (just prints out the docker.list if any)
	sudo truncate -s 0 /etc/apt/sources.list.d/docker.list # truncate the docker.list if it exists
	echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" | sudo tee -a /etc/apt/sources.list.d/docker.list # put the proper repo in the docker list for updating on trusty 14.4 ubuntu
	sudo apt-get update
	sudo apt-get purge lxc-docker
	sudo apt-cache policy docker-engine
	sudo apt-get update
	sudo apt-get install linux-image-extra-$(uname -r) -y
	sudo apt-get install docker-engine -y
	sudo service docker start #should already be running
	sudo docker run hello-world #should print Hello from Docker.
	sudo usermod -aG docker ubuntu #adds docker to the ubuntu group
	cd /home/ubuntu
	sudo apt-get install python-pip -y
	sudo apt-get install npm -y
	sudo npm install -g bower
	# install ruby, git, sqllite, yaml, and other build essentials
	sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev -y
	
### install ruby gems support
	#!/bin/bash
	git clone https://github.com/rbenv/rbenv.git ~/.rbenv
	echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
	echo 'eval "$(rbenv init -)"' >> ~/.bashrc
	exec $SHELL
	git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
	echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
	exec $SHELL
	git clone https://github.com/rbenv/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash
	rbenv install 2.2.3
	rbenv global 2.2.3
	ruby -v #should print ruby 2.2.3p173 (2015-08-18 revision 51636) [x86_64-linux]
	gem install bundler
	
### install nodejs
	#!/bin/bash
	curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
	sudo apt-get install -y nodejs
	
### optional aws
	sudo pip install awscli
	sudo pip install aws-shell
	sudo pip install saws

### elastic file system
	sudo apt-get install nfs-common  # for Elastic FS
