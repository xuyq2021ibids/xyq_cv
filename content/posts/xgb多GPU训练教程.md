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



# DSS的华人编辑

##  Editorial Advisory Board
1. Sulin Ba, University of Connecticut, Storrs, Connecticut, United States of America

## Senior Editors

1. Zhiling Guo, Singapore Management University, Singapore, Singapore
2. Wenjing (Wendy) Duan, The George Washington University, Washington, District of Columbia, United States of America
3. Benjamin Shao, PhD, Arizona State University, Tempe, Arizona, United States of America
4. Chih-Ping Wei, PhD, National Taiwan University, Taipei, Taiwan
5. J. Leon Zhao, The Chinese University of Hong Kong, Hong Kong, Hong Kong
6. Kexin Zhao, Ph.D., UNC Charlotte, Charlotte, North Carolina, United States of America
7. Xia Zhao, UNC Greensboro, Greensboro, North Carolina, United States of America
8. Lina Zhou, UNC Charlotte, Charlotte, North Carolina, United States of America

## Associate Editors

1. Hsin-Lu Chang, PhD, National Chengchi University, Taipei, Taiwan
2. Rui Chen,Iowa State University, Ames, Iowa, United States of America
3. Zhiyuan Chen, University of Maryland Baltimore County, Baltimore, Maryland, United States of America
4. Yen-Chun Chou, PhD, National Chengchi University, Taipei, Taiwan
5. Yanqing Duan, PhD, University of Bedfordshire Business School, Luton, UK
6. Shaokun Fan, Oregon State University College of Business, Corvallis, Oregon, United States of America
7. S.M. Huang, PhD, National Chung Cheng University, Minxiong, Taiwan
8. Teng Huang, Sun Yat-Sen University, Guangzhou, China
9. Hongbing Jiang, City University of Hong Kong, Hong Kong, Hong Kong
10. Yong (Jimmy) Jin, PhD, The Hong Kong Polytechnic University, Hong Kong, Hong Kong
11. Chen Li, Fayetteville State University, Fayetteville, North Carolina, United States of America
12. D. Li, PhD, University of Minnesota Duluth, Duluth, Minnesota, United States of America
13. Der-Chiang Li, National Cheng Kung University, Tainan, Taiwan
14. Xiaobai Li, University of Massachusetts Lowell, Lowell, Massachusetts, United States of America
15. Winston Lin, PhD, University at Buffalo, Buffalo, New York, United States of America
16. Zhangxi Lin, Texas Tech University, Lubbock, Texas, United States of America
17. Mei Liu, University of Kansas Medical Center School of Nursing, Kansas City, Kansas, United States of America
18. **Xin (Robert) Luo, PhD**, The University of New Mexico, Albuquerque, New Mexico, United States of America
19. Eric Ngai, PhD, The Hong Kong Polytechnic University, Hong Kong, Hong Kong
20. Liangfei Qiu, University of Florida, Gainesville, Florida, United States of America
21. Geng Sun, The University of Texas Rio Grande Valley, Brownsville, Texas, United States of America
22. Gang Wang, University of Delaware, Newark, Delaware, United States of America
23. Jingguo Wang, The University of Texas at Arlington, Arlington, Texas, United States of America
24. Lei (Michelle) Wang, The Pennsylvania State University, University Park, Pennsylvania, United States of America
25. Jennifer Xu, PhD, Bentley University, Waltham, Massachusetts, United States of America
26. Cathy (Liu) Yang, HEC Paris, Jouy-en-Josas, France
27. Fang Yin, Senior Instructor, University of Oregon, Eugene, Oregon, United States of America
28. Dongsong Zhang, PhD, UNC Charlotte, Charlotte, North Carolina, United States of America
29. Julie Zhang, PhD,University of Massachusetts Lowell, Lowell, Massachusetts, United States of America
30. Wei Zhou, PhD, ESCP Business School, Paris, France

# DSS的大陆学者

##  Editorial Advisory Board

1. **Jae Kyu Lee, PhD**, Xi'an Jiaotong University, Xian, China

## Senior Editors



## Associate Editors

1. **Weiling Ke**, Southern University of Science and Technology, Shenzhen, Guangdong, China
2. Gang Kou, Southwestern University of Finance and Economics, Chengdu, China
3. **Shan Liu, PhD**, Xi'an Jiaotong University, Xian, China
4. Qiang Wei,Tsinghua University, Beijing, China



# 参考文献里的DSS的学者

1. Kadar, C. , ETH Zurich, Weinbergstr. 56/58, 8092 Zurich, Switzerland
2. Maculan, R. , ETH Zurich, Weinbergstr. 56/58, 8092 Zurich, Switzerland
3. Feuerriegel, S. , ETH Zurich, Weinbergstr. 56/58, 8092 Zurich, Switzerland
4. Vomfell, Lara, Warwick Business School, University of Warwick, Coventry CV4 7AL, UK
5. Härdle, Wolfgang Karl, Singapore Management University, 50 Stamford Road, Singapore 178899, Singapore
6. Lessmann, Stefan, Faculty of Business and Economics, Humboldt University of Berlin, Unter den Linden 6, Berlin 10099, Germany
7. Willing, Christoph, University of Freiburg, Germany
8. Klemmer, Konstantin, Imperial College London, United Kingdom
9. Brandt, Tobias, Rotterdam School of Management, Erasmus University, Netherlands
10. Neumann, Dirk, University of Freiburg, Germany






