install maven use dockerfile
============================

建立 Dockerfile
---------------

```
FROM java:7

RUN sudo apt-get update
RUN sudo apt-get install -y maven
```

進行 Dockerfile 建置
--------------------

```
docker build -t agileworks/java7_maven_dockerfile .
```

取得 sample 專案
----------------

```
mkdir ~/workspace && cd ~/workspace
git clone https://github.com/agileworks-tw/spring-boot-sample.git
cd spring-boot-sample/
```

透過 Docker 運行專案
--------------------

```
docker run --rm \
-v $HOME/.m2:/root/.m2 \
-v `pwd`:/app \
-w /app \
-p 8000:8000 \
agileworks/java7_maven_dockerfile \
/bin/bash -c 'cd data-rest && mvn spring-boot:run'
```
