# Ubuntu



[TOC]

## 1、基础安装

### 1.1 常用命令

**查看ip**

```bash
ip addr
```

**查看主机名**

````bash
hostname
````

**查找本机用户名**

```bash
whoami
```

**查看隐藏目录**

```bash
ls -ah
```



**防火墙**

```bash
sudo ufw enable	#开启
		 disable	#关闭
		 status	#状态
		 reload	#重启
		 version	#版本
		 default allow	#默认允许外部访问
		 default deny	#默认拒绝外部访问
		 allow ${端口号} #允许指定外部端口访问 重启失效
```

**SSH协议远程拷贝**

```bash
scp [ -r -p] ${file/folder} ${whoami}@${ip}:${指定路径}

scp ${whoami}@${ip}:${file/folder} ${指定路径}
```

---

### 1.2 SSH服务

1. 安装所需包、更新Ubuntu系统

   ```bash
   sudo apt update && sudo apt upgrade
   ```

2. openssh-server组件
   ```bash
   sudo apt install openssh-server
   ```

   > **注意**：可能出现问题（自带的openssh-client的openssh-client与所要安装的openssh-server所依赖的版本不同，安装指定提示的版本openssh-client）
   
   ```bash
   sudo apt install openssh-client=[版本号]
   ```

​		**重新执行   sudo apt install openssh-server**

3. 、开启openssh服务 `<可忽略（默认自启）>`

   ```bash
   sudo /etc/init.d/ssh start
   ```

4. 验证是否安装`<两种方式有输出代表成功>`
   ```bash
   dpkg -l |grep ssh
   ```

   ```bash
   ssh localhost
   ```


![image-20230415020148045](img\image-20230415020148045.png)

6. 验证是否运行

   ```bash
   ps -e |grep ssh
   ```

   ![image-20230415020429436](img\image-20230415020429436.png)
   
   `补充：`
   
   远程ssh 格式 
   
   ```bash
   ssh username@192.168.1.112
   #改别名，不用每次都输ip
   sudo vim ~/.bashrc
   #写入
   alias '${别名}'="ssh username@xx.11.232.xx"
   #使生效
   source ~/.bashrc
   ```
   
   

---

### 1.3 software

1. 更新源

   ```bash
   sudo apt update
   ```

2. 安装 fcitx 输入法框架

   ```bash
   sudo apt install fcitx
   ```

3. 设置 fcitx为系统输入法

   > 坐下菜单->语言支持->选择fcitx

4. 设置 开机自启动

   ```bash
   sudo cp /usr/share/applications/fcitx.desktop /etc/xdg/autostart/
   ```

5. 卸载 ibus 输入法框架

   ```bash
   sudo apt purge ibus
   ```

6. 安装 搜狗

   ```bash
   sudo dpkg -i sogoupinyin_4.0.1.2800_x86_64.deb
   ```

7. ==安装 输入法依赖==

   ```bash
   sudo apt install libqt5qml5 libqt5quick5 libqt5quickwidgets5 qml-module-qtquick2
   sudo apt install libgsettings-qt1
   ```

```bash
sudo dpkg -i  bcompare-4.4.2.26348_amd64.deb
sudo dpkg -i  code_1.69.2-1658162013_amd64.deb
sudo dpkg -i  google-chrome-stable_current_amd64.deb
sudo dpkg -i  wps-office_11.1.0.11691_amd64.deb
```

> 重启 reboot

---

### 1.4 zsh

1. 检查系统所有的shell

   ```bash
   cat /etc/shells   
   cat $SHELL #查看当前使用的shell
   ```

2. 安装zsh

   ```bash
   sudo apt install zsh #安装zsh
   chsh -s /bin/zsh #将zsh设置成默认shell（注：不要加sudo）
   ```

3. 安装`on-my-zsh`插件框架

   `如果没装git请 ：sudo apt install git`

   `如果没有装curl请 ：sudo apt install curl`

   ```bash
   wget --no-check-certificate https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh 
   sh -c "$(curl -fsSL https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)" #国内镜像源
   echo ${ZSH_CUSTOM}	#查看路径
   ```

4. 安装插件

   ```bash
   #zsh-autosuggestions 命令行命令键入时的历史命令建议
   git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
   #zsh-syntax-highlighting 命令行语法高亮插件
   git clone https://gitee.com/Annihilater/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
   ```

5. 配置

   ```bash
   sudo gedit ~/.zshrc
   ```

   `写入`

   ```bash
   #防止中文乱码
   export LC_ALL=en_US.UTF-8
   export LANG=en_US.UTF-8
   #设置字体模式以及配置命令行的主题
   POWERLEVEL9K_MODE='nerdfont-complete'
   ZSH_THEME="agnoster"
   #启动错误命令自动更正
   ENABLE_CORRECTION="true"
   #在命令执行的过程中，使用小红点进行提示
   COMPLETION_WAITING_DOTS="true"
   plugins=(
           git
           extract
           zsh-autosuggestions
           zsh-syntax-highlighting
   )
   source $ZSH_CUSTOM/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
   ```

   ---

## 2、开发环境

### 2.1 jdk

[免登录下载](http://www.codebaoku.com/jdk/jdk-oracle-jdk11.html)

1. 安装

   ```bash
   sudo dpkg -i jdk-11.0.17_linux-x64_bin.deb
   ```

2. 配置环境

   ```bash
   sudo vim ~/.zshrc	#使用zsh
   ```

   `在末尾添加`

   ```bash
   export JAVA_HOME=/usr/lib/jvm/jdk-11
   export JRE_HOME=${JAVA_HOME}/jre
   export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
   export PATH=${JAVA_HOME}/bin:$PATH
   ```

3. 配置生效

   ```bash
   source ~/.zshrc
   ```

4. 运行查看

   ```bash
   java -version
   ```

### 2.2 maven

[下载地址](https://archive.apache.org/dist/maven/maven-3/)`注：binaries版`

```bash
sudo tar -zxvf apache-maven-3.8.8-bin.tar.gz -C /usr/local/src/maven/
sudo vim ~/.zshrc
#写入如下内容
    #set maven
    export MAVEN_HOME=/usr/local/src/maven/apache-maven-3.8.8
    export CLASSPATH=${MAVEN_HOME}/lib:$CLASSPATH
    export PATH=${MAVEN_HOME}/bin:$PATH
#构建命令
source ~/.zshrc
#查看版本号
mvn -v
#配置ali镜像
sudo vim /usr/local/src/maven/apache-maven-3.8.8/config/settings.xml
#写入以下内容到 </mirrors>里
	 <!-- aliyun中央仓库 -->
         <mirror>
            <id>alimaven</id>
            <name>aliyun maven</name>
            <mirrorOf>central</mirrorOf>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        </mirror>
#中央仓库下载缺省或
#更新的各种配置文件和类库（jar包)到Maven本地仓库中 -> ~/.m2/repository
mvn help:system
sudo cp /usr/local/src/maven/apache-maven-3.8.8/config/settings.xml ~/.m2
#拷贝目的：升级的时候只需要替换一下安装包

```

### 2.3 navicat

[网盘提取](链接：https://pan.baidu.com/s/1yeufENjGLQRInN2o3J013A?pwd=toif )

给navicat15.AppImage附执行权限，之后可以直接双击执行

```bash
sudo chmod +x navicat15.AppImage
```

> 下载文件，==断网操作==
>
> 执行navicat-keygen-tools/bin内的navicat-keygen以及之前生成的私钥

```bash
./navicat-keygen-tools/bin/navicat-keygen --text RegPrivateKey.pem
```

```tex
依次输入：	1	1	15
		-- 会生成一串许可证秘钥随后的姓名和组织随便填
		-- 复制秘钥到软件界面注册->手动激活
		-- 复制请求码，到命令行，按两次回车
		-- 复制生成的激活码
		-- 切换回软件 将激活码粘贴到框内点击ok，提示激活成功
```

---

## 4、版本控制工具

### 4.1 Git

```tex
SVN是集中式版本控制，版本库是集中在中央服务器的
拉去最新版本，推送到中央服务器
必须联网，对网络带宽要求较高
```

```tex
Git是分布式版本控制系统，没有中央服务器
目前最先进的分布式版本控制系统
```

> Git配置

查看配置 git config -l

```bash
#查看系统配置
git config --system --list
#查看本地配置
git config --global --list
```

> ==设置用户名和邮箱（用户标识，必要）==

```bash
git config --global user.name "qiuyu"
git config --global user.email "1969414176@qq.com"
```

> Git基本理论（核心）

<img src="img\image-20230417005527739.png" alt="image-20230417005527739" style="zoom: 67%;" />

- Workspace：开发者工作区

- Index / Stage：暂存区/缓存区

- Repository：仓库区（或本地仓库）

- Remote：远程仓库

> git status	查看文件状态

- Untracked：未提交

- Unmodify：文件已入库

- Modified：新文件，但未提交

- Staged：暂存状态

```bash
git status [filename]		#查看指定文件状态
git status				#查看所有文件状态
```

> git add	#添加暂存区	commit	#提交本地仓库

```bash
git add --all				#添加所有文件到暂存区
		.					#不会记录删除操作
git commit -m "信息内容"	 #提交暂存区内容到本地仓库 -m 提交信息
git commit --amend			#重写上一次的提交信息
```

> git log	查看历史版本

```bash
git log						#查看日志
git log --pretty=oneline	#简洁输出
git log [filename]			#查看该文件有多少个版本可以回滚
git reflog					#当前版本库的提交历史
```

> git tag	设置版本标签

```bash
git tag 别名 版本号			#设置版本标签
git tag -d 别名			#删除标签
```



> git  [分支]

branch 创建分支	checkout 切换分支	push 删除	merge 合并	fetch 拉去远程所有分支	

```bash
git branch					#查看HEAD的指向(当前所属分支)
git branch 分支名				#新建分支
git branch -a/-v					#查看当前所有分支
git branch -m 分支名 新的分支名			#修改分支名称
git brabch -d/-D 分支名				#删除分支
git push origin --delete 远程分支名		#删除远程分支

git checkout 分支				#切换分支
git checkout -b 分支			#创建并切换分支
git checkout -- file		#文件撤销回到最近一次修改的状态

git merge 分支			#合并
              --allow-unrelated-histories	#强制合并分支
git fetch
		  xxx	#拉去指定的最新内容
```

> git reset	回滚代码仓库 	==git reset [恢复等级] [回滚id/tag别名]==

```bash
git reset --soft		#头指针恢复，已经add的暂存区以及工作空间的所有东西都不变
          --mixed		#头指针恢复，已经add的暂存区会丢失掉，工作空间的代码不变
          --hard		#一切就全都恢复了
git reset --hard HEAD^		#迭代我们仓库的上一个版本
                 HEAD~3		#回滚master前三个版本
git reset 回滚id filename
```

> git remote	远程交互

打开.git/config  找到[remote "origin"] url = "https/ssh"，可以手动操作

```bash
git remote show origin			#查看远程仓库信息
git remote add origin url		#添加remote
git remote prune origin			#刷新本地分支仓库
git remote remove origin		#删除
git remote rename origin		#修改
```

`如果是 ssh的url，需要生成.ssh的安全认证的公钥`

```bash
ssh-keygen -t rsa -C'email@email.com'		#三次回车
ssh -T git@github.com						#测试是否配置成功
```

>git clone	克隆，不需要git init，clone自动初始化

```bash
git clone url
          -b url		#指定分支
```

> git stash	保存当前工作切换分支（以栈的方式保存，先进后出）

```bash
git stash			#保存当前工作状态
          list		#查看当前储存多少个工作状态
          pop		#将状态恢复
          drop list名称		#移除指定list
          clear		#一处所有list
          show		#查看最新保存的stash和当前目录的差异
```

> git pull	拉去

> git push	推送到远程仓库

```bash
git push -u origin 分支名			#推送远程分支
```



---

## 5、Shell

### 5.1 变量

**5.1.1 系统预定义变量**

```bash
$HOME	$PWD	$SHELL	$USER	#常用变量
```

```bash
env				
env | less
printenv | less		#查看所有系统全局变量

printenv USER		#打印某个变量 printenv 替换 $
echo $USER			#使用某个变量

set				#显示当前Shell中所有变量
set | less
```

**5.1.2 自定义变量**

**基本语法**

​		变量名=变量值，注意：=前后不能有空格

​		撤销变量：unset	变量名

 		声明静态变量：redonly变量，注意：不能unset

```bash
export 变量名		#可把变量提升为全局变量
```

**5.1.3 特殊变量**

**$n**

n  为数字，$0 脚本名称，$0-9 一到九个参数，十以上用大括号包含，如 ${10}`

**$#**

获取所有==输入参数个数==，常用于循环，判断参数的个数是否正确，以及加强脚本的健壮性

**$***

代表命令行中所有的参数，==把所有的参数看成一个整体==

**$@**

代表命令行中所有的参数，==把每个参数区别对待==

**$?**

最后一次执行命令的返回状态，如果值为0，证明上一个命令正确执行，如果非0，证明执行不正确

### 5.2 运算符

**基本语法** （###普通写法：`expr 1 + 2`，定义变量：s=`expr 5 \* 2`）

​		"$((运算式))" 或 "$[运算式]"

​		例如：`s=$[(2+3)*4]`	#常用 []

### 5.3 条件判断

**1）基本语法**

test [ condition ]		==注：前后有空格==

如：test $a = hello	echo $?

**2）常用判断条件**

**（1）整数比较**

-eq	等于queal	-ne	不等于not equal	

-lt	小于less than	-le	小于等于less equal	

-gt	大于greater than	-ge	大于等于greater equal		如：`[ 1 -ge 2 ]`

**（2）按文件权限判断**

-r 有read的权限	-w 有write的权限	-x 有execute的权限		如：`[ -w 文件 ]`

**（3）按文件类型进行判断**

-e	文件存在（existence）		如：`[ -e xx/xx/文件 ]`

-f	文件存在并且是一个常规的文件（file）

-d	文件存在并且是一个目录（directory）

**（4）多条件判断**

&& 或 -a	前一条执行成功，才执行后一条

|| 或 -o	前一天执行失败，才执行后一条		如：`[ aaa ] && echo OK || echo notOK`

### 5.4 流程控制

**5.4.1 if 判断**

**基本语法**

（1）单分支

```bash
if [ 条件判断式 ] ; then
	程序
fi
#或者
if [ 条件判断式 ]
then
	程序
fi
```

（2）多分支

```bash
if [ 条件判断式 ]
then
	程序
elif [ 条件判断式 ]
then
	程序
else
	程序
fi
```

**5.4.2 case 语句**

**基本语法**

```bash
case $变量名 in			#行尾必须 in
"值1")						#每一个模式匹配必须以右括号")"结束
	变量等于值1，执行程序1
;;							#";;"表示命令序列结束，相当于java中的break
"值2")
	变量等于值2，执行程序2
;;
	...			#省略其他分支
*)							#"*)"代表默认模式，相当于java中的default
	以上都不满足，执行此程序
;;
esac
```

**5.4.3 for 循环**

**基本语法**

```bash
for (( 初始值;循环控制条件;变量变化 ))		#如：for((i=0;i<=100;i++))
do
	程序										#sum=$[$sum+$i]			
done
echo $sum			#输出
#或
for 变量名 in 值1 值2 值3; do echo $变量名; done
#获
for i in {1...100}; do sum=$[$sum+$i]; done; echo $sum
```

**5.4.3 while 循环**

```bash
a=1
while [ $a -le $1 ]
do
#	sum2=$[ $sum2 + $a ]
#	a=$[$a + 1]
	let sum2 += a
	let a++
done
echo $sum2
```

### 5.5 read 读取控制台输入

**基本语法**

read (选项) (参数)

1）选项

​		-p：指定读取值时的提示符

​		-t：指定读取值时等待的时间（秒）-t不加表示一直等待

2）参数

​		变量：指定读取值的变量

如：`read -t 10 -p "请输入xx：" name`

​		`echo "welcome，$name"`

### 5.5 函数
