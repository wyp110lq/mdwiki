# 新一代托管系统_websphere单机部署手册
## 1. websphere安装

安装所需要的软件包:

InstalMgr1.6.2_LNX_X86_64_WAS_8.5.5.zip

WAS_ND_V8.5_1_OF_3.zip

WAS_ND_V8.5_2_OF_3.zip

WAS_ND_V8.5_3_OF_3.zip

把安装的软件包放到/home/was8.5/installer路径

输入命令:unzip InstalMgr1.6.2_LNX_X86_64_WAS_8.5.5.zip 

解压 InstalMgr1.6.2_LNX_X86_64_WAS_8.5.5.zip 包

![avatar](../images/install.png)

执行安装程序：

输入命令进入路径：cd /home/was8.5/installer

输入命令：./install     （./consoleinst.sh）

## 2、创建部署包目录
### 2.1 创建容器存放文件夹
在管理集群应用服务器文件目录中创建一个存放war包的文件夹，如创建名叫sofa文件夹（创建的文件夹有读写及执行等权限），将sofa**.war上传到sofa文件目录下:

![avatar](../images/1.png)

并执行解压命令：
jar –xvf sofa**.war
![avatar](../images/2.png)

解压后相应目录中有web-inf及meta-inf两个文件。如图:

![avatar](../images/3.png)

解压war包：jar –xvf sofa**.war

打war包：  jar -cvf acs.war ./*     
### 2.2 存放sofa_home目录
把sofa_home文件夹上传到/home下
![avatar](../images/5.png)
![avatar](../images/6.png)
## 3、参数配置
## 4、部署应用服务
## 5、sofa平台初始化
## 6、数据库部署脚本
## 7、其他配置
## 8、升级JDK
## 9、删除概要文件