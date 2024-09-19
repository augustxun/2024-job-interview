## Linux

### 1. 目录相关的 Linux 命令？

[JavaGuide-Linux命令](https://javaguide.cn/cs-basics/operating-system/linux-intro.html#linux-常用命令)

### 2. Linux中查看某个进程占用率较高的话，用哪个命令？

top或htop可以动态显示系统中各个进程的资源占用情况，CPU使用率，内存利用率。

top -o %CPU

### 3. 如何查看系统日志文件？

### 4. 说说 grep 命令？

grep 是Linux 系统中常用的命令行工具，用于在文件中搜索符合特定模式的文本行。

基本用法：

![img](https://cdn.nlark.com/yuque/0/2024/png/42782944/1723732622054-3de41491-7788-43ce-a876-eb5b230d7972.png)

### 5. 查看Nginx错误日志的linux命令？

![img](https://cdn.nlark.com/yuque/0/2024/png/42782944/1723732891369-f110fa32-74c1-4ef2-80a3-8dbe44c35c00.png)

### 6. 筛选包含500这种错误关键字的命令？

![img](https://cdn.nlark.com/yuque/0/2024/png/42782944/1723732732930-bc27f717-3788-4898-9507-7d05af4bf552.png)

### 7. lsof指令全称是什么?

"list open files"

列举被进程打开的文件的信息

### 8. Linux 如何通过页实现内存管理的？



### 9. 如何将虚拟地址转换成物理地址？

------

## Maven

### package 和 install 的区别？

package 和 install 都是 Maven 构建生命周期里的一个阶段。

package 阶段是将编译过的代码和资源文件打包成 JAR、WAR。
install 阶段是将 JAR 包安装到本地 Maven 仓库中，这使得其他项目可以依赖和使用它



------

## Git

### 说说你知道的 git 命令？

![img](https://cdn.nlark.com/yuque/0/2024/png/42782944/1723773768588-a6bb56f1-cb17-4ccc-bc97-93f839dfccce.png)

### Git 什么情况下会发生冲突？如何解决？

可能发生冲突的场景包括：

1. 不同分支在同一文件的同一行上有不同的更改，Git 无法确定保留哪一行更改。
2. 文件在一个分支被删除，在另一个分支被修改。
3. 文件在两个分支中都被重命名，Git 无法确定使用哪个新的文件名。

解决冲突的过程：

1. Git 自动检测冲突，并在进行合并时提示文件未合并
2. git status 命令查看冲突的文件
3. 打开冲突文件，手动去编辑冲突的部分
4. 用 git add 命令标记冲突已经解决
5. 解决完冲突后完成合并、变基