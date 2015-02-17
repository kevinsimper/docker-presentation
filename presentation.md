title: Docker Hosting
controls: false
style: style.css

--

# Docker Hosting
## Who can we ship this container?

--

# About me

## @kevinsimper
## Graduateland

--

# Disclaimer
## Not paid reviews!

--

# Do it yourself vs PAAS

--

# Tutum

![tutum](tutum.png)

--

# Orchard

![orchard](orchard.png)

--

# StackDock

![stackdock](stackdock.png)

--

# Joyent

![joyent](joyent.png)

--

## Okay, lets create a cluster with Tutum

--

![tutum-1](tutum-1.png)

--

![tutum-1](tutum-2.png)

--

![tutum-1](tutum-3.png)

--
```
$ docker pull nginx:latest

$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu              14.04               5ba9dab47459        2 weeks ago         188.3 MB
ubuntu              14.04.1             5ba9dab47459        2 weeks ago         188.3 MB
ubuntu              latest              5ba9dab47459        2 weeks ago         188.3 MB
ubuntu              trusty              5ba9dab47459        2 weeks ago         188.3 MB
nginx               latest              4b5657a3d162        2 weeks ago         91.66 MB

$ docker tag nginx tutum.co/kevinsimper/mynginx
$ docker images
REPOSITORY                     TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu                         14.04               5ba9dab47459        2 weeks ago         188.3 MB
ubuntu                         14.04.1             5ba9dab47459        2 weeks ago         188.3 MB
ubuntu                         latest              5ba9dab47459        2 weeks ago         188.3 MB
ubuntu                         trusty              5ba9dab47459        2 weeks ago         188.3 MB
nginx                          latest              4b5657a3d162        2 weeks ago         91.66 MB
tutum.co/kevinsimper/mynginx   latest              4b5657a3d162        2 weeks ago         91.66 MB
hello-world                    latest              e45a5af57b00        6 weeks ago         910 B
```
--
```
$ docker login tutum.co
Username: kevinsimper
Password:
Email: kevin.simper@gmail.com
Login Succeeded

$ docker push tutum.co/kevinsimper/mynginx
The push refers to a repository [tutum.co/kevinsimper/mynginx] (len: 1)
Sending image list
Pushing repository tutum.co/kevinsimper/mynginx (1 tags)
Image d9ee0b8eeda7 already pushed, skipping
Image 3225d58a895a already pushed, skipping
Image 30d39e59ffe2 already pushed, skipping
Image 511136ea3c5a already pushed, skipping
Image c90d655b99b2 already pushed, skipping
Image 224fea58b6cc already pushed, skipping
Image f22d05624ebc already pushed, skipping
Image ef9d79968cc6 already pushed, skipping
Image 117696d1464e already pushed, skipping
Image 2ebe3e67fb76 already pushed, skipping
Image ad82b43d6595 already pushed, skipping
Image e90c322c3a1c already pushed, skipping
Image 4b5657a3d162 already pushed, skipping
Pushing tag for rev [4b5657a3d162] on {https://tutum.co/v1/repositories/kevinsimper/mynginx/tags/latest}
```
--
![tutum-4](tutum-4.png)
--
![tutum-4](tutum-5.png)
--
![tutum-4](tutum-6.png)
--
![tutum-4](tutum-7.png)
--
![tutum-4](tutum-8.png)
--
![tutum-4](tutum-9.png)
--
![tutum-4](tutum-10.png)
--
![tutum-4](tutum-11.png)
--
![tutum-4](tutum-12.png)
--
# Load Balancing?
## Tutum Haproxy 
https://github.com/tutumcloud/tutum-docker-clusterproxy
--
![tutum-4](tutum-13.png)
--
![tutum-4](tutum-14.png)
--
![tutum-4](tutum-15.png)
--
![tutum-4](tutum-16.png)
--
![tutum-4](tutum-17.png)
--
# tutum cli
## works, but implicit
--
# What else is there?
--
# docker machine & docker swarm
--
## First we need some machines
# Docker machine
--
## download and install 
## docker machine to your path
--
```bash
$ docker-machine create -d digitalocean --digitalocean-access-token ${DIGITAL_OCEAN} node01
INFO[0000] Creating SSH key...
INFO[0001] Creating Digital Ocean droplet...
INFO[0005] Waiting for SSH...
+ [ https://get.docker.com/ = https://get.docker.com/ ]
+ sh -c apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
gpg: requesting key A88D21E9 from hkp server keyserver.ubuntu.com
gpg: key A88D21E9: public key "Docker Release Tool (releasedocker) <docker@dotcloud.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
+ sh -c echo deb https://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list
+ sh -c sleep 3; apt-get update; apt-get install -y -q lxc-docker
+ sh -c docker version
INFO[0132] "node01" has been created and is now the active machine
INFO[0132] To connect: docker $(docker-machine config node01) ps
kevinsimper$ docker $(docker-machine config node01) ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
--
```
$ docker-machine ls
NAME     ACTIVE   DRIVER         STATE     URL
node01   *        digitalocean   Running   tcp://104.131.178.98:2376
```
--
```
$ docker-machine ssh
Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 3.13.0-43-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Tue Feb 17 03:47:41 EST 2015

  System load:  0.01               Processes:              68
  Usage of /:   11.4% of 19.56GB   Users logged in:        0
  Memory usage: 13%                IP address for eth0:    104.131.178.98
  Swap usage:   0%                 IP address for docker0: 172.17.42.1

  Graph this data and manage this system at:
    https://landscape.canonical.com/

Last login: Tue Feb 17 03:47:41 2015 from 62-135-247-12-dynamic.dk.customer.tdc.net
root@node01:~#
```
--
## works like coreos _etcd_
```
$ docker run --rm swarm create
Unable to find image 'swarm:latest' locally
a8bbe4db330c: Pull complete
9dfb95669acc: Pull complete
0b3950daf974: Pull complete
633f3d9a9685: Pull complete
bba5f98a0414: Pull complete
defbc1ab4462: Pull complete
92d78d321ff2: Pull complete
511136ea3c5a: Already exists
swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
Status: Downloaded newer image for swarm:latest
1759275a913132d137a80afd6a929836
```
--
```
docker run -d swarm join --addr=104.131.178.98:2375 token://1759275a913132d137a80afd6a929836

docker run -t -p 104.131.6.53::2375 swarm manage token://1759275a913132d137a80afd6a929836
```
```
docker -H tcp://<swarm_ip:swarm_port> info
```
--
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED                  STATUS              PORTS                           NODE        NAMES
92d78d321ff2        mynginx:latest      "nginx"             Less than a second ago   running             104.131.178.98:49178->80/tcp    node-01     mynginx
```
--
## rough edges
- takes time
- docker listening on network
- loadbalacing
--
# deis.io
## your own paas
--
## $ brew install deisctl
--
## coreos
* has cloudinit/userdata for all providers

```
$ gem install docl
$ docl authorize
$ docl upload_key deis ~/.ssh/deis.pub
$ # retrieve your SSH key's ID
$ docl keys
deis (id: 690051)
$ # retrieve the region name
$ docl regions --metadata --private-networking
Amsterdam 2 (ams2)
Amsterdam 3 (ams3)
London 1 (lon1)
New York 3 (nyc3)
Singapore 1 (sgp1)
$ ./contrib/digitalocean/provision-do-cluster.sh ams2 690051 4GB
```
--
## $ deisctl install platform
--
## install _deis_ terminal
## curl -sSL http://deis.io/deis-cli/install.sh | sh
--
## deis register http://my-coreos-cluster.com
--
## mimicking heroku functionality and cli

## $ deis apps:create
## $ deis apps:list
## $ git push deis master
--
```
$ git push deis master
Counting objects: 13, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (13/13), done.
Writing objects: 100% (13/13), 1.99 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
-----> Building Docker image

```
--
```
asd
```