# amazon-docker-devops
*"my way to do devops on amazon aws using docker."*

> this is an ongoing project that is essentially a guidebook on how I do devops related tasks, in supplement to the [dockistry project](https://github.com/dockistry).  Dockistry is in pre-alpha, but these command guides are what I use to design the go-lang code used in dockistry automated events.

## [chapter 1](https://github.com/forktheweb/amazon-docker-devops/blob/master/setup-ubuntu.md)
- setup an ubuntu trusty instance
- install gems support
- install nodeJs
- install the aws-cli, and aws-shell, and saws
- install and mount elastic fs using NFS4

## [chapter 2](https://github.com/forktheweb/amazon-docker-devops/blob/master/setup-nginx-rancher-boom-etcd-go-ssl.md)
- link your cross-cluster credentials and cluster-management commands
- command and key/value management
- setting up websites on SSL
- setting up rancher management

## [chapter 3](https://github.com/forktheweb/amazon-docker-devops/blob/master/setup-gpg.md)
- how to generate a gpg keyring and private key storage
- how to create a docker container-based keyring
- using profiles with Amazon's Key Management Storage uing IAM

> this project is a work in progress, here's the todos:

- [] chapter 4
  - [] duplicty using gpg encryption for backups
  - [] using bup for hot backups
  - [] temporary containers for development
  - [] strategies for dealing with ephemeral (i.e.- not volume mounted) docker containers
  - [] strategies for dealing with volume mounted containers
  - [] using docker-compose 2.0 spec

- [] chapter 5
  - [] RDS and Aurora data sync strategies
  - [] Load-balancing using L7 and Rancher
  - [] Service discovery using Consul
  - [] Automatic health checks and node-load distribution with HA Proxy and Nginx

- [] chapter 6
  - [] Using RethinkDB to Shard LOTS of data
  - [] Using Citus/Postgres clusters

- [] chapter 7
  - [] How to build an Angular SPA using Docker containers
  - [] How to build an API using Docker containers
  - [] How to build a React Native app using the same API
  - [] How to create a code pipeline and automate testing with Travis CI
