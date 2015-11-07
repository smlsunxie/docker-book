將 docker-compose 之 web 轉換為 dockerfile
------------------------------------------

原 docker-compose 設定
----------------------

```
web:
  container_name: ghost_env
  image: trunk/ghost_env
  command: "/bin/bash -l -c 'npm start --production'"
  ports:
    - 2368:2368
  working_dir: /ghost
  volumes:
    - ./:/ghost
  links:
    - mysql
  restart: always
```

轉為 dockerfile for production
------------------------------

```
FROM trunk/ghost_env
COPY ./ /ghost
WORKDIR /ghost

ENV NODE_ENV "production"
ENV DOMAIN_HOST "http://localhost:2368"

EXPOSE 2368

CMD /bin/bash -l -c 'npm start --production'
```

啟動 mysql
----------

### use docker-compose

```
docker-compose up -d mysql
```

### user docker

```
docker run -d \
--name ghost_mysql \
-v ghosta_database:/var/lib/mysql/ \
-p 3306:3306 \
-e MYSQL_ADMIN_PASS=root \
-e MYSQL_USER_NAME=ghost \
-e MYSQL_USER_DB=ghost \
-e MYSQL_USER_PASS=ghost \
-e CREATE_MYSQL_BASIC_USER_AND_DB=true \
dgraziotin/mysql

```

啟動 ghost
----------

```
docker run -d \
-p 2368:2368 \
--name ghost_prod \
--link ghost_mysql \
trunk/ghost

```
