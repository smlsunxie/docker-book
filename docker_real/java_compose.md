運行專案使用 docker-compose
===========================

建立 docker-compose.yml
-----------------------

```
server:
  container_name: spring_boot_run
  image: agileworks/java7_maven_dockerfile
  command: "/bin/bash -c 'cd data-rest && mvn spring-boot:run'"
  working_dir: /app
  volumes:
    - $HOME/.m2:/root/.m2
    - ./:/app
  ports:
   - "8000:8000"

```

運行 docker-compose
-------------------

```
docker-compose up server
```
