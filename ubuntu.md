# ubuntu

## 一、基础安装

### 常用命令

**查看ip**

```shell
ip addr
```

**查看主机名**

````shell
hostname
````

**查找本机用户名**

```shell
whoami
```

**查看隐藏目录**

```bas
ls -ah
```



**防火墙**

```shell
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

```shell
scp [ -r -p] ${file/folder} ${whoami}@${ip}:${指定路径}
```

---

### SSH服务

1. 安装所需包、更新Ubuntu系统

   ```shell
   sudo apt update && sudo apt upgrade
   ```

2. openssh-server组件
   ```shell
   sudo apt install openssh-server
   ```

   > 可能出现问题（自带的openssh-client的openssh-client与所要安装的openssh-server所依赖的版本不同，所以要安装对应的版本，来覆盖掉ubuntu自带的openssh-client）

![image-20230415014752156](D:\notes\img\image-20230415014752156.png)

3. 指定版本openssh-client
   ```shell
   sudo apt install openssh-client=1:8.2p1-4ubuntu0.2
   ```

4. 然后再安装openssh-server

5. 验证是否安装
   ```shell
   dpkg -l |grep ssh
   ssh localhost
   ```
   
   

![image-20230415020148045](D:\notes\img\image-20230415020148045.png)

6. 验证是否运行

   ```shell
   ps -e |grep ssh
   ```

   ![image-20230415020429436](D:\notes\img\image-20230415020429436.png)

---

### sogou输入法

1. 更新源

   ```shell
   sudo apt update
   ```

2. 安装 fcitx 输入法框架

   ```shell
   sudo apt install fcitx
   ```

3. 设置 fcitx为系统输入法

   > 坐下菜单->语言支持->选择fcitx

4. 设置 开机自启动

   ```shell
   sudo cp /usr/share/applications/fcitx.desktop /etc/xdg/autostart/
   ```

5. 卸载 ibus 输入法框架

   ```shell
   sudo apt purge ibus
   ```

6. 安装 搜狗

   ```shell
   sudo dpkg -i sogoupinyin_4.0.1.2800_x86_64.deb
   ```

7. ==安装 输入法依赖==

   ```shell
   sudo apt install libqt5qml5 libqt5quick5 libqt5quickwidgets5 qml-module-qtquick2
   sudo apt install libgsettings-qt1
   ```

> 重启 reboot

---

### wps/compare/code/google

```shell
sudo dpkg -i  bcompare-4.4.2.26348_amd64.deb
sudo dpkg -i  code_1.69.2-1658162013_amd64.deb
sudo dpkg -i  google-chrome-stable_current_amd64.deb
sudo dpkg -i  wps-office_11.1.0.11691_amd64.deb
```

> 重启 reboot

---

### zsh

1. 检查系统所有的shell

   ```shell
   cat /etc/shells   
   cat $SHELL #查看当前使用的shell
   ```

2. 安装zsh

   ```shell
   sudo apt install zsh #安装zsh
   chsh -s /bin/zsh #将zsh设置成默认shell（注：不要加sudo）
   ```

3. 安装`on-my-zsh`插件框架

   `如果没装git请 ：sudo apt install git`

   `如果没有装curl请 ：sudo apt install curl`

   ```shell
   wget --no-check-certificate https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh 
   sh -c "$(curl -fsSL https://gitee.com/shmhlsy/oh-my-zsh-install.sh/raw/master/install.sh)" #国内镜像源
   echo ${ZSH_CUSTOM}	#查看路径
   ```

4. 安装插件

   ```shell
   #zsh-autosuggestions 命令行命令键入时的历史命令建议
   git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
   #zsh-syntax-highlighting 命令行语法高亮插件
   git clone https://gitee.com/Annihilater/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
   ```

5. 配置

   ```shell
   sudo gedit ~/.zshrc
   ```

   `写入`

   ```shell
   #防止中文乱码
   export LC_ALL=en_US.UTF-8
   export LANG=en_US.UTF-8
   # 设置字体模式以及配置命令行的主题
   POWERLEVEL9K_MODE='nerdfont-complete'
   ZSH_THEME="agnoster"
   # 启动错误命令自动更正
   ENABLE_CORRECTION="true"
   # 在命令执行的过程中，使用小红点进行提示
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

## 二、java开发环境

### navicat

[网盘提取](链接：https://pan.baidu.com/s/1JjXrpXWsxV5TlrDzZCivpw?pwd=n5x9 
提取码：n5x9 )

> 下载文件，==断网操作==
>
> 执行navicat-keygen-tools/bin内的navicat-keygen以及之前生成的私钥

```shell
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

### jdk

[免登录下载](http://www.codebaoku.com/jdk/jdk-oracle-jdk11.html)

1. 安装

   ```shell
   sudo dpkg -i jdk-11.0.17_linux-x64_bin.deb
   ```

2. 配置环境

   ```shell
   sudo vim ~/.zshrc	#使用zsh
   ```

   `在末尾添加`

   ```shell
   export JAVA_HOME=/usr/lib/jvm/jdk-11
   export JRE_HOME=${JAVA_HOME}/jre
   export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
   export PATH=${JAVA_HOME}/bin:$PATH
   ```

3. 配置生效

   ```shell
   source ~/.zshrc
   ```

4. 运行查看

   ```shell
   java -version
   ```

### maven

[下载地址](https://archive.apache.org/dist/maven/maven-3/)`注：binaries版`

```shell
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

---

## 三、开发工具

### idea

## 四、版本控制工具

### Git

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

<img src="D:\notes\img\image-20230417005527739.png" alt="image-20230417005527739" style="zoom: 67%;" />

- Workspace：开发者工作区

- Index / Stage：暂存区/缓存区

- Repository：仓库区（或本地仓库）

- Remote：远程仓库

> 查看文件状态

- Untracked：未提交

- Unmodify：文件已入库

- Modified：新文件，但未提交

- Staged：暂存状态

```bash
#查看指定文件状态
git status [filename]
#查看所有文件状态
git status

git add --all			#添加所有文件到暂存区
		.				#不会记录删除操作
git commit -m "信息内容"	#提交暂存区内容到本地仓库 -m 提交信息
git commit --amend		#重写上一次的提交信息

git log					#查看日志
git log --pretty=oneline	#简洁输出
git log [filename]		#查看该文件有多少个版本可以回滚

git tag 别名 版本号		#设置版本标签
git tag -d 别名			#删除标签

git reflog			#当前版本库的提交历史

```

> git分支 ==git branch、git checkout==

```bash
git branch			#查看HEAD的指向
git branch 分支名			#新建分支
git branch -a/-v			#查看所有分支
git brabch -d/-D 分支				#删除分支
git push origin --delete 远程分支名		#删除远程分支

git checkout 分支			#切换分支
git checkout -b 分支		#创建并切换分支
git checkout -- file		#文件撤销回到最近一次修改的状态

git merge 分支			#合并
```



> 回滚代码仓库 ==git reset [恢复等级] [回滚id/tag别名]==

```bash
git reset --soft		#头指针恢复，已经add的暂存区以及工作空间的所有东西都不变
          --mixed		#头指针恢复，已经add的暂存区会丢失掉，工作空间的代码不变
          --hard		#一切就全都恢复了
git reset --hard HEAD^		#迭代我们仓库的上一个版本
                 HEAD~3		#回滚master前三个版本
git reset 回滚id filename
```



> 远程交互

打开.git/config  找到[remote "origin"] url = "https/ssh"，可以手动操作

```bash
git remote add origin url		#添加remote
git remote remove origin		#删除
git remote rename origin		#修改
```

`如果是 ssh的url，需要生成.ssh的安全认证的公钥`

```bash
ssh-keygen -t rsa -C'url'		#三次回车
```

### GitLab

运行在linux中，[下载官网](https://packages.gitlab.com/gitlab/)

