如果有一个 Linux 命令使用频度排行榜的话，「ls」肯定榜上有名，而且说不定还是第一名。ls 是目录处理命令，是英文单词 list 的缩写，它的功能是显示某一目录下的文件或子目录。

ls 命令的使用格式是：ls [选项] \[目录名]。中括号代表可选。

# 1. 小试牛刀

ls 最简单的用法就是直接在某一目录下输入 ls，然后回车，会显示当前目录下的所有文件和子目录（不包括隐藏文件）。如下图，

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20ls%20%E5%91%BD%E4%BB%A4%E8%AF%A6%E8%A7%A3/2019-01-07_201013.png)

如果想查看别的目录所含的文件，可以在 ls 后面跟目录名，比如查看根目录下的所有文件，如下图：

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20ls%20%E5%91%BD%E4%BB%A4%E8%AF%A6%E8%A7%A3/2019-01-07_220502.png)

若想显示当前目录下包括隐藏文件在内的所有文件和子目录，可以加一个选项「-a」，或「--all」，如下图：

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20ls%20%E5%91%BD%E4%BB%A4%E8%AF%A6%E8%A7%A3/2019-01-07_203922.png)

同理，若想显示根目录下包括隐藏文件在内的所有文件和子目录，命令为：「ls -a /」

上图中，以「.」开头的文件就是隐藏文件，隐藏文件的作用并不 是要藏起来让用户找不到，而是为了告诉用户，这是系统文件，除非你确定要对该文件进行操作，否则不要轻易动隐藏文件，这就是为什么用户可以很方便地用「ls -a」这条命令查看隐藏文件。如果你想创建一个隐藏文件，和上图一样，只要给这个文件起一个以「.」开头的文件名就可以了。

# 2. 以长格式显示文件信息

「长」的英文单词是「long」，所以，以长格式显示文件信息的命令就是「ls -l」，比如，查看 root 目录下的文件的长格式信息，用这个命令：`ls -l /root`，然后回车，可以看到这样的信息：

![](https://raw.githubusercontent.com/YoungYo/Linux---Notes/master/images/Linux%20ls%20%E5%91%BD%E4%BB%A4%E8%AF%A6%E8%A7%A3/2019-01-07_222705.png)

下面详细解释一下显示的这些信息，