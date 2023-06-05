---
title: nvidia-smi挂掉了怎么办
hideSummary: True
hideMeta: True
---

# 1. 重启

# 2. 重启不行

看这个

https://blog.csdn.net/qq_40200387/article/details/90341107

`cat /proc/driver/nvidia/version`

`cat /var/log/dpkg.log | grep nvidia`

`sudo dpkg --list | grep nvidia-*`



`sudo apt-get purge nvidia*`


```bash
sudo add-apt-repository ppa:graphics-drivers
sudo apt-get update
```


`sudo apt-get install nvidia-415 nvidia-settings nvidia-prime`

`nvidia-smi`


`sudo apt-mark hold nvidia-415`（可不做，没成功过）

