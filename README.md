# Docker-
Skip to content
 
Search or jump to…

Pull requests
Issues
Marketplace
Explore
 
@SunNYNO1 Sign out
0
0 0 SunNYNO1/Docker-
 Code  Issues 0  Pull requests 0  Projects 0  Wiki  Insights  Settings
Docker-
/

Name your file…
or cancel
 
# Docker的基本使用

## 一、Docker指令：

### 1-镜像命令
* 查看docker版本: 
    * docker version
* 拉取镜像：
    * docker pull 镜像名：标签名
* 删除镜像：
    * docker rmi 镜像名：标签名  
* 将在镜像中的各种修改保存到本地镜像：
    * docker commit  [m="commit jdk"]  [--author="gutianlangyu"]  镜像ID 本地仓库名/镜像名:标签名  
    （-m：描述我们此次创建image的信息； --author：用来指定作者。）
* 拷贝本地文件到容器，或者反之：
    * docker cp 本地路径+文件名 容器长ID:/容器中的某一具体位置+文件名（可以重命名为其他名字）
    * docker cp 容器文件路径 本地路径
    
### 2-容器命令
* 创建并运行容器：
    * docker run -itd --name --容器名 镜像名：标签名 [命令] 
    （参数介绍：-it 交互式进程 ； -d 守护式进程）
* 与指定容器交互：
    * docker exec -it 标签名 /bin/bash [或其他命令] 
    （/bin/bash意为：进入容器的bash命令行窗口）
* 显示正在容器：
    * docker ps (显示正在运行的容器)
    * docker ps -a （显示所有创建的容器）
* 关闭/移除容器：
    * docker stop 容器名
    * docker rm 容器名
* 查看容器信息:
    * docker inspect 容器名 -f '{{.Id}}'（查看长ID）
* 显示容器中的进程：
    * docker top 容器名

### <font color=#FF0000>3-Dockerfile的使用（待补充）

## 二、Docker容器的环境配置：
### 安装cuda
* 官网下载对应版本的cuda ：[cuda下载地址](https://developer.nvidia.com/cuda-toolkit-archive)
* docker cp cuda路径 容器长ID：cuda文件名
* 安装过程参考下面cudnn安装过程  

### 安装cudnn
* 下载cudnn ：[cudnn下载地址](https://developer.nvidia.com/rdp/cudnn-archive)
* docker cp cuda路径 容器长ID：cuda文件名
* cp cudnn-9.0-linux-x64-v7.solitairetheme8 cudnn-9.0-linux-x64-v7.tgz  
(这句话的意思是，将拷贝过来的容器转换为tgz文件)
* tar -xvf cudnn-9.0-linux-x64-v7.tgz
* cp include/* /usr/local/cuda-9.0/include  
(如果报错没有该文件夹，cd到cuda文件所在地址)
* cp lib64/* /usr/local/cuda-9.0/lib64
* chmod a+r /usr/local/cuda-9.0/include/cudnn.h /usr/local/cuda-9.0/lib64/libcudnn*
* export PATH=/usr/local/cuda-9.0/bin:\\$PATH  
(为cuda配置环境变量)
* vi ~/.bashrc(或者nano ~/.bashrc)
(打开该文件，以配置环境变量)
* 在打开的文件中加入“export LD_LIBRARY_PATH=/home/cuda/lib64:\\$LD_LIBRARY_PATH”
* source ~/.bashrc
(使命令生效)
* ldconfig -v
* cat /usr/local/cuda-9.0/include/cudnn.h | grep CUDNN_MAJOR -A 2
(cudnn安装成功，查看cudnn版本)  

### 安装anaconda
* apt-get install wget
* wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda2-4.2.0-Linux-x86_64.sh
(从清华的镜像源下载anaconda)
* bash Anaconda2-4.2.0-Linux-x86_64.sh
(通过.sh文件安装anaconda)
* vim ~/.bashrc
* export PATH=/root/anaconda2/bin:\\$PATH
* source ~/.bashrc  
(配置环境变量，安装过程中可以设置将路径加入环境变量，从而不再需要另外配置)
* 在conda中创建新的环境并配置
* 更新容器的镜像源：
    * 备份源：sudo cp /etc/apt/sources.list /etc/apt/sources.list.old
    * 编辑源：vim /etc/apt/sources.list
    * 删除sources.list文件内所有内容
    <table><tr><td bgcolor=pink>

    #deb-src http://mirror.neu.edu.cn/ubuntu/ xenial main restricted  
    #Added by software-properties  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial main restricted  
    deb-src http://mirror.neu.edu.cn/ubuntu/ xenial restricted multiverse universe #Added by software-properties  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-updates main restricted  
    deb-src http://mirror.neu.edu.cn/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-properties  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial universe  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-updates universe  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial multiverse  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-updates multiverse  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse  
    deb-src http://mirror.neu.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-properties  
    deb http://archive.canonical.com/ubuntu xenial partner  
    deb-src http://archive.canonical.com/ubuntu xenial partner  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-security main restricted  
    deb-src http://mirror.neu.edu.cn/ubuntu/ xenial-security main restricted multiverse universe #Added by software-properties  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-security universe  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-security multiversedeb http://mirror.neu.edu.cn/ubuntu/ xenial main restricted  
    deb-src http://mirror.neu.edu.cn/ubuntu/ xenial restricted multiverse universe #Added by software-properties  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-updates main restricted  
    deb-src http://mirror.neu.edu.cn/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-properties  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial universe  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-updates universe  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial multiverse  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-updates multiverse  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse  
    deb-src http://mirror.neu.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-properties  
    deb http://archive.canonical.com/ubuntu xenial partner  
    deb-src http://archive.canonical.com/ubuntu xenial partner  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-security main restricted  
    deb-src http://mirror.neu.edu.cn/ubuntu/ xenial-security main restricted multiverse universe #Added by software-properties  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-security universe  
    deb http://mirror.neu.edu.cn/ubuntu/ xenial-security multiverse  

    </td></tr></table>  
    
    *  更新源：apt-get update  

* 创建dockerfiles文件夹在其中建立Dockerfile文件（没有后缀）
<table><tr><td bgcolor=pink>

    FROM sunke/sk:ssd  （此处为自己的镜像名和标签名）
    MAINTAINER sunke 1246680000@shu.edu.cn ssh  
    RUN apt-get update -y  
    RUN apt-get --purge remove openssh-client -y  
    RUN apt-get install openssh-client=1:7.1p1-4 -y  
    RUN apt-get install -y openssh-server  
    RUN mkdir /var/run/sshd  
    RUN echo "root:1234" | chpasswd  
    #允许root用户以任何认证方式登陆（用户密码认证和公钥认证）  
    RUN sed -i 's/prohibit-password/yes/g' /etc/ssh/sshd_config  
    ENTRYPOINT ["/usr/sbin/sshd","-D"]  
    EXPOSE 22   

</td></tr></table>

* 执行以上Dockerfile
    * docker build -t 镜像名：标签名 .   
    （在默认路径不是dockerfiles/Dockerfile时，使用命令-f Dockerefile的绝对路径）
* 生成镜像：
    * dockers stop 容器ID  
    （停止容器）
    * docker commit -a "作者信息" -m "附带信息" [容器ID] [新的镜像名]:[新的标签名]  
    （将该容器创建到本地的镜像）  
    <font color=#FF0000>（注意镜像与容器并不是包含关系，容器的环境配置好之后可以制作为镜像，在上传容器环境之前也必须将其制作为镜像）（容器可以理解为镜像的编辑状态，pull和push时都要是镜像的状态）

* 上传镜像：
    * docker push 远程仓库地址/仓库名/远程仓库中的镜像名：标签名  
     (这里push的镜像名需要和远程仓库路径对应，所以需要将上传的镜像更改名字为以上形式使用)(也可以在生成镜像的时候直接把名字命名为该形式)
     
### 机器学习平台操作
* 机器学习平台登录：
    * [机器学习平台](http://10.0.4.228)
* 建立个人仓库：
    * 镜像仓库-我的仓库-创建私有仓库
* 仓库管理平台：
    * [仓库管理平台](https://hub.hoc.ccshu.net/harbor/projects)
* 将镜像上传到该仓库（过程如上）
* 创建容器：机器学习-容器管理-创建容器
    * 自定义镜像（自己上传到仓库的镜像）-配置数据卷  
    (可以在容器的详情里配置自己的镜像环境)  
    <font color=#FF0000>(如何创建自己的数据卷)
* 使用FileZilla的方式:
    * 连接自己的容器，将本地数据可视化的拷贝到容器的数据卷中
    * 通过命令窗口远程机器学习平台，cd到训练模型的文件路径
    * 执行.py文件

* <font color=#FF1111>(待补充)使用pycharm的方式：
* 

## 四、vim的使用事项：
* 打开文件：
    * vim ~/.bashrc  
    (上例：打开环境变量配置文件)
* 进入编辑：
    * i
* 退出编辑：
    * esc
* 退出编辑情况下快捷键;
    * 删除一行：DD
    * 编辑命令: “:w\q\q!”
    (写入、退出、强制退出)
* 双击复制、右击粘贴

## 五、参考链接：
* [zsxDocker环境配置](https://blog.csdn.net/GodWriter/article/details/88734552)
* [wjs镜像制作及上传](https://stepneverstop.github.io/2019/01/02/create-sniper-docker-image/)
* [师姐的pycharm配置文章](https://www.jianshu.com/p/893e85353843)
* [runoob.com:Docker教程](http://www.runoob.com/docker/docker-command-manual.html)
@SunNYNO1
Commit new file
Commit summary 
docker try
Optional extended description

docker 
  Commit directly to the master branch.
  Create a new branch for this commit and start a pull request. Learn more about pull requests.
 
© 2019 GitHub, Inc.
Terms
Privacy
Security
Status
Help
Contact GitHub
Pricing
API
Training
Blog
About
