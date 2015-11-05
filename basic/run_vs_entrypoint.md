CMD 與 ENTRYPOINT 的差別
========================

下列內容來源：[論docker中 CMD 與 ENTRYPOINT 的區別](http://www.programfish.com/blog/?p=151)

Dockerfile裏有 CMD 與 ENTRYPOINT 兩個功能咋看起來很相似的指令，開始的時候覺得兩個互用沒什麽所謂，但其實並非如此。

CMD
---

> The main purpose of a CMD is to provide defaults for an executing container.

CMD在容器運行的時候提供一些命令及參數，用法如下：

### type 1

`CMD ["executable","param1","param2"](exec form, this is the preferred form)`

運行一個可執行的文件並提供參數。

### type 2

`CMD ["param1","param2"](as default parameters to ENTRYPOINT)`

為ENTRYPOINT指定參數。

### type 3

`CMD command param1 param2 (shell form)`

(shell form)：是以”/bin/sh -c”的方法執行的命令。

ENTRYPOINT 　
-------------

字面意思是進入點，而它的功能也恰如其意。

> An ENTRYPOINT allows you to configure a container that will run as an executable.

它可以讓你的容器功能表現得像一個可執行程序一樣。

容器功能表現得像一個可執行程序一樣，這是什麽意思呢？

使用下面的 ENTRYPOINT 建置 image：

`ENTRYPOINT ["/bin/echo"]`

docker build 出來的鏡像以後的容器功能就像一個 /bin/echo

比如我 build 出來的鏡像名稱叫 imageecho，那麽我可以這樣用它：

docker run -it imageecho “this is a test”

`ENTRYPOINT ["/bin/cat"]`

build 出來的鏡像你可以這樣運行 (假設名為 st)：

`docker run -it st /etc/fstab`

這樣相當： /bin/cat /etc/fstab 這個命令的作用。運行之後就輸出 `/etc/fstab` 裏的內容。

ENTRYPOINT有兩種寫法：

### type 1

ENTRYPOINT ["executable", "param1", "param2"](the preferred exec form)

### type 2

ENTRYPOINT command param1 param2 (shell form)

你也可以在 docker run 命令時使用 --entrypoint 指定（但是只能用 type 1）。

CMD 可以為 ENTRYPOINT 提供參數，ENTRYPOINT 本身也可以包含參數， 但是你可以把那些可能需要變動的參數寫到 CMD 裏而把那些不需要變動的參數寫到 ENTRYPOINT 裏面例如：

```
FROM ubuntu:14.10
ENTRYPOINT ["top", "-b"]
CMD ["-c"]
```

把可能需要變動的參數寫到 CMD 裏面。然後你可以在 docker run 裏指定參數，這樣 CMD 裏的參數(這裏是 -c) 就會被覆蓋掉而 ENTRYPOINT 裏的不被覆蓋。
