---
title: jupyter常用配置
hideSummary: True
hideMeta: True
---




jupyter的常用配置

有时候没法用pycharm，有需要交互的调参，就需要jupyter配置一下。
我一般习惯于用jupyterlab

## 1. jupyter-lab安装包
`conda create -n YOUR_ENVIRONMENT python pandas seaborn xgboost`

`conda install jupyterlab -c conda-forge`

## 2. python版本的ggplot安装包
`conda install plotnine -c conda-forge`

## 3. jupyterlab-lsp自动补全代码的官方建议
- `conda install -c conda-forge 'jupyterlab>=3.0.0,<4.0.0a0' jupyterlab-lsp`

- `conda install -c conda-forge python-lsp-server r-languageserver`

由于已经安过了jupyterlab，也不需要r的服务，直接用下面这个命令就行\
`conda install -c conda-forge jupyterlab-lsp python-lsp-server`
## 4. jupyterlab-code-formatter代码自动格式化
还有一个自动formatter的服务，自动格式化jupyter的服务
`conda install -c conda-forge jupyterlab-code-formatter`
老装不上

先装一个`balck`和`isort`用`conda`，再用`pip`装`jupyterlab-code-formatter`
这两个瓜皮（`balck`和`isort`）都不熟悉，还是用`autopep8`好了
尽量避免conda和pip混用

装好autopep8之后去jupyterlab的setting里把`jupyterlab-code-formatter`的默认引擎换成autopep8

最后重启jupyterlab服务，而不是内核

## jupyterlab添加或删除内核
- 切换到要添加的环境，确认已安装ipykernel
  - `python -m ipykernel --version`
  - 如果没有安装，则安装：`conda install ipykernel`
- jupyter安装内核（kernel）
  - `python -m ipykernel install --user --name=xxx --display-name "在notebook中显示的环境名"` xxx是你的虚拟环境名称
- 查看jupyter notebook kernel
  - jupyter kernelspec list
- jupyter删除内核
  - `jupyter kernelspec remove kernelname` kernelname是你的虚拟环境名称（即上面添加的内核名称）
