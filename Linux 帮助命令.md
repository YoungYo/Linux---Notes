在 Linux 中，如果想查看某个命令或者配置文件是干什么的，那么可以用 man 命令来查看某个命令或配置文件的详细信息。

# 1. 查看命令

比如：

```shell
man ls
```

这条命令就是查看 ls 的帮助文档，man 并不是指男人，而是 manual 的缩写。

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%E5%B8%AE%E5%8A%A9%E5%91%BD%E4%BB%A4/2019-01-05_185219.png)

上图就是`man ls`执行后的结果，红框中的部分是对 ls 命令的简短描述。不论用 man 查看什么命令，第一行都会显示这个命令的一个简短描述。

如果想查看某个命令的某个选项，该怎么做呢？假设我想查看 ls 命令的 -l 这个选项，那么在执行完 man ls 后，可以输入`/-l`，然后按下回车，就可以在 ls 的帮助文档中匹配到「-l」这个字符串，帮助文档中可能会有很多个「-l」，如果当前匹配到的字符串不是你想要的那个，还可以按 n 查找下一个。

# 2. 查看配置文件

查看配置文件和查看命令是一样的，直接在 man 后面跟配置文件的名字就可以。