# mariadb role #
自动安装二进制版本的mariadb数据库

## 目录结构 ##
```bash
    mariadb
    ├── files
    │   ├── mariadb-10.2.23-linux-x86_64.tar.gz
    │   └── mysql.sh
    ├── handlers
    │   └── main.yml
    ├── tasks
    │   ├── config_path.yml
    │   ├── config_service.yml
    │   ├── create_conf.yml
    │   ├── create_datadir.yml
    │   ├── create_link.yml
    │   ├── group.yml
    │   ├── initialization_db.yml
    │   ├── install_dependency.yml
    │   ├── main.yml
    │   ├── secure_installation.yml
    │   ├── unarchive.yml
    │   └── user.yml
    ├── templates
    │   └── my.cnf.j2
    └── vars
        └── main.yml
```

**files**： 目录存放mariadb的二进制压缩包和环境变量配置文件
**handlers**： 目录下的内容是配置文件发生改变后重载配置
**tasks/group.yml**： 创建mysql用户组
**tasks/user.yml**： 创建mysql用户
**tasks/unarchive.yml**： 解压压缩包到远程主机的`/usr/local/`目录下
**tasks/crerate_link.yml**： 为解压后的数据库程序目录创建软连接`mysql`
**tasks/config_path.yml**： 配置数据库的环境变量
**tasks/create_datadir.yml**： 创建数据库目录，可以在变量文件中自定义路径
**tasks/install_dependency.yml**： 安装依赖软件`libaio`
**tasks/initialization_db.yml**： 初始化数据库
**tasks/create_conf.yml**： 创建配置文件，默认使用配置文件为`/etc/mysql/my.cnf`
**tasks/config_service**：配置启动脚本并启动服务
**tasks/secure_installation.yml**： 执行安全配置脚本，设置密码，默认密码为`123456`
**tasks/main.yml**： 任务执行顺序

**templates/my.cnf.j2**： 数据库配置文件模板

**vars/main.yml**： 自定义变量，设置数据库压缩包名，数据库解压路径基名，数据库存放目录

## 调用文件 ##

```
    ---
    - hosts: test    #执行主机列表
      roles:
        - role: mariadb   #执行角色
```

## 使用 ##
将下载的mariadb数据库的二进制版压缩包放到`mariadb/files`目录，修改`mariadb/vars/main.yml`文件中的变量为合适的值

编写角色调用文件，文件名随意，然后运行`ansible-playbook xxx.yml`

> 注意：该角色仅适用于Redhat系操作系统，只在CentOS7.6-x86_64上测试过，需要远程主机已配置好YUM仓库。