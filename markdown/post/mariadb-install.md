---
title: mariadb-install
cover: 'https://cdn.jsdelivr.net/gh/iahccc/PicBed/img/archive_img.jpeg'
date: 2021-03-05 06:50:11
tags: mariadb
categories: 软件安装
---

# MariaDB安装注意事项：
1. 安装完成后需执行数据库安装脚本:
    mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql
2. 执行mysql_secure_installation初始化密码
