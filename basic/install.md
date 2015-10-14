安裝 docker
===========

ubuntu
------

```
sudo apt-key adv --keyserver hkp://pgp.mit.edu:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
sudo add-apt-repository "deb https://apt.dockerproject.org/repo ubuntu-$(lsb_release -s -c) main"
sudo apt-get update
sudo apt-get install docker-engine
sudo docker run hello-world

```

OSX
---

docker-tools: https://www.docker.com/toolbox
