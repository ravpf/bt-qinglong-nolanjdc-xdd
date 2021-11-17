# bt-qinglong-nolanjdc-xdd
整理出一套适合小白的教程
### 环境：centos 7.6，宝塔面板以及docker

控制台放行端口，然后用xhell安装宝塔面板

[https://www.netsarang.com/zh/free-for-home-school/](https://www.netsarang.com/zh/free-for-home-school/)打开，这里填写邮箱获取免费版

### 宝塔面板：

```jsx
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```

安装完后，复制登录地址账号密码，忘记了就在xshell工具输入bt，然后输入14

登录宝塔面板，第一次打开宝塔面板要你安装环境，关掉不装直接左侧菜单栏找到软件商店，在运行环境里找到docker管理器，安装。

安装完成后打开xshell

### 青龙面板：

```jsx
docker run -dit \
   -v $PWD/ql/config:/ql/config \
   -v $PWD/ql/log:/ql/log \
   -v $PWD/ql/db:/ql/db \
   -v $PWD/ql/repo:/ql/repo \
   -v $PWD/ql/raw:/ql/raw \
   -v $PWD/ql/scripts:/ql/scripts \
   -v $PWD/ql/jbot:/ql/jbot \
   -p 5700:5700 \
   --name qinglong \
   --hostname qinglong \
   --restart unless-stopped \
   whyour/qinglong:latest
```

注意：第9行5700:5700，修改前面那个5700就可以设置端口，比如5800:5700，你的青龙面板访问端口则为5800

根据提示直到安装完成，完成后，我们去浏览器打开http://你的ip:你设置的端口   例如http://192.168.0.1:5700。。如果是提示登录，那么使用账号admin，密码admin登录一次。然后宝塔面板左侧菜单找到文件，进入宝塔面板的文件管理器。找到路径：根目录/root/ql/config，找到auth.json的文件，编辑。设置好账号密码，重新返回青龙面板使用新账号密码登录。如果第一次打开青龙面板直接进入安装程序，则设置密码然后登录即可。

### Nolanjdc

拉源码国内

```jsx
git clone https://ghproxy.com/https://github.com/ravpf/nvjdcdocker.git /root/nolanjdc
```

拉取基础

```jsx
sudo docker pull ravpf/nvjdc:latest
```

如果提示这个，就是失败了，可能是网络原因error pulling image configuration: Get "[https://production.cloudflare.docker.com/registry-v2/docker/registry/v2/blobs/sha256/4b/4bc8f708bdc9bcee830926fe46f33082332b53ef586c84c26de08f137f4c96ff/data?verify=1637048374-JykQlbm%2BsBtB%2FflzE8AFEi3FLcQ%3D](https://production.cloudflare.docker.com/registry-v2/docker/registry/v2/blobs/sha256/4b/4bc8f708bdc9bcee830926fe46f33082332b53ef586c84c26de08f137f4c96ff/data?verify=1637048374-JykQlbm%2BsBtB%2FflzE8AFEi3FLcQ%3D)": net/http: TLS handshake timeout

重来

Digest: sha256:4f4378dcf8d722f2eacb2bd6e062ea25e3c22f17b6b38d5c292813de9a75c4f8


表示成功

执行命令

```jsx
yum install wget unzip -y
```

创建一个目录放配置

```jsx
cd /root/nolanjdc
```

创建一个config的目录

```jsx
mkdir -p  Config && cd Config
```

下载config.json 配置文件并且修改自己的配置不能缺少

```jsx
wget -O Config.json   https://ghproxy.com/https://raw.githubusercontent.com/ravpf/nvjdc/main/Config.json
```

提示2021-11-16 15:16:43 (154 MB/s) - ‘Config.json’ saved [1427/1427]为下载成功

下载完后如何配置？

宝塔面板文件管理器/root/nolanjdc/Config，找到config.json，编辑需要修改的内容如下：

//服务器名称
"QLName": "容器名，与青龙后台创建的尽量保持一致",
//青龙地址
"QLurl": "[***http://](http://119.29.21.190:5700/)ip/端口***",     如[http://152.136.209.57:5700](http://152.136.209.57:5700/)
//青龙2,9 OpenApi Client ID
"QL_CLIENTID": "复制到这里",
//青龙2,9 OpenApi Client Secret
"QL_SECRET": "复制到这里",

QL_CAPACITY": 40,     这个40修改后打开nolan既可以看到

其他内容按需求修改，改完保存

回到xshell

回到nolanjdc创建目录chromium文件夹并进入

```jsx
cd /root/nolanjdc && mkdir -p  .local-chromium/Linux-884014 && cd .local-chromium/Linux-884014
```

下载

```jsx
wget https://mirrors.huaweicloud.com/chromium-browser-snapshots/Linux_x64/884014/chrome-linux.zip && unzip chrome-linux.zip
```

删除只是下载的压缩包

```jsx
rm  -f chrome-linux.zip
```

回到刚刚创建的目录

```jsx
cd  /root/nolanjdc
```

启动镜像

```jsx
sudo docker run   --name nolanjdc -p 5701:80 -d  -v  "$(pwd)":/app \
-v /etc/localtime:/etc/localtime:ro \
-it --privileged=true  ravsmoe/nvjdc:latest
```

注意：这里默认端口是5701，如果装了ninja并且端口也为5701，一定要记得改端口。上述脚本5701:80  改5701即可。

查看日志

```jsx
docker logs -f nolanjdc
```

回到浏览器，尝试打开http://ip:5701,完成！

### XDD-plus

依赖：go语言，因此需要安装go。

## GO语言介绍

Go（又称Golang）是Google开发的一种静态强类型、编译型、并发型，并具有垃圾回收功能的编程语言。
罗伯特·格瑞史莫（Robert Griesemer），罗勃·派克（Rob Pike）及肯·汤普逊（Ken Thompson）于2007年9月开始设计Go，稍后Ian Lance Taylor、Russ Cox加入项目。Go是基于Inferno操作系统所开发的。Go于2009年11月正式宣布推出，成为开放源代码项目，并在Linux及Mac OS X平台上进行了实现，后来追加了Windows系统下的实现。在2016年，Go被软件评价公司TIOBE 选为“TIOBE 2016 年最佳语言”。 目前，Go每半年发布一个二级版本（即从a.x升级到a.y）。

## 第一步 下载解压

教程工具仍然是xshell。

老样子SSH到服务器 先 yum check-update一遍

```jsx
yum check-update
```

然后下载go

```jsx
cd /usr/local && wget https://golang.google.cn/dl/go1.16.7.linux-amd64.tar.gz -O go1.16.7.linux-amd64.tar.gz
```

解压

```jsx
tar -xvzf go1.16.7.linux-amd64.tar.gz
```

## 第二步 配置环境

然后我们宝塔面板，手动打开根目录/etc/profile文件，将如下文字添加的文件最后一行，保存，退出

```jsx
export GO111MODULE=on
export GOPROXY=https://goproxy.cn
export GOROOT=/usr/local/go
export GOPATH=/usr/local/go/path
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
```

然后敲命令

```jsx
source /etc/profile
```

## 检查是否安装成功

输入命令

```jsx
go env
```

### 环境安装好了，接下来XDD-plus

## 第二步 安装Git

重新打开xshell，输入如下命令 安装Git

```bash
wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

rpm -ivh epel-release-latest-7.noarch.rpm

yum install -y git
```

然后进入第三步

## 第三步 拉库编译

先拉库

```bash
cd ~ && git clone https://ghproxy.com/https://github.com/shufflewzc/xdd-plus.git
```

然后进行编译

```bash
cd xdd-plus && go build
```

GO会下载一些东西，静静等待完成。

然后赋予权限跟运行

```bash
chmod 777 xdd
./xdd
```

等到提示更新失败进行下一步，然后不要傻傻的在xshell等待还来问哪里没弄好

## 第四步 修改配置文件

首先登录青龙面板 系统设置-应用设置-新建一个应用

命名随意，权限全给

宝塔面板打开/root/xdd-plus/conf/config.yaml

编辑config.yaml

```bash
mode: parallel
containers:
  - address: 此处写青龙面板地址 ip:port
    username: admin    青龙账号
    password: admin    青龙密码
    cid: admin         青龙xdd应用的Client id
    secret: admin      青龙xdd应用的Client secret
    weigth:  
    mode: parallel
    limit: 9999
AtTime:  #填写1-12之间的数  填错自负默认为10  10点容易出现高峰超时。
IsHelp:   #填写true或者false  false
IsOldV4: #填写true或者false  false是否新版或者旧版V4
Wskey: # 填空默认禁用wskey转换 需要的填true
IsAddFriend: #填写true或者false  false
Lim: #填写1-N 代表限制次数
Tyt: #填写1-N 代表推一推需要的互助值，默认为8
Later: #延时防止黑IP自己设置 默认60 不怕黑的改为1即可 单位是秒
theme:  
static: ./static
master: 
database:  
qywx_key:
daily_push:
resident:
user_agent:
telegram_bot_token:
telegram_user_id:
TGURL: #填写TG代理地址参考https://www.kejiwanjia.com/server/5221.html#3worker
qquid:     此处填写QQ号码 大号，不是拿来当机器人的那个号
qqgid:     此处填写QQ群号码
qbot_public_mode: true
default_priority:
no_ghproxy: true
daily_asset_push_cron:
repos:
  - git: https://github.com/shufflewzc/faker2.git
```

改完保存，重进xshell

## 第五步 运行

现在就配置好了，再输入：`cd /root/xdd-plus` 然后 `./xdd` 运行，运行的时候有个二维码 你用机器人QQ（不是你填的那个QQ哈）扫一下。这里注意，xhell窗口一定要放大，不然出来的二维码扫不出来

扫码完毕

这个窗口先不关，xshell新建一个窗口`cd /root/xdd-plus`

设置xdd静默运行 shell工具输入 `./xdd -d`。

提示：2021/11/16 15:34:52.300 [I] [deamon.go:29] 小滴滴运行于后台模式表示成功，

关闭xshell，打开QQ，发送菜单给机器人qq，有回复表示成功。然后青龙面板拉库，nolan登录复制ck发送给qq机器人

# END
