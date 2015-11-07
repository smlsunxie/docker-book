注意事項
========

host 使用
---------

若沒有使用 host binding，建議 host 都用 `0.0.0.0`

如：

```
server: {
  host: '0.0.0.0',
  port: '2368'
}
```

command 的使用
--------------

通常若你需要登入到 docker 內部進行操作，且希望 `.bashrc` 內的設定可以有效

比如使用 nvm 或是 rvm 在下指令時可以用：

```
/bin/bash -l
```

進行 docker login

若在建置 dockerfile 時就可以使用

```
/bin/bash -l -c 'nvm --version'
```

來執行相關程序
