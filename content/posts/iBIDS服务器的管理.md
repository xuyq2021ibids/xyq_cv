---
title: iBIDS服务器的管理
hideSummary: True
hideMeta: True
---

## 1. 添加用户

`adduser XXX —home /data1/home/xxx`

`sudo usermod -d /data1/home/wf wf`

## 2. 提权入组
`adduser XXX sudo/iBIDS`

## 3. conda配置
```shell
/opt/anaconda3/bin/conda init
vi ~/.bashrc 
conda activate jupyterLab
```
## 4. 给jupyterhub添加用户
`sudo vi /etc/jupyterhub/jupyterhub_config.py`

## 5. 待补充【一些来在公司的常用命令记录】



