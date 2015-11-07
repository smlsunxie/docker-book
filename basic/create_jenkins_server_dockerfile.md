install Jenkins use dockerfile
==============================

Jnekins 安裝指令如下：

```
sudo apt-get install -y wget
wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install -y jenkins
```

建立 Dockerfile
---------------

```
FROM ubuntu:14.04

RUN sudo apt-get install -y wget
RUN wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
RUN sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
RUN sudo apt-get update
RUN sudo apt-get install -y jenkins

EXPOSE 8080
CMD su jenkins -c 'java -jar /usr/share/jenkins/jenkins.war'
```

其中

> Second, we are deprecating the advanced “<public>:<private>” syntax of the EXPOSE build instruction. This special syntax allowed the Dockerfile to specify in advance that the exposed port should be published on a certain port on all host interfaces. We have found that this hurts separation of concerns between dev and ops, by restricting in advance the system administrator’s ability to configure redirects on the host. The regular “EXPOSE <private>” syntax is not affected.

這邊有踩到地雷：

`EXPOSE 8080:8080`

在 Dockerfile 已經不能這樣進行，所以 Dockerfile 內的 EXPOSE 適合在 `container link` 使用，之後會講到

進行建置 image
--------------

```
docker build -t jenkins_server_file .
```

運行 docker
-----------

```
docker run --rm --name jenkins_server_file -p 80:8080 jenkins_server_file .
```
