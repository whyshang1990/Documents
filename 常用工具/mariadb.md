# MariaDB

## Windows 10 安装

- 下载zip包，解压后进入bin目录
- 安装数据库`./mysql_install_db.exe`
- 执行命令`./nysqld.exe --install mariadb`新建服务,此处mariadb为启动服务名
`Service successfully installed.`表示安装成功
- 启动服务 `net start mariadb`(关闭服务`net stop mariadb`)
