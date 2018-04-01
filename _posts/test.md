```
>library(ggplot2)
> library(gridExtra)
> data("diamonds")
> ggplot(subset(diamonds, carat >= 2.2),aes(x = table, y = price, colour = cut)) +geom_point(alpha = 0.7) +
geom_smooth(method = "loess", alpha = 0.05, size = 1, span = 1) +theme_bw()
> p1 = ggplot(subset(diamonds, carat >= 2.2),aes(x = table, y = price, colour = cut)) +geom_point(alpha = 0.7) +
geom_smooth(method = "loess", alpha = 0.05, size = 1, span = 1) + theme_bw()
```
此时
```
>p1
```


```
>p2
```

```
p1_npg = p1 + scale_color_npg()
```


```
p1_npg +theme(panel.grid = element_blank(),axis.ticks=element_blank(),panel.border = element_blank(),axis.line=element_line(colour="black",arrow=arrow(length=unit(0.20,"npc"))))
```

```
p2_npg = p2 + scale_fill_npg()
```


```
p2_npg+theme(panel.grid = element_blank(),axis.ticks=element_blank(),panel.border = element_blank(),axis.line=element_line(colour="black",arrow=arrow(length=unit(0.20,"npc"))))
```


