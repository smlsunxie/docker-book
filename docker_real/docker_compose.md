建立 docker-compose
-------------------

初始設定
--------

```
build:
  container_name: nodejsSample_build
  command: "/bin/bash -l -c 'npm i && node_modules/.bin/grunt buildDev'"

test:
  container_name: nodejsSample_test
  command: "/bin/bash -l -c 'node_modules/.bin/grunt test'"

web:
  container_name: nodejsSample
  command: "/bin/bash -l -c 'npm start'"

mysql:
  container_name: nodejsSample_mysql
  image: dgraziotin/mysql

  environment:
    MYSQL_ADMIN_PASS: "root"
    MYSQL_USER_NAME: "nodejsSample"
    MYSQL_USER_DB: "nodejsSample"
    MYSQL_USER_PASS: "nodejsSample"
    CREATE_MYSQL_BASIC_USER_AND_DB: "true"

```

ports
-----

### 考慮開發過程需要 debug

比如 mysql 需要讓 host 可以連進去查看資料

link
----

專案若需要跟 mysql 互動，則需要進行連結

external_links
--------------

若一個 mysql docker 可能會跟不同 docker 共用

則可以使用 external_links 在使用 docker-compose 時需要額外啟動。

需要注意的是，external_links 所指定的 docker 必須在啟動目標 docker 前就啟動完畢，相關 env 將會在 container 建立時就進行載入，假若是在 contaniner 建立後才補上參數，或是啟動 container 前 external_links 所指定的 docker 尚未啟動，將無法正確使用，需要將 contaniner rm 之後再行啟動方能正常運作。
