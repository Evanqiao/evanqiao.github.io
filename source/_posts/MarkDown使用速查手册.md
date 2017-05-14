title: MarkDown使用速查手册
date: 2017-4-16 21:57:36
categories: 博客建设
tags: 
 - MarkDown
toc: false
comments: true
mathjax: true
---

### 标题
```MarkDown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
```
### 字体

```
使用 * 和 ** 表示斜体和粗体。
```

**粗体**
*斜体*

### 使用html定制字体样式

```html
<font face="黑体">我是黑体字</font>
<font size= 4 face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=6 face="黑体">color=#0099ff size=6 face="黑体"</font>
<font color=#00ffff size=72>color=#00ffff</font>
<font color=gray size=72>color=gray</font>
```

<font size=4 face="微软雅黑">
[RGB颜色查询](http://www.qiaozhezou.com/rgb.htm)

</font>

效果：  
<font face="黑体">我是黑体字</font>
<font size= 4 face="微软雅黑">我是微软雅黑</font>
<font face="STCAIYUN">我是华文彩云</font>
<font color=#0099ff size=7 face="黑体">color=#0099ff size=7 face="黑体"</font>
<font color=#00ffff size=72>color=#00ffff</font>
<font color=gray size=72>color=gray</font>

```MarkDown
使用 [描述](链接地址) 为文字增加外链接。
或
[描述][1]
[1]: blog.xiaojn.cn
```

### 表格

```
| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | $1600 |   5     |
| 手机        |   $12   |   12   |
| 管线        |    $1    |  234  |
```

| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | $1600 |   5     |
| 手机        |   $12   |   12   |
| 管线        |    $1    |  234  |

### 数学公式

```MarkDown
数学公式的默认定界符是$$...$$和\\[...\\]（对于块级公式），以及$...$和\\(...\\)（对于行内公式）。
在hexo主题下要使用公式的话，需要在文件头写上：mathjax: true
hexo下面的数学公式渲染，支持的不是很好，下面的分行公式就无法渲染出来
```

```
a_k = \begin{cases}
  k & \text{for $k \le n/2$} \\
  n & \text{for $k=n/2$} \\
  k-1 & \text{otherwise}
\end{cases}
```

$$
a_k = \begin{cases}
  k & \text{for $k \le n/2$} \\
  n & \text{for $k=n/2$} \\
  k-1 & \text{otherwise}
\end{cases}
$$


```
\begin{split}\tan^2 x
  &= \sin^2 x/\cos^2 x \\
  &= 1/\cos^2 x - 1
\end{split}
```


\\[
\begin{split}\tan^2 x
  &= \sin^2 x/\cos^2 x \\
  &= 1/\cos^2 x - 1
\end{split}
\\]


```
\frac{a+b}{x+\log\dfrac{Y}{Z}}
```

$$
\frac{a+b}{x+\log\dfrac{Y}{Z}}
$$

### 插入图片

```MarkDown
![图片描述](http://pic.qiantucdn.com/58pic/13/21/43/19H58PICyxS_1024.jpg)
或
![图片描述][1]
[1]: 图片地址
```
![图片描述](http://pic.qiantucdn.com/58pic/13/21/43/19H58PICyxS_1024.jpg)


### 插入网易云音乐

```
可到网易云上复制外链
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=430 height=86 src="//music.163.com/outchain/player?type=2&id=32196944&auto=1&height=66"></iframe>
```

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=430 height=86 src="//music.163.com/outchain/player?type=2&id=32196944&auto=1&height=66"></iframe>

### 插入视频 ```
<iframe height=600 width=800 src="http://player.youku.com/embed/XNjcyMDU4Njg0">
```

<iframe height=600 width=800 src="http://player.youku.com/embed/XNjcyMDU4Njg0">


