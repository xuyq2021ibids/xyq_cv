---
title: linux常用命令
hideSummary: True
hideMeta: True
---



## 1. [Jupyter Lab 密码登录、远程访问](https://blog.csdn.net/qq_27370437/article/details/117845115)


## 2. [后台挂起某个任务](https://zhuanlan.zhihu.com/p/617627144)
- `nohup jupyter-lab --port 9990  --ip 0.0.0.0 --no-browser > stdout.txt 2> stderr.txt &`
- 使用 jobs 命令可以查看当前 shell 中后台运行的任务列表，包括任务编号、状态和命令。

## 3. 查看某个用户的进程
`top -u xuyuquan`

## 4. 查找某个特定进程
以下这条命令是检查sshd进程是否存在：
`ps -ef | grep sshd`

## 5. 杀死某个特定进程
`kill -9 PID`

## 6. 创建用户
`adduser UserName --home the_path_to_home`

`usermod –aG wheel UserName/wheel`

`passwd UserName`

## 7. 查看端口
- 查看某个端口是否被占用
  - `netstat -anp |grep 端口号`
- 查看已经使用的端口号
  - `netstat -nultp（此处不用加端口号）`

## 8. 删除用户
想要完全删除用户账号（也就是删除所有与该用户相关的文件），以下这两种方法个人觉得是最好的：
（1）使用`userdel -r xiaoluo`命令删除。
（2）先使用`userdel xiaoluo`删除账户和组的信息，在使用find查找所有与该用户的相关文件，在使用`rm -rf`删除
先使用`userdel xiaoluo`删除账户和组的信息，再使用【`find / -name "*xiaoluo*"`】查找所有于该用户的相关文件，在使用`rm -rf`删除

## 9. [Linux下的用户配置文件](https://zhuanlan.zhihu.com/p/435749651)
主要就是四个文件
- `/etc/passwd`
- `/etc/shadow`
- `/etc/group`
- `/etc/gshadow`

### 9.1. 用户信息文件/etc/passwd
第一字段：用户名称

第二字段：密码标志

第三字段：UID

0：超级用户

1-499：系统用户

500-65535：普通用户

第四字段：GID（用户初始组ID）

第五字段：用户说明

第六字段：家目录，普通用户：/home/用户名/ 超级用户/root/

第七字段：登录之后的SHELL

### 9.2. 影子文件/etc/shadow

第一字段：用户名

第二字段：加密密码，加密算法升级为SHA512散列加密算法，如果密码位是“！！”或“*”代表没有密码，不能登录

第三字段：密码最后一次修改日期，使用1970年1月1日作为标准时间，每过一天时间戳加一

第四字段：两次密码的修改时间间隔时间（和第三字段相比）

第五字段：密码有效期（和第三字段相比）

第六字段：密码修改到期的警告天数（和第五字段相比）

第七字段：是密码过期后的宽限天数（和第5字段相比）0：代表密码过期后立即生效，-1：则代表密码永远不会失效。

第八字段：账号失效时间，要用时间戳表示

第九字段：保留

### 9.3. 组信息文件/etc/group和组密码文件/etc/gshadow

第一字段：组名

第二字段：组密码标志

第三字段：GID

第四字段：组中附加用户


## 10. linux下的压缩和解压缩tar命令
解包：`tar -zxvf FileName.tar`

打包：`tar -czvf FileName.tar [-C DirName .（or其他目录名）][DirName]`

`zip -rq FileName.zip FileName`

**`zip`就是个煞笔命令，不能跳转指定目录。不在当前目录压缩就会把绝对路径带进去，坚决不要使用这个傻逼。**


## 11. `&`的意义
放在命令最后，代表这条命令放到后台去执行，此时可以执行新的命令了。

## 12. `nohub`的意义
全称 no hangup 即不挂起，即使关掉shell，依然会继续执行。

`&`和`nohub`的区别和常见使用场景
`&`代表后台执行，免疫SIGINT信号（`Ctrl+C`），但是不免疫SIGHUP信号（关闭shell）；`nohub`相反，关闭shell仍会执行，但是`Ctrl+C`可以中断执行。因此可以将这两个联合使用，让进程既不受`Ctrl+C`影响，也不受shell关闭影响，类似守护进程。

## 13. 多窗口管理

一种方式是用nhup，还有一种是[tmux](https://www.ruanyifeng.com/blog/2019/10/tmux.html)

[一文助你打通 tmux](https://zhuanlan.zhihu.com/p/102546608)

### 13.1. 新建会话和列出会话
```bash
$ tmux
$ tmux new -s <session-name>
$ tmux ls
```
### 13.2. 切换会话
```bash
# 使用会话编号
$ tmux switch -t 0
# 使用会话名称
$ tmux switch -t <session-name>
```
### 13.3. 杀掉会话
```bash
# 使用会话编号
$ tmux kill-session -t 0
# 使用会话名称
$ tmux kill-session -t <session-name>
# 或者在session里面按ctrl+d
ctrl + d
```
### 13.3. 退出会话和再次进入
`ctrl + b, d`
```bash
$ tmux a -t session_name_or_id
```

### 13.4. 新建窗口
`ctrl + b, % 或者 "`

### 13.5. 切换窗口
`ctrl + b, 光标`

### 13.6. 关闭窗口
`ctrl + b, x`


