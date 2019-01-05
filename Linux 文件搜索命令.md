<font color=#0099ff size=7 face="黑体">目录</font>

[TOC]

在 Linux 中搜索文件有几个常用的命令： find、locate、which、whereis、grep 等。首先来介绍一下 find，find 命令的语法是：

find [搜索路径] [匹配条件]

find 命令的选项非常多，常用的有这么几个：

# 1. find

## 1.1 根据名称查找

### 1.1.1 区分大小写 -name

```shell
find /etc -name init
```

该命令的功能是在 etc 目录下查找 文件名为 init 的文件。和 Windows 不同的是，这种查找方式是完全匹配，也就是只匹配文件名为 init 的文件，而不是只要文件名中含有 init 就会被匹配到。

如果想模糊匹配，需要使用通配符：`find /etc -name *init*`

如果想查找以 init 开头的文件，可以这样查找：`find /etc -name init*`

如果想查找以 init 开头，并且后面紧跟 3 个字符的文件，可以这样查找：`find /etc -name init???`

这个选项只会查找跟 init 有关的文件，而不会查找跟 INIT 有关的文件。如果想在查找时不区分大小写。要用下面这个选项。
### 1.1.2 不区分大小写 -iname

```shell
find /etc -iname init
```
这个命令的功能和上面 1.1 中的命令功能一样，唯一的区别就是在查找时不区分文件名中字母的大小写。
## 1.2 根据文件大小查找 -size

```shell
find / -size +204800
```
这条命令的功能是，在根目录下查找大于 100MB 的文件。因为它的单位是数据块，而一个数据块是 0.5KB，所以 100MB 是 204800 个数据块，所以要写 204800。
若把「+」换成「-」，就是查找小于 100MB 的文件；若换成「=」，就是查找等于 100MB 的文件。

## 1.3 根据所有者查找 -user

```shell
find /home -user root
```
这条命令的功能是，在 home 目录下查找所有者为 root 用户的文件
## 1.4 根据所有组查找 -group

```shell
find /home -group root
```
这条命令的功能是，在 home 目录下查找所有组为 group 的文件。
## 1.5 根据时间查找

```shell
find /etc -cmin -5
```
这条命令的功能是，在 /etc 下查找 5 分钟内被修改过属性的文件和目录。如果是超过 5 分钟，就写「+5」，也就是：find /etc -cmin +5
下面是几个类似的选项：

 - -amin 访问时间（a - access）
 - -cmin 文件属性（c - change）
 - -mmin 文件内容 （m - modify）

## 1.6 根据文件类型查找 -type

```shell
find /etc -type f
```
这条命令的功能是查找 etc 目录下的所有文件。
若把 f 换成 d，则是查找 etc 目录下的所有目录；
若把 f 换成 l（小写字母），则是查找 etc 目录下的所有软链接文件。

## 1.7 连接符 -a、-o

若查找的条件有多个，可通过连接符将不同的选项连接起来。其中，「-a」表示 and，即通过「-a」连接的多个条件要同时满足，「-o」表示 or，通过「-o」连接的多个条件只满足其中一个即可。

例如，在 etc 目录下查找大于 80 MB，小于 100MB 的文件：
```shell
find /etc -size +163840 -a -size -204800
```
再例如，在 etc 目录下查找以 init 开头的文件。*注意：只是查找文件，不包含目录。*
```shell
find /etc -name init* -a -type f
```
![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20%E6%96%87%E4%BB%B6%E6%90%9C%E7%B4%A2%E5%91%BD%E4%BB%A4%20%E2%80%94%E2%80%94%20find/2019-01-03_201750.png)

## 1.8 对搜索结果执行操作

### 1.8.1 -exec

```shell
find /etc -name inittab -exec ls -l {} \;
```
这条命令的功能是查找 etc 目录下的名为 inittab 的文件，然后查看它的详细信息。「-exec」后面跟要对搜索结果做的操作。
当然也可以和上面的连接符一起用，比如：

```shell
 find /etc -size +163840 -a -size -204800 -exec ls -l {} \;
```
这条命令的作用是在 etc 目录下查找大于 80 MB，小于 100MB 的文件，然后列出他们的详细信息。
### 1.8.2 -ok

如果把「-exec」换成「-ok」，那么在对每个文件执行操作之前，都会询问你是否要执行。

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20%E6%96%87%E4%BB%B6%E6%90%9C%E7%B4%A2%E5%91%BD%E4%BB%A4%20%E2%80%94%E2%80%94%20find/2019-01-03_223738.png)

## 1.9 根据文件的 i 节点查找 -num

```shell
find . -inum 142722
```
查找当前目录下 i 节点为 142722 的文件。「.」表示当前文件。

**PS：因为硬链接是不能跨分区的，利用这个命令可以找到一个文件的硬链接。**

# 2. locate
find 命令已经如此强大，为什么还需要 locate呢？因为 find 命令是通过遍历磁盘来查找文件，故需要占用非常多的系统资源，搜索速度相对较慢，而 locate 命令是在 Linux 系统内的一个文件数据库中查找你所需要的文件，而不是直接遍历磁盘，所以查找速度比 find 快很多。locate 最常见的一种使用方式是在 locate 命令后面直接跟一个文件名，就像这样：
```shell
locate init
```
这条命令会把所有包含 init 的文件或目录都搜索出来，注意，locate 后面直接跟文件名的话，是区分大小写的，要想让它在查找过程中不区分大小写，可以加一个选项「-i」，就像下面这样：

```shell
locate -i init
```



locate 命令虽然搜索速度快，但是如果创建一个文件后立马搜索这个文件会搜不到，因为这个文件还没被更新到文件数据库里去。这个时候可以手动更新一下数据库，用下面这个命令：
```shell
updatedb
```
然后再去搜索刚才创建的文件，就可以搜的到了。

## 2.1 locate 的缺点

在使用 locate 的时候，要注意一个问题，就是 locate 是找不到存放在 tmp 目录下的文件的，因为 tmp 目录不在 locate 的查找范围之内。

# 3. which

which 是查找命令文件的命令。比如说我想查找 ls 这个命令在哪个目录下，就可以使用 which 来进行查找：

```shell
which ls
```

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20%E6%96%87%E4%BB%B6%E6%90%9C%E7%B4%A2%E5%91%BD%E4%BB%A4%20%E2%80%94%E2%80%94%20find/2019-01-05_153524.png)

# 4. whereis

whereis 和 which 的功能差不多，用法也是后面跟一个要查找的命令，都是用来查找命令文件的。查找结果除了显示命令所在的命令以外，where 不会列出要查找的命令的别名相关的信息，而是会列出这个命令的帮助文档所在的目录。

看一下具体的例子：

```shell
whereis rm
```

下面是命令运行结果：

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20%E6%96%87%E4%BB%B6%E6%90%9C%E7%B4%A2%E5%91%BD%E4%BB%A4%20%E2%80%94%E2%80%94%20find/2019-01-05_155034.png)

# 5. grep

## 5.1 直接查找

grep 这个命令比较特殊，它不像前几个命令一样是查找某一个文件，而是查找文件内容。用法就是 grep 命令后面跟关键词，然后跟要查找的文件。比如：

```shell
grep multiuser /etc/inittab
```

这个命令的作用是在 etc 目录下的 inittab 文件中查找关键字 multiuser，然后将该关键字所在的行显示出来。下面是命令运行结果：

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20%E6%96%87%E4%BB%B6%E6%90%9C%E7%B4%A2%E5%91%BD%E4%BB%A4%20%E2%80%94%E2%80%94%20find/2019-01-05_160529.png)

## 5.2 不区分大小写 -i

上面那种查找方式是严格区分大小写的，如果不想区分大小写，那么可以在 grep 后面加一个选项「-i」，比如：

```shell
grep -i multiuser /etc/inittab
```

这个命令与上面那个命令作用类似，只是在查找关键词时不区分大小写了。该命令运行结果如下：

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20%E6%96%87%E4%BB%B6%E6%90%9C%E7%B4%A2%E5%91%BD%E4%BB%A4%20%E2%80%94%E2%80%94%20find/2019-01-05_162002.png)

可以看到，该命令不仅搜索到了 multiuser 所在的行，还搜索到了 Multiuser 所在的行。

## 5.3 排除指定字符串 -v

在 Linux 的配置文件中，「#」代表注释，如果我想看配置文件的内容，但是不想看注释，就可以在搜索文件内容时排除「#」所在的行。就可以这样做：`grep -v # /etc/inittab`

但是有的注释并不是单独一行，而是写在配置语句的后面，这样的话单纯地排除「#」所在的行就会把配置语句也排除掉，造成误伤。也就是说我们只能排除掉以「#」开头的行，这种情况下应该这样写：

```shell
grep -v ^# /etc/inittab
```

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20%E6%96%87%E4%BB%B6%E6%90%9C%E7%B4%A2%E5%91%BD%E4%BB%A4%20%E2%80%94%E2%80%94%20find/2019-01-05_164314.png)

以上就是为大家介绍的 Linux 里面常用的文件搜索命令，希望能够对大家有所帮助。

欢迎关注我的微信公众号，扫描下方二维码或微信搜索：AProgrammer，就可以找到我，我会持续为你分享 IT 技术。
![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/%E5%BE%AE%E4%BF%A1%E5%85%AC%E4%BC%97%E5%8F%B7%E4%BA%8C%E7%BB%B4%E7%A0%81.jpg)

