install Jenkins
===============

延續上一章節中的 ubuntu_base

Jnekins 安裝指令如下：

```
sudo apt-get install -y wget
wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install -y jenkins
```

我們可以用

```
docker exec -it ubuntu_base /bin/bash -l
```

進去後執行上述安裝指令

安裝完成後，將安裝完成的 container commit 為新的 image

```
docker commit ubuntu_base jenkins
```

如此，我們就有新的 images 可以執行，接著我們再將新的 image: jenkins 啟動

```
docker run -i -d -p 8080:8080 --name jenkins jenkins su jenkins -c "java -jar /usr/share/jenkins/jenkins.war"
```

接著，我們可以再一次 commit 目前的狀態

```
docker commit jenkins jenkins_server
```

最後，我們只要執行下面指令就可以啟動 jenkins_server

```
docker run -p 8080:8080 jenkins_server
```

本章節純粹使用 docker 的 command 完成一個 jenkins server 的建置，讓我們來使用 dockerfile 進行 jenkins server 的建立
