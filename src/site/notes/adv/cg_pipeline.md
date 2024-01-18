---
{"dg-publish":true,"permalink":"/adv/cg-pipeline/","title":"CG Pipeline","noteIcon":""}
---

# AYON

OpenPype将会在24年开始逐步变成AYON的一部分。变为core模块。

Windows下开始使用docker部署，要方便许多。总来说，现在分为一个backend (server) 和frontend 前端页面，以及一个launcher和许多的addons。

首先部署的是服务，可以在下面的链接中根据ayon-docker中写的命令去安装。它会同时在docker容器中装好所需要的部件。(数据库，frontend，backend)。

此时就可以去浏览器页面默认为(localhost:5000)，去配置项目。

本地部署需要自己构建launcher，并上传到backend中去。构建的方式参考下面的链接。

关于安装上的一些命令，可以在这里找到。[[adv/my_develop#AYON\|Develop AYON]]

# rez

## 前言

针对流程上的工具，可以配置软件环境。例如DCC工具等。可以使用例如`rez-env <package_name>` 配置当前Shell的环境。

## 安装

### 第一步

**via pip**

```bash
# create virtualenv then install it
pip install rez
# config REZ_CONFIG_FILE point to your rezconfig.py
```

**via source**

```bash
python ./install.py
```

### 第二步

安装完成后使用`rez-bind --quickstart` 为现有软件创建rez包。

### 第三步

尝试官方的example。

```bash
cd example_packages/hello_world
rez-build --install

# 这会在你的系统里创建一个名为hello_world的包rez-env hello_world进入到这个包的环境中~
```

## 使用

1. 配置rezconfig.py文件

```bash
packages_path = [
    "~/packages",           # locally installed pkgs, not yet deployed
    "~/.rez/packages/int",  # internally developed pkgs, deployed
    "~/.rez/packages/ext",  # external (3rd party) pkgs, such as houdini, boost                  
    "<your_extra_path_here>"
]
```

2. 配置第一个包

例如想创建一个关于maya，版本2023环境的包。那么就可以在上述packages_path中的某个目录下，先创建maya（包名）这个文件夹，再创建2023（版本号）这个文件夹，再创建package.py，文件。其内容如下：

```bash
name = "maya"

version = "2023"

def commands():
    env.MAYA_UI_LANGUAGE = 'en_US'
    alias("maya2023", r"C:/Program Files/Autodesk/Maya2023/bin/maya.exe")
```

> [!INFO] commands函数定义了如何配置当前的环境。

3. 当完成上述配置后，在命令行中使用`rez-env maya`则会配置成功当前环境。
# CGRU

官方网站：[CGRU](https://cgru.info/)

## 写到前面

一个渲染农场管理软件，之前最早我捣鼓了Deadline，但是其配置之繁琐，步骤之复杂，着实被震撼到了，当然我觉得，复杂也许意味着强大。直到发现了这个工具，它的配置真的很简单，主流的DCC工具都也支持，是老牌的工具了。它需要一台电脑当server，可以是随便一台电脑。

## 准备一下

当前版本：3.3.1

1. 下载与配置

可以在这个网站下载到[Render Farm Manager, Project Tracker. - Browse Files at SourceForge.net](https://sourceforge.net/projects/cgru/files)最新版本，将它放置到一个目录中，比如我放在`c:\data\exec\cgru\v3.3.1`, 解压到这个里面，然后在这个目录中创建一个`config.json`，这里我填入的内容如下：

```json
{
	"cgru_config":
	{
		"-company":"Discover",
		"af_servername":"192.168.31.100",
		"af_serverport":51000
	}
}
```

因为默认配置文件(`config_default.json`) 中的servername默认是127.0.0.1，我们需要改成服务端电脑的ip地址，这样其它的机器就可以找到啦。这份`config.json`，在其它电脑上也需要用到~ 所以如果改动了，需要同步到其它电脑上。

> [!INFO] cgru并没有区分客户端的安装文件与服务端的安装文件，所以配置好之后，直接将v3.3.1目中压缩一份，复制到其它电脑上就行了。

2. 启动

服务启动

```bash
set CGRU_LOCATION=C:\data\exec\cgru\v3.3.1
cd /d %CGRU_LOCATION%
setup.cmd
start\AFANASY\_afserver.cmd
```

* 服务启动后就可以在浏览器中根据配置文件中填写的地址与端口查看各种信息了。

Keeper启动 - 这会启动一个桌面图标，其中一些功能就像是deadline的slaves。可以选择是否让本机执行渲染任务。在其它的电脑就只需要启动这个Keeper就可以啦。

```bash
set CGRU_LOCATION=C:\data\exec\cgru\v3.3.1
cd /d %CGRU_LOCATION%
setup.cmd
start.cmd
```


3. 选择电脑是否执行渲染任务。

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.2bzugvjljim8.webp" alt="image" width=300/>


## 提交渲染任务

这里以Blender为例子。

首先给Blender安装插件。插件在cgru的目录plugins中。具体安装就不赘述啦。

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.1z3vpdtsuqt.webp" alt="image" width=500/>

在Blender中填好渲染输出的路径后，在Render标签中找到Afanasy子标签，填写你的Job Name后就可以提交了。然后在浏览器中查看当前Job的状态。

## 更多

上面的只是非常非常简单的配置与使用。更深入的使用还是要仔细查看它的文档。在这里 https://cgru.readthedocs.io/en/latest/index.html

# OpenPype (Deprecated) 

>[!WARN] 现在openpype已经成为AYON-Core，见 [[adv/cg_pipeline#AYON\|AYON]]

## 写到前面

在捣鼓Pipeline时，从Github上发现了OpenPype这款DCC管道工具和CgWire开源的流程管理软件。
OpenPype就像是Publisher和Loader，交接棒一样进行流程中数据的管理，并且支持和CgWire的互动，满足一般工作室的需求。
目前Pype的官方提供了云支持，但代码是开源的你可以自己搭建数据库和Pype。

## 准备工作

> 系统 Windows+Docker
> Bash 其中在生成CA的部分，是使用的cgywin

1. 数据库使用的mongodb，我们为了安全，使用ca证书进行authorization，测试的时候发现ca证书和数据库如果换了电脑，就需要重新生成。

生成CA证书和公私钥 (下方的命令)

```bash
Ref: https://github.com/bitnami/containers/blob/main/bitnami/mongodb/README.md

cd  /cygdrive/c/data/db/.mongodb/ca_local

openssl genrsa -out mongoCA.key 2048

openssl req -x509 -new \
    -subj "/C=US/ST=NY/L=New York/O=Example Corp/OU=IT Department/CN=mongoCA" \
    -key mongoCA.key -out mongoCA.crt

export NODE_NAME=mongodb
openssl req -new -nodes \
    -subj "/C=US/ST=NY/L=New York/O=Example Corp/OU=IT Department/CN=${NODE_NAME}" \
    -keyout ${NODE_NAME}.key -out ${NODE_NAME}.csr


openssl x509 \
    -req -days 365 -in ${NODE_NAME}.csr -out ${NODE_NAME}.crt \
    -CA mongoCA.crt -CAkey mongoCA.key -CAcreateserial -extensions req

cat ${NODE_NAME}.key ${NODE_NAME}.crt > ${NODE_NAME}.pem


rm ${NODE_NAME}.csr


# 在docker-desktop的mongodb中的exec中执行下方命令，生成pfx格式的密钥
cd /certificates
openssl pkcs12 -inkey mongodb.key -in mongodb.crt -export -out mongodb.pfx （passwd: a1234）
openssl pkcs12 -inkey mongoCA.key -in mongoCA.crt -export -out mongoCA.pfx （passwd: a1234）
```

使用compose.yaml配置文件和.env配置文件通过使用docker-desktop搭建mongodb数据库

2. 创建好这两个文件

.env

```env
### MongoDB configuration

MONGODB_VERSION=7.0.2
MONGODB_DATA=C:/data/db/.mongodb/mongodb_local
MONGODB_CA=C:/data/db/.mongodb/ca_local
MONGODB_CONF=C:/data/db/.mongodb/local_config
ALLOW_EMPTY_PASSWORD=no
MONGODB_ROOT_PASSWORD=tC0jsi16gEqx
MONGODB_REPLICA_SET_KEY=mL7sArv24C4o
```

compose.yaml

```yaml

volumes:
  mongodb_data: { driver: local }

services:
  mongodb:
    image: docker.io/bitnami/mongodb:${MONGODB_VERSION:-6.0}
    restart: always
    volumes:
      - "${MONGODB_DATA}:/bitnami/mongodb"
      - "${MONGODB_CA}:/certificates"
      - "${MONGODB_CONF}:/bitnami/mongodb/conf"
    environment:
      MONGODB_REPLICA_SET_MODE: primary
      MONGODB_REPLICA_SET_NAME: ${MONGODB_REPLICA_SET_NAME:-rs0}
      MONGODB_PORT_NUMBER: ${MONGODB_PORT_NUMBER:-27018}
      MONGODB_INITIAL_PRIMARY_HOST: ${MONGODB_INITIAL_PRIMARY_HOST:-mongodb}
      MONGODB_INITIAL_PRIMARY_PORT_NUMBER: ${MONGODB_INITIAL_PRIMARY_PORT_NUMBER:-27018}
      MONGODB_ADVERTISED_HOSTNAME: ${MONGODB_ADVERTISED_HOSTNAME:-mongodb}
      MONGODB_ENABLE_JOURNAL: ${MONGODB_ENABLE_JOURNAL:-true}
      ALLOW_EMPTY_PASSWORD: ${ALLOW_EMPTY_PASSWORD:-yes}
      MONGODB_ROOT_PASSWORD: ${MONGODB_ROOT_PASSWORD:-password123}
      MONGODB_REPLICA_SET_KEY: ${MONGODB_REPLICA_SET_KEY:-replicasetkey123}
      MONGODB_EXTRA_FLAGS: --tlsMode=preferTLS --tlsCertificateKeyFile=/certificates/mongodb.pem --tlsClusterFile=/certificates/mongodb.pem --tlsCAFile=/certificates/mongoCA.crt
    expose:
      - ${MONGODB_PORT:-27018}
    ports:
      - "${MONGODB_BIND_IP:-0.0.0.0}:${MONGODB_HOST_PORT:-27018}:${MONGODB_PORT:-27018}"
```

3. cd到这两个文件的文件夹中，调用`docker compose up -d`命令，执行成功后如果没有报错就可以使用一些工具来测试数据库是否创建成功了。

> mongosh来连接数据库并创建用户

```bash
# 连接
"C:\data\port\mongosh-1.10.6-win32-x64\bin\mongosh" --port 27018 -u root -p tC0jsi16gEqx --authenticationDatabase admin

# 使用admin数据库
use admin

# 创建用户 (测试的时候发现Pype并没有自己的用户管理，是通过数据库用户来进行权限管理的，真的牛)
db.createUser( { user: "aaron", pwd: "secretpassword", roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] } ) 

db.createUser( { user: "Mohan", pwd: "password", roles:[{role: "read" , db:"openpype"}, {role: "readWrite" , db:"avalon"}] } ) 

db.grantRolesToUser( "ll", [{role:"read", db:"openpype"}, {role:"readWrite", db:"avalon"}] ) 

# 改变用户的密码
db.changeUserPassword("root", "secretpassword") 
# 删除用户
db.dropUser("siteUserAdmin") 
# 列出所有用户
db.system.users.find() 
# 改变用户的权限
db.grantRolesToUser('aaron', ['readWriteAnyDatabase']);

```

## 其它

或者不要像上面那样，那么麻烦，如果你有一台vps主机，可以参考如果安装mongodb在那台vps上。可以见[[adv/os_linux\|OS Linux]]中如果安装Mongodb。

## 从源码编译

> 截至至3.17.3版本，使用Python3.9

从 https://github.com/ynput/OpenPype 仓库的Readme中参考

1. 首先Clone原仓库使用`git clone --recurse-submodules git@github.com:ynput/OpenPype.git`

2. 然后打开一个PowerShell，进入到仓库的文件夹里，执行`.\tools\create_env.ps1` 创建虚拟venv环境

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.2lyughc8ikm0.webp" alt="image" />

3. 再执行`.\tools\fetch_thirdparty_libs.ps1`下载第三方依赖库
4. 执行`.\tools\build.ps1`去构建Pype，执行成功后在build文件中就会有`openpype_gui.exe`和`openpype_console.exe`这两个程序


## 配置Pype

可以使用root账号，登录Mongodb数据库，然后执行

```
# 创建一个管理员账号
db.createUser( { user: "aaron", pwd: "<password>", roles: [ { role: "userAdminAnyDatabase", db: "admin" } ] } )

# 给账号设置权限 (openpype只读，这是一些系统设置，avalon需要可读可写)
db.grantRolesToUser( "aaron", [{role:"read", db:"openpype"}, {role:"readWrite", db:"avalon"}] )
```

然后就可以在![image](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.1pej7ki22chs.png)中选择 Admin -> Studio Settings -> Module (Kistu) 中填入你的cgwire的地址与端口，例如我填写的是`http://localhost:25061` 然后点击保存设置

此时再去执行 (一个是Module用于同步Kitsu的项目数据，一个是启动OpenPype) 

> 我这里是Windows系统下

```bash
@echo off

set "KITSU_LOGIN=lilong999000@gmail.com"
set "KITSU_PWD=<password>"
set OPENPYPE_MONGO=mongodb://aaron:<password>@localhost:27018
C:\data\exec\openpype\build\openpype_console.exe module kitsu sync-service

pause
```

```bash
@echo off

set OPENPYPE_MONGO=mongodb://aaron:<password>@localhost:27018
C:\data\exec\openpype\build\openpype_console.exe

pause
```


DCC 的配置 在 Admin -> Studio Settings -> Applications中，可以选择启用哪些DCC，也可以在这里面配置环境变量等等![image](https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.39v4jdcijps0.webp) 例如这里是我配置的关于Houdini的一个，配置了RenderMan渲染插件~ 如果Pype没有列出你的DCC版本也可以在下方的Key和Label中自己去配置一个~


有了这些全部的设置，就可以尽情去捣鼓了~

## 从仓库下载

e.g. OpenPype · GitHub https://github.com/ynput/OpenPype/releases/tag/3.17.3 下载下来的版本运行与从源码构建的版本是一致的。


## 其它

### Cgwire的导出与导入

如果需要迁移整个cgwire，因为是在docker中创建的容器，所以可以将整个容器import和export

> [!WARNING] 这样运行docker命令后，默认数据是存储到docker的volume中，我们可以导出当前的docker的数据到本地，再导入，这样volume就被取消了，虽然不知这样是否安全

当在第一次创建cgwire后，可以立马执行
`docker export cgwire-run > cgwire-run-export.tar` 
再执行
`docker import cgwire-run-export.tar cgwire-run`，

这个时候再去导出和导入这个容器数据就也在其中了
`docker export cgwire-run > cgwire-run-export.tar`
再执行
`docker import cgwire-run-export.tar cgwire-run`

最后执行
`docker run --user=root --env=PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin --env=DEBIAN_FRONTEND=noninteractive --env=PG_VERSION=12 --env=DB_USERNAME=root --env=DB_HOST=  --workdir=/opt/zou -p 25061:80 --restart=no --runtime=runc -d --name cgwire-run cgwire-run /opt/zou/start_zou.sh`

> 查看command的命令`docker ps -a --no-trunc`,上面的启动命令就是用这个命令查询出来的command，以及其它的包括环境变量的配置是在docker里copy docker run

* 但这样不好的是，丢失了元数据以及image的数据

# Cgwire

[Installation | Kitsu Documentation](https://kitsu.cg-wire.com/installation/)

虽然Pype中也有Project Manage，但是我们可以扩展使用Cgwire去进行项目的管理与创建。

执行 `docker run -d -p 25061:80 --name cgwire-run cgwire/cgwire:0.17.37` 去开启cgwire服务，这里设置的是为防止污染80端口而改为25061端口，版本为截至现在最新的0.17.37，这里可以改为latest选择最新版本

成功后在浏览器打开localhost:25061，初次登录用户名与密码是

```
- login: admin@example.com
- password: mysecretpassword
```

进去之后就可以创建用户，创建部门，资产类型等等，这里我创建了一个名为Discover的项目。

<img src="https://cdn.jsdelivr.net/gh/aaronmack/image-hosting@master/e/image.5q4k52oe0q80.webp" alt="image" />
