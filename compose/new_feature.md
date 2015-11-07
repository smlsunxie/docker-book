新特性
------

### extends

可以繼承於某一個特定的 docker-compose-yml

ex:

```
extends:
  file: common.yml
  service: webapp
```

### Variable substitution

與 extends 類似，不過可以透過 env 變數進行相關參數替換

ex:

```
db:
  image: "postgres:${POSTGRES_VERSION}"
```

### volumes_from

把掛在在特定 Container 的 volumes 也掛載進入特定位置

ex:

```
volumes_from:
 - service_name
 - container_name
 - service_name:rw
```
