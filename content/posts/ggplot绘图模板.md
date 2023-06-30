---
title: ggplot绘图模板
hideSummary: True
hideMeta: True
---




设置字体，清晰度，尺寸。

目前是一个非常合适的方图，如果图比较宽，把`width` double一下即可

```r
library(ggplot2)
library(ggalluvial)

# =============使用windowsFonts()查看字体别名=============
windowsFonts()

# > windowsFonts()
# $serif
# [1] "TT Times New Roman"
# 
# $sans
# [1] "TT Arial"
# 
# $mono
# [1] "TT Courier New"


# =============带字体的桑基图=============
titanic_wide <- data.frame(Titanic)
ggplot(data = titanic_wide,
       aes(axis1 = Class, axis2 = Sex, axis3 = Age,
           y = Freq)) +
  scale_x_discrete(limits = c("Class", "Sex", "Age"), expand = c(.2, .05)) +
  xlab("Demographic") +
  geom_alluvium(aes(fill = Survived)) +
  geom_stratum() +
  geom_text(stat = "stratum", aes(label = after_stat(stratum))) +
  theme_minimal() +
  ggtitle("passengers on the maiden voyage of the Titanic",
          "stratified by demographics and survival")+
  theme_bw()+
  theme(text=element_text(size=16, 
                          # family="mono"
                          # family="sans"
                          family="serif"
                          ))

# =============输出符合大小的图=============
ggsave("./demo1.png",
       plot = last_plot(),
       device = "png",
       path = NULL,
       scale = 1,
       width = 2000,
       height = 1500,
       units = "px",
       dpi = 300,
       limitsize = TRUE
)
```



