---
title: xgb多GPU训练教程
hideSummary: True
hideMeta: True
---

# 1. 背景

xgb在gpu训练上速度很快，单个就几秒钟解决。但是涉及到超参数的搜索，希望速度更快，因而需要多卡训练。

想象一下假设有6个参数，每个有6个取值，6^6=4.6万个参数组合。即使训练一轮只需要30秒，全部搜索下来也需要16天。如果有四个显卡加速，就只需要4天。

# 2. 首先安装环境和各种用得到的包

## 2.1. 创建一个py0的环境
`conda create -n py10 python=3.10`

目前cudf仅仅支持到3.10，不要冲到3.11去了

## 2.2. 各种包

`conda install -c conda-forge jupyterlab jupyterlab_code_formatter pandas seaborn plotnine python-lsp-server autopep8  py-xgboost-gpu scikit-learn dask dask-ml dask-cuda -y`

## 2.3. 这个dask-cudf比较难搞

先装`cudf`
`conda install -c rapidsai -c conda-forge -c nvidia cudf=23.04 cudatoolkit=11.8`

再装`dask-cudf`
`conda install -c rapidsai dask-cudf`


# 3. 编一个数据集





