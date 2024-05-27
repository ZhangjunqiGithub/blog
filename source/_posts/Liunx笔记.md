---
title: Linux笔记
categories: Linux笔记
tags: Linux笔记
date: 2022/9/27
cover: https://ts1.cn.mm.bing.net/th/id/R-C.cfadf725641b671120d544f2f2e26007?rik=csP98zUzaBFdCQ&riu=http%3a%2f%2fup.ipaddesk.com%2fpic%2f71%2f1e%2f52%2f711e527571de79a25518e4c655ff447a.jpg&ehk=Xcuwedcp3RlJM45767D3D852%2bNdZIxYYEt7iH340LZ0%3d&risl=&pid=ImgRaw&r=0
---

### 1、字符串的测试

```bash
str1=aaa
str2=1234
test $str1 = $str2 或 test [ $str1 = $str2 ] (注意空格)---用于判断str1和str2两个字符串是否相等
echo $? ---输出上一条语句的运行结果
执行结果
=> 1 (表示str1不等于str2)如果str1等于str2则输出为0
```

### 2、数值的测试

**-eq**  ---  **=**

**-lt**  ---  **<**

**-gt**  ---  **>**

**-le**  ---  **<=**

**-ge**  ---  **>=**

**-ne**  --- **!=**

```bash
v1=1000
v2=2000
test $v1 -eq $v2 (判断v1是否等于v2)(注意空格)
echo $?
执行结果
=> 1
```

### 3、文件的测试

```bash
test -d Music  ---判断Music是否是文件夹
test -f ex*  ---判断以ex开头的文件是否为普通文件
test -x Music  ---判断Music文件是否为可执行文件
```

### 4、算数运算（表达式求值）

```bash
v1=1
v1=`expr $v1 + 1`  ( * 需转义 \* )(注意空格)
echo $v1 
=> 2
```

```bash
a=2
b=3
echo "result=$((a+b))"
执行结果
=> result=5
```

```bash
let "a=3" "b=5"
let c=a*b
echo $c
执行结果
=> 15
```

### 5、挂载U盘

插入U盘找到多出的文件夹如/dev/sdb1

```bash
sudo mount /dev/sdb1 ./usb  ---(挂载U盘)
```

```bash
sudo unmount ./usb  ---(卸载)
```

```bash
sudo fdisk -l  ---(显示设备)
```

### 6、shell脚本编写

```bash
touch 文件名  ---(建文件)
vi 文件名  ---(编辑，按 i 进入insert可编辑状态，按ESC退出可编辑状态，在非编辑状态输入:wq保存并退出，:q!强制退出)
cat 文件名  ---(查看文件)
ls -l 文件名  ---(列出文件详细信息)
```

### 7、执行shell脚本

> 1. ```bash
>    chmod u+x 文件名  ---(加权限)
>    执行方式如下：
>    （1）./ 文件名  ---(需要加权限)
>    （2）sh 文件名  ---(不需要加权限)
>    （3）. 文件名  ---(需要加权限)
>    ```

### 8、shell变量

```bash
var1=123
echo $var1
执行结果
=>123
var1="hello world"
echo $var1
执行结果
=>hello world

unset var1  ---(删除变量)

位置变量（0~9）
./test  7   77   777   7777
   $0   $1  $2   $3    $4
命令行参数
>9时，shift[n]

注意：
read var1 var2
输入 aaa bbb
如果编写一个脚本文件export1用来回显var1和var2的值
./export1和sh export1无法回显输出var1和var2的值
原因：会启动一个新的shell(其中不含var1和var2)
.export1可以回显
原因：启动旧的shell(其中含有var1和var2)

但是：如果之心export var1后，则./export1和sh export1在新的shell中将可以读取var1的值，但是不能读取var2的值
```

### 9、if + for + while

```bash
if 条件
then
	...
elif 条件
then
	...
else
	...
fi
```

```bash
for var in hello world 123
do
	echo $var
done
exit 0
=> hello
=> world
=> 123

输出所有以.conf结尾的文件
for file in $(ls *.conf)
do
	echo file
done
exit 0

补充：echo -n  ---(表示该语句执行完毕后不换行)
```

### 课堂脚本代码：

```bash
#! /bin/bash
echo $0
echo $1,$2,$3,$4,$5,$6,$7,$8,$9
shift
echo $1,$2,$3,$4,$5,$6,$7,$8,$9
shift
echo $1,$2,$3,$4,$5,$6,$7,$8,$9
shift 2
echo $1,$2,$3,$4,$5,$6,$7,$8,$9
shift 2
echo $1,$2,$3,$pt 
end
```



```bash
#! /bin/bash
count=0
for var in $(ls *)
do
echo $var
count=`expr $count + 1`
done
echo "总的文件数为:$count"
exit 0
```



```bash
#! /bin/bash
echo "abc is the user's name?please answer yes or no"
read name
if [ "$name" = "yes" ](注意空格)
then
	echo "hello abc!"
elif [ "$name" = "no" ]
then
	echo "abc isn't the user's name."
else
	echo "input error"
fi
exit 0
```



```bash
#! /bin/bash
touch file1
rm -f file2
if [ -f file1 ] && echo "hello" && [ -f file2 ] && echo "world"
then
	echo "in if"
else
	echo "in else"
fi
exit 0
```

#### shell中case的使用：

```bash
#! /bin/bash
echo "please enter the number of the week:"
read number
case $number in
1)echo "Monday";;
2)echo "Tuesday";;
3)echo "Wednsday";;
4)echo "Thursday";;
5)echo "Friday";;
6)echo "Saturday";;
7)echo "Sunday";;
*)echo "your enter must be in 1-7,";;
esac
```



```bash
#! /bin/bash
echo "abc is the user's name?please anwser yes or no:"
read name
case $name in
y|Y|yes|YES) echo "hello abc!";;
n*|N*) echo "abc isn't the user's name?";;
*) echo "sorry,your input isn't recognized.";;
esac
exit 0
```

```bash
#! /bin/bash
for var in $*
do
	echo "arguments $var in the command line."
done
exit 0

执行脚本文件
sh test aa bb cc dd
=> arguments aa in the command line.
=> arguments bb in the command line.
=> arguments cc in the command line.
=> arguments dd in the command line.
```

```bash
#! /bin/bash
echo -n "please enter password:"
read password
while [ "$password" != "123456" ]
do
	echo "sorry,try again"
read password
done
exit 0
```

```bash
#! /bin/bash
var=0
while read v
do
var=$((var+v))
echo $var
done
```

```bash
echo 10 > f1
echo 20 >> f1
cat f1
=>10
=>20
```

```bash
输入重定向
#! /bin/bash
while read v1 v2
do
echo $v1---$v2
done < f1(只重定向上面最近一次输入)
如果脚本文件没有写重定向代码，可在执行脚本时： ./test < f1 ；输入重定向f1为存储数据的文件
```

```c
#include<fcntl.h>
main()
{
    int fd;
    int num;
    char buf[20];
    fd = open("f1", O_RDONLY);
    if(fd == -1)
    {
        perror("open");
        exit(1);
    }
    while((num = read(fd, buf, 5)) > 0)
    {
        if(write(1, buf, num) < num)
            printf("write 1 less than should\n");
        printf("\n");
    }
    close(fd);
}
```

#### 将从s1开始的5个字符和从s2开始的5个字符交换位置：

```c
#include<fcntl.h>
#include<unistd.h>
main()
{
	int fd;
	int s1;
    int filesize;
	fd = open("f1", O_RDWR);
	if(fd == -1)
    {
        perror("open");
        exit(1);
    }
    filesize = lseek(fd, 0, SEEK_END)-1;
    scanf("%d", &s1);
    char *buf1 = (char *)malloc(s1);
    char *buf2 = (char *)malloc(s1);
    lseek(fd, 0, SEEK_SET);
    read(fd, buf1, s1);
    lseek(fd, filesize-s1, SEEK_SET);
    read(fd, buf2, s1);
    lseek(fd, 0, SEEK_SET);
    write(fd, buf2, s1);
    lseek(fd, filesize-s1, SEEK_SET);
    write(fd, buf1, s1);
    close(fd);
}
```

#### dup的使用：

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <string.h>
int main()
{
	int fd1 = -1, fd2 = -1;
	int num;
	char buf[20];
	scanf("%s", buf);
	printf("%s\n", buf);
	fd1 = open("f1", O_RDWR|O_TRUNC);
	if(fd1 == -1) {
		perror("open");
		exit(1);
	}
	fd2 = dup2(fd1, 1);
	if(fd2 < 0) {
		perror("dup2");
		exit(1);
	}
	num = wirte(fd1, buf, strlen(buf));
	printf("\nfd1 = %d, fd2 is %d\n",fd1, fd2);
	close(fd1);
	close(fd2);
	return 0;
}
```

#### 进程管理

```c
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
int main(void) {
    pid_t pid;
    int x = 0;
    pid = fork();
    if(pid == -1) {
        perror("fork");
        exit(-1);
    }
    else if(pid == 0) {
        printf("Child, x = %d\n", x);
        x = -10;
    }
    else {// id > 0
        printf("Parent process, x = %d\n", x);
        x = 100;
    }
    printf("x = %d\n", x);
    return 0;
}
```

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <string.h>
int main(int argc, char* argv[])
{
	pid_t pid;
	int fd;
	char * info = "hello world";
	char buf[64] = {0};
	int nBytes;
	
	fd = open("file", O_RDWR | O_CREAT, 0660);
	if(fd < 0) {
		perror("open file failed");
		exit(1);
	}
	printf("before fork\n");
	if((pid = fork()) < 0) {
		perror("fork error");
	}
	else if(pid == 0) {
        ......
		exit(0);
	}else {
		sleep(1);
        ......
		printf("%s\n", buf);
	}
	exit(0);
}
```

#### 多线程：

```c
#include<pthread.h>
#include<stdio.h>
int sum = 0;
void *printsum(void *arg) {
    while(1) {
        sleep((int)arg);
        printf("%d%d\n", sum, (int)arg);
    }
}
int main() {
    pthread_t tid1, tid2;
    int ret, x;
    ret = pthread_create(&tid1, NULL, printsum, (void *)3);
    ret = pthread_create(&tid2, NULL, printsum, (void *)5);
    if(ret != 0) {
        printf("Create pthread error!\n");
        return -1;
    }
    while(1) {
        scanf("%d", &x);
        printf("\n");
        sum += x;
    }
}
```

```c
#include<pthread.h>
#include<stdio.h>
int x = 0;
int y = 0;
void thread1(void) {
	printf("This is pthread1.the sentense 1\n");
	y = 7;
	sleep(1);
	printf("This is pthread1.the sentense 2\n");
	x += y;
}
void thread2(void) {
	printf("This is pthread2.the sentense 1\n");
	x = 4;
	sleep(1);
	printf("This is pthread2.the sentense 2\n");
	y += 8;
}
int main() {
	pthread_t id1, id2;
	pthread_create(&id1, NULL, (void *)thread1, NULL);
	pthread_create(&id2, NULL, (void *)thread2, NULL);
	pthread_join(id1, NULL);
	pthread_join(id2, NULL);
	printf("x = %d, y = %d\n", x, y);
}
```

```c
#include <unistd.h>
#include <signal.h>
#include <stdio.h>
#include <stdlib.h>
int sum = 0;
void handler(int x) {
	printf("sum=%d\n", sum);
}
int main() {
	int x;
	signal(SIGINT, handler);
	for( ; ; ) {
		scanf("%d", &x);
		sum += x;
	}
	return 0;
}
```

#### 信号的使用：

```c
#include <stdlib.h>
...
int sum = 0, x;
void handler(int i) {
    printf(..., sum);
    alarm(10);
}
void main() {
    signal(SIGALRM, handler);
    alarm(10);
    while(1) {
        scanf(..., &x);
        sum += x;
    }
}
```

```c
#include <stdio.h>
#include <signal.h>
void wakeup(int);
main() {
    printf("about to sleep for 4 seconds\n");
    signal(SIGALRM, wakeup);
    alarm(4);
    pause();
    printf("morning so soon?\n");
}
void wakeup(int signum) {
    printf("Alarm received from kernel\n");
}
```

#### 管道是使用：

```c
int main() {
    pid_t pid;
    char buf[32] = {0};
    int pips[2];
    pipe(pips);
    if((pid = fork()) == 0) {
        close(pips[0]);//关闭管道的只读端口
        wirte(pips[1], s, strlen(s));
    }else if(pid > 0) {
        close(pids[1]);//关闭管道的只写端口
        wait(NULL);
        read(pips[0], buf, sizeof(buf));
        printf("%s\n", buf);
    }
    return 0;
}
```

#### 两个进程通过管道传递数据：

1、

```c
//fifowr1.c
#include <unistd.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <fcntl.h>W
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int main() {
    int fd, rtn;
    char buf[100];
    if(mkfifo("file.fifo", 0644) == -1) {
        if(errno == EEXIST) {
            printf("file.fifo exists\n");
        }
    }
    fd = open("file.fifo", O_WRONLY);
    printf("fifo open write only\n");
    scanf("%s", buf);
    rtn = write(fd, buf, strlen(buf) + 1);
    close(fd);
    pause();
    retun 0
}
```

```c
//fiford1.c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int main(int argc, char * argv[]) {
    int fd, n;
    char info[100];
    fd = open("file.fifo", O_RDONLY);
    printf("fifo open ok\n");
    n = read(fd, info, sizeof(info));
    printf("fifo read %d: %s\n", n, info);
    close(fd);
    return 0;
}
```

2、

```c
//fifowr2.c
#include <stdlib.h>
#include <string.h>
int main() {
    int fd, rtn;
    int val[100];
    if(mkfifo("file.fifo", 0644) == -1) {
        if(errno == EEXIST) {
            printf("file.fifo exists\n");
        }
    }
    fd = open("file.fifo", O_WRONLY);
    printf("fifo open write only\n");
    for(int i = 0; i < 4; i++) {
        scanf("%d", &val[i]);
        rtn = write(fd, &val[i], sizeof(int));
    }
    printf("done\n");
    sleep(10);
    close(fd);
    return 0;
}
```

```c
//fiford2.c
#include <unistd.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <fcntl.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int main(int argc, char * argv[]) {
    int fd;
    int r, v[100], ix = 0;
    fd = open("file.fifo", O_RDONLY);
    printf("fifo open ok\n");
    do{
        r = read("file.fifo", O_RDONLY);
     	if(r > 0)
            printf("%d\n", v[ix]);
        ix++;
    } while( r > 0);
    close(fd);
    printf("fifo read vals : %d\n", ix - 1);
    return 0;
}
```

```c
/*1、设计C程序，从0开始以1秒间隔按递增顺序输出连续偶数序列，按下ctrl+c后则从最后输出的值开始，以1秒间隔输出递减连续偶数序列。再次按下ctrl+c后，从最后输出的值开始，以1秒间隔输出递增连续偶数序列
2、设计C程序collector和calculator,两个程序同时运行。collector循环接收键盘输入的整数，每次collector接收到一个整数，calculator计算collector已接收的所有整数的和并输出*/
1、
#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/stat.h>
int reg = 1;
void asc() {
    reg *= -1
}
int main() {
    int second = 0;
    if(signal(SIGINT, asc) != SIG_ERR) {
        while(1) {
            printf("%d\n", second);
            sleep(1);
            if(reg == 1) {
                second += 2;
            }else {
                second -= 2;
            }
        }
    }
    return 0;
}

2、
calculator：
    #include<sys/stat.h>
    #include<sys/types.h>
    #include<fcntl.h>
    #include<stdio.h>
    #include<stdlib.h>
    #include<string.h>
    int main(int argc, char * argv[]) {
    int fd;
    int result = 0;
    int r, v[100], ix = 0;
    fd = open("file.fifo", O_RDONLY);
    printf("fifo open ok\n");
    do{
        r = read(fd, &v[ix], sizeof(int));
        if(r > 0)
            result += v[ix];
        printf("%d\n", result);
        ix++;
    }while(r > 0);
    close(fd);
    printf("fifo read vals : %d\n", ix - 1);
    return 0;
}

collector：
#include<sys/stat.h>
#include<sys/types.h>
#include<fcntl.h>
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include <errno.h>
    int main() {
    int fd, rtn;
    int val[100];
    if(mkfifo("file.fifo", 0644) == -1) {
        if(errno == EEXIST) {
            printf("file.fifo exists\n");
        }
    }
    fd = open("file.fifo", O_WRONLY);
    printf("fifo open write only\n");
    for(int i = 0; i < 100; i++) {
        scanf("%d", &val[i]);
        rtn = write(fd, &val[i], sizeof(int));
    }
    printf("done\n");
    sleep(10);
    close(fd);
    return 0;
}

```

#### 打包文件的实验：

```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
char *fname[] = {"music", "video", "2.jpg"};
int main() {
    int fdpack, fd;
    int lenth, bytes;
    char buf[100];
    //open a.pack to write into
    fdpack = open("a.pack", o_CREAT | O_WRONLY, 0666);
    // pack file 1
    lenth = strlen(fnames[0]);
    write(fdpack, &lenth, sizeof(int));
    wirte(fdpack, fnames[0], strlen(fnames[0]));
    fd = open(fnames[0], O_RDONLY);
    lenth = lseek(fd, 0, SEEK_END);
    printf("file flength -- %d\n", lenth);
    write(fdpack, &lenth, sizeof(int));
    lseek(fd, 0, SEEK_SET);
    bytes = read(fd, buf, lenth);
    write(fdpack, buf, lenth);
    close(fd);
    close(fdpack);
}
```

```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
int main() {
    int fdpack, fd;
    int lenth, rdbytes;
    char filenm[100], buf[100];
    //open a.pack to read from
    fdpack = open("a.pack", O_RDONLY);
    //how long is file1'name?
    rdbytes = read(fdpack, &lenth, sizeof(int));
    printf("f name length-- %d\n", lenth);
    tdbytes = read(fdpack, filenm, lenth);
    filenm[lenth] = '\0';
    printf("f name -- %s\n", filenm);
    //open file1 to write into
    fd = open(filenm, O_WRONLY | O_CREAT, 0666);
    //how long is file1'data?
    rdbytes = read(fdpack, &lenth, sizeof(int));
    printf("f length -- %d\n", lenth);
    rdbytes = read(fdpack, buf, lenth);
    wirte(fd, buf, lenth);
    close(fd);
    close(fdpack);
}
```

#### 一个程序发送数据，令一个程序接收数据：

```c
//发
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/msg.h>
#include<sys/ipc.h>
#include<string.h>
#include"mymsg.h"
int main(int argc, char * argv[]) {
    int qid, rtn, v;
    char c;
    key_t key;
    mymsg mx;
    key = ftok("/tmp", 1);
    if(key == -1) {
        perror("ftok failed");
        exit(2);
    }
    qid = msgget(key, IPC_CREAT | 0600);
    if(qid == -1) {
        perror("msgget failed");
        exit(3);
    }
    scanf("%d %c", &v, &c);
    mx._type = 1;
    mx.ival = v;
    mx.ch = c;
    rtn = msgsnd(qid, &mx, sizeof(int) + sizeof(char), 0);
    if(rtn == -1) {
        perror("msgsnd failed");
        eixt(4)l
    }
    printf("send message: %d %c \n", mx.ival, mx.ch);
    return 0;
}
```

```c
//收
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/msg.h>
#include<sys/ipc.h>
#include<string.h>
#include"mymsg.h"
int main(int argc, char * argv[]) {
    int rtn;
    int qid;
    key_t key;
    mymsg msg;
    key = ftok("/tmp", 1);
    if(key == -1) {
        perror("ftok failed");
        exit(0);
    }
    qid = msgget(key, 0644);
    if(qid == -1) {
        perror("msgget failed");
        exit(2);
    }
    rtn = msgrcv(qid, &msg, 4 + 1, 0, 0);
    if(rtn == -1) {
        perror("msgsnd failed");
        eixt(3);
    }
    printf("recv : %d %c\n", msg.ival, msg.ch);
    return 0;
}
```

#### 计算整个文件的大小

```c
/*计算整个文件的大小*/
len = lseek(fd,0,SEEK_END);
```

#### 一次性读取文件中所有的内容，并输出文件大小和文件的内容

```c
//一次性读取文件中所有的内容，并输出文件大小和文件的内容
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<sys/stat.h>
#include<stdlib.h>
 
int main(int argc,char* argv[])
{
	FILE *fp = NULL;
	fp = fopen("001.PCM","r"); //打开文件
	if(fp == NULL)
	{
		printf("--: %s---%d--001.PCM open error",__FILE__,__LINE__);
	}
	fseek(fp,0L,SEEK_END);  //定位到文件末尾
	int flen = ftell(fp); //得到文件大小
	char *p = (char *)malloc(flen); //分配空间存储文件中的数据
	if(p == NULL)
	{
		fclose(fp);
		return 0;
	}
	fseek(fp,0L,SEEK_SET); //定位到文件开头
	fread(p,flen,1,fp);  //一次性读取全部文件内容
	printf("file flen is %d\n\n",flen);
	//printf("read file buff is %s",p);
	free(p);
	return 0;
}
```



### 10、执行c语言

如果头文件和.c文件在同一个文件夹，则只需要执行  gcc -o main.exe 所有.c文件

如果头文件不在当前目录，则需在以上命名后加上-Idirname  ---(dirname为头文件位置)

静态链接库：

（1）将所有的(不含main.c).c文件进行  gcc -c 文件名.c  生成对应的.o文件

（2）将所有的.o文件打包到libstack.a库文件中（其中stack可变）例：ar -rc libstack.a push.o pop.o isempty.o

（3）将静态库中的文件链接到可执行文件main中（其中main可变）

​	例：gcc -o main main.c -lstack -L./   (main.c  ：编写的主函数，./  ：查找位置)

动态库：例制作libstack2.so

gcc push.c pop.c isempty.c -fPIC -shared -o libstack2.so

gcc -o main main.c -lstack2 -L./

export LD_LIBRARY_PATH=./:$LD_LIBRARY_PATH

接下来即可执行可执行文件

### 11、makefile的使用

test : main.o push.o pop.o isempty.o

​			gcc -o test push.o pop.o isempty.o main.o

main.o : main.c push.h pop.h isempty.h

​					gcc -c main.c

push.o : push.c push.h

​					gcc -c push.c

pop.o : pop.c pop.h

​				gcc -c pop.c

isempty.o : isempty.c isempty.h

​						gcc -c isempty.c

clean:

​			rm test *.o

**执行：**

​	make test   ---(生成了4个.o文件)

​	./test

​	make clean

​	**install : test**

​				**makedir.app**

​	**cp test ./app**	

​	make install

​	touch pop.c

​	make

### 实验1

##### 第一题：

```bash
#! /bin/bash
count1=0
count2=0
count3=0
for var in $(ls *)
do
echo "$var"
if [ -d $var ] 
then
count1=`expr $count1 + 1`
fi
if [ -f $var ]
then
count2=`expr $count2 + 1`
fi
if [ -x $var ]
then
count3=`expr $count3 + 1`
fi
done
echo "子目录数:$count1"
echo "总的文件数为:$count2"
echo "有执行权限的文件数:$count3"
exit 0
```

##### 第二题：

```bash
#! /bin/bash
echo "Name---Chinese---Math---English" > record2 
while read Name Chinese  Math English
do
if [ $Chinese -lt 60 ] || [ $Math -lt 60 ] || [ $English -lt 60 ]
then
echo -n $Name >> record2
echo -n '---' >> record2
echo -n $Chinese >> record2
echo -n '---' >> record2
echo -n $Math >> record2
echo -n '---' >> record2
echo $English >> record2
echo $Name---$Chinese---$Math---$English
fi
done < record
```

#### 复习课3个题目

1、编写Shell脚本，将当前目录下所有的“.c”文件拷贝一份备份文件“.c.bak”。

```bash
#! /bin/bash
for file in $(ls *.c)
do
        if [ -f $file ]
        then
                cp $file $file.bak
        fi
done
exit 0
```

2、设计C程序filecut,将当前目录下文本文件f1中，从10字节开始、长为15字节的内容删除

```c
#include<fcntl.h>
#include<stdlib.h>
#include<unistd.h>
#include<stdio.h>
#include<sys/stat.h>
int main() {
    int fd;
    char buf1[9];
    int filesize;
    fd = open("f1", O_CREAT | O_RDWR);
    if(fd == -1) {
        perror("open");
        exit(1);
    }
    filesize = lseek(fd, 0, SEEK_END) - 1;
    printf("filesize=%d\n", filesize);
    char *buf2 = (char *)malloc(filesize - 24); //分配空间存储文件中的数据
    lseek(fd, 0, SEEK_SET);
    read(fd, buf1, 9);
    lseek(fd, 24, SEEK_SET);
    read(fd, buf2, filesize - 24);
    lseek(fd, 0, SEEK_SET);
    ftruncate(fd, 0);//清空文件
    lseek(fd, 0, SEEK_SET);
    write(fd, buf1, 9);
    write(fd, buf2, filesize - 24);
    close(fd);
    return 0;
}
```

3、设计C程序，程序运行时创建子进程，子进程输出自己的ID，从键盘读用户输入的若干字符串并存入当前目录下的文件f1。子进程结束后父进程输出子进程写入文件的内容。

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <sys/wait.h>
#include<string.h>
int count = 0;
int main(int argc, char* argv[])
{
        pid_t pid;
        int fd;
        char * info = "hello world";
        char buf[64];
        int status;

        fd = open("f2", O_RDWR | O_CREAT, 0660);
        if(fd < 0) {
                perror("open file failed");
                exit(1);
        }
        printf("before fork\n");
        if((pid = vfork()) < 0) {
                perror("fork error");
        }
        else if(pid == 0) {
                printf("This is the child process. My PID is: %d. My PPID is: %d.\n", getpid(), getppid());
                printf("enter # exit!\n");
                while(1) {
						scanf("%s", buf);
                        if(strcmp(buf, "#") == 0)
                                break;
                        write(fd, buf, strlen(buf));
                        write(fd, "\n", 1);
                        count += strlen(buf) + 1;
                }
                exit(0);
        }else {
                printf("this is the father process.\n");
                waitpid(pid, &status, 0);
                lseek(fd, 0, SEEK_SET);
                char *temp = (char *)malloc(count);
                read(fd, temp, count);
                printf("%s", temp);
                sleep(1);
        }
        exit(0);
}
```

