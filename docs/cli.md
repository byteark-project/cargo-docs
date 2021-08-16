# CLI 使用文档

## endpoint
##### 1.配置操作的端点
```shell
cs endpoint set devel_admin https://ENDPOINT-SERVER MYUSER MYPASSWORD
```
##### 2.查看配置的端点信息
```shell
cs endpoint list devel_admin (cs endpoint list)
```
##### 3.删除已配置的端点信息
```shell
cs endpoint remove devel_admin
```
## mb
##### 1.创建一个存储桶
```shell
cs mb devel_admin/byteark
```
##### 2.创建一个多级目录的存储桶（mkdir -p）
```shell
cs mb devel_admin/byteark/myself/imgs
```
##### 3.创建多个存储桶
```shell
cs mb devel_admin/byteark devel_admin/byteark2
```
##### 4.忽略文件已存在（若文件夹中有文件将会被删除掉）
```shell
cs mb --ignore-existing devel_admin/byteark
```
## rb
##### 1.删除一个存储桶
```shell
cs rb devel_admin/byteark
```
##### 2.删除端点下的所有存储桶
```shell
cs rb --force --dangerous devel_admin
```
## cp
##### 1.从本地上传一个文件到端点服务器
```shell
cs cp Music/*.txt devel_admin/byteark
```
##### 2.从本地上传整个文件夹到一个端点服务器
```shell
cs cp --recursive /Users/mac/Desktop/ devel_admin/byteark
```
##### 3.从本地上传整个文件夹到多个端点服务器
```shell
cs cp --recursive /Users/mac/Desktop/ devel_admin/byteark devel_admin/byteark2 devel_admin/byteark
```
##### 4、将远程端点的数据拷贝到本地
```shell
cs cp --recursive devel_admin/byteark /Users/mac/Desktop/
```
## rm
##### 1.删除文件
```shell
cs rm devel_admin/byteark/file.txt
```
##### 2.删除一个文件下的所有文件以及子目录
```shell
cs rm devel_admin/byteark
```
##### 3.清空端点下的所有文件（存储桶不会被删除）
```shell
cs rm rm --recursive --force --dangerous devel_admin
```
##### 4.保留7到10天内新建的文件
```shell
cs rm --recursive --force --newer-than 7d --older-than 10d devel_admin/byteark
```
## mirror
##### 1.将本地的文件迁移到远程端点服务器
```shell
cs mirror /Users/mac/Desktop/ devel_admin/byteark
```
##### 2.数据同步到本地
```shell
cs mirror devel_admin/byteark /Users/mac/Downloads/
```
##### 3.将一个远程端点中的文件迁移到另一个端点
```shell
cs mirror devel_admin/byteark test/byteark
```
##### 4.将本地数据同步到远程服务器（文件存在时将覆盖）
```shell
cs mirror --overwrite /Users/mac/Downloads/ devel_admin/byteark
```
##### 5.模拟同步本地数据到远程仓库
```shell
cs mirror --fake /Users/mac/Downloads/ devel_admin/byteark
```
##### 6.将本地数据同步到远程仓库，监控变化
```shell
cs mirror --watch /Users/mac/Downloads/ devel_admin/byteark
```
##### 7.将本地数据同步到远程仓库，删除服务器不同本地的文件
```shell
cs mirror --remove /Users/mac/Downloads/ devel_admin/byteark
```
##### 8.将本地数据同步到远程仓库，校验文件md5值进行覆盖
```shell
cs mirror --md5 /Users/mac/Downloads/ devel_admin/byteark
```
## stat
##### 1.查看存储桶的信息
```shell
cs stat devel_admin/byteark
```
##### 2.查看文件的信息
```shell
cs stat devel_admin/byteark/file.txt
```
##### 3.查看文件夹下所有文件的信息
```shell
cs stat --recursive devel_admin/byteark
```
## mv
##### 1.移动本地文件到文件服务器
```shell
cs mv /Users/mac/Desktop/file.txt devel_admin/byteark
```
##### 2.移动本地文件夹内所有文件到文件服务器
```shell
cs mv --recursive /Users/mac/Desktop devel_admin/byteark
```
##### 3.移动文件服务器文件到本地
```shell
cs mv devel_admin/byteark/file.txt /Users/mac/Desktop
```
##### 4.移动文件服务器文件夹内所有文件到本地
```shell
cs mv --recursive devel_admin/byteark /Users/mac/Desktop
```
## tree
##### 1.以树形目录展示端点数据
```shell
cs tree devel_admin
```
##### 2.以树形目录展示端点数据，文件信息也展示出来
```shell
cs tree --files devel_admin
```
