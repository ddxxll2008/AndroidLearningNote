##Paint详解

参考：[HenCoder Android 开发进阶: 自定义 View 1-2 Paint 详解](http://hencoder.com/ui-1-2/)

###关于颜色的三层设置

直接设置颜色的 API 用来给图形和文字设置颜色； setColorFilter() 用来基于颜色进行过滤处理； setXfermode() 用来处理源图像和  View 已有内容的关系。

颜色渐变用Shader，用图像来填充用BitmapShader，颜色与图像的叠加用PorterDuff.Mode

ColorFilter，对颜色进行处理，移除颜色或者增加颜色，进行颜色合成，或者对颜色进行处理（使用矩阵）

Xfermode() 用来处理源图像和  View 已有内容的关系

![image](https://ws2.sinaimg.cn/large/006tNc79ly1fig738su5oj30j909ymy2.jpg)

###色彩优化

抖动，双线性过滤，路径变化，阴影，模糊，获取实际的绘制路径，获取文字路径

##绘制文字

参考[HenCoder Android 开发进阶：自定义 View 1-3 drawText() 文字的绘制](http://hencoder.com/ui-1-3/)

###drawText

注意参数坐标是文字的左下方位置

StaticLayout：文字换行

绘制文字时，可以设置大小，字体，伪粗体，删除线，下划线，斜度，横向放缩，字符间距，用 CSS 的 font-feature-settings 的方式来设置文字，对齐方式，设置绘制所使用的 Locale（地域）等。

###测量文字

获取推荐行距，获取 Paint 的 FontMetrics（包括top, ascent, baseline, descent, bottom），获取文字显示范围，测量文字宽度，获取字符串中每个字符的宽度，获取设置宽度下的文字个数，以及光标相关的方法。