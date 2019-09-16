# DMP导入篇

## 1登录系统

登录linux系统上的oracle数据库，默认数据泵导出的文件是放在/home/oracle/dmp下面。

## 2切换oracle用户环境变量

用root登录oracle数据库所在的linux系统,执行下面语句：
```
cd /home/oracle/
source  .bash_profile   --用户变量
su oracle   --切换到oracle用户
```

## 3输入导入语句并执行

  impdp 导入的用户名/密码 directory=dump_dir dumpfile=命名.dmp REMAP_SCHEMA=DMP的用户名:导入的用户名REMAP_TABLESPACE=DMP用户的表空间:导入用户的表空间full=y logfile=日志命名.log


举例：
```
impdp sofaqy/sofaqy directory=dump_dir dumpfile=sofa1130_20180625.dmp REMAP_SCHEMA=sofa1130:sofaqy REMAP_TABLESPACE=HXJJTG:HXJJTG full=y logfile=sofaqy.log
```

备注：
查看用户和默认表空间的关系。  
username 需要大写
select   username ,default_tablespace from   dba_users  where username = 'GFSMZQSOFA'

