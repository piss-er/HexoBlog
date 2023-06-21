---
title: MySQL忘密码了怎么办
---
[教程原文](http://t.csdn.cn/mFZZj)
1. 管理员模式打开cmd，如果还开着mysql服务那先关掉`net stop mysql`
   用如下命令打开越权模式
   ```mysqld --console --skip-grant-tables --shared-memory```
   这样可以越权模式打开mysql，保留这个黑窗口
   ![](https://img.gejiba.com/images/3267fbb2193cabd2d0f97388665052c5.png)
2. 用管理员身份打开一个cmd
   ```mysql -u root -p```
   让输入密码时无需输入直接回车就可以进入root用户
   （上一步没有关闭mysql服务这一步就会失败）
3. 刷新权限，不然不允许改密码
   `flush privileges;`
4. 改密码为123
   ```alter user 'root'@'localhost' identified by '123';```
   
5. 再刷新一下权限就可以了
   ```flush privileges;```
![](https://img.gejiba.com/images/d30c87e2c9bb2d6ca0eb22fb855ad4df.png)