##绘制基础

参考：[HenCoder Android 开发进阶: 自定义 View 1-1 绘制基础](http://hencoder.com/ui-1-1/)

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

##Canvas 对绘制的辅助 clipXXX() 和 Matrix
参考[HenCoder Android 开发进阶：自定义 View 1-4 Canvas 对绘制的辅助 clipXXX() 和 Matrix](http://hencoder.com/ui-1-4/)

###范围裁切clip

###几何变换

* 使用 Canvas 来做常见的二维变换
* 使用 Matrix 来做常见和不常见的二维变换
* 使用 Camera 来做三维变换

##绘制顺序

参考：[HenCoder Android 开发进阶：自定义 View 1-5 绘制顺序](http://hencoder.com/ui-1-5/)

###绘制过程

包括

* 背景
* 主体（onDraw()）
* 子 View（dispatchDraw()）
* 滑动边缘渐变和滑动条
* 前景

![image](https://ws4.sinaimg.cn/large/006tKfTcly1fiiwb2nr63j30ga0bddgg.jpg)

```
// View.java 的 draw() 方法的简化版大致结构（是大致结构，不是源码哦）：

public void draw(Canvas canvas) {  
    ...

    drawBackground(Canvas); // 绘制背景（不能重写）
    onDraw(Canvas); // 绘制主体
    dispatchDraw(Canvas); // 绘制子 View
    onDrawForeground(Canvas); // 绘制滑动相关和前景

    ...
}
```

![image](https://ws2.sinaimg.cn/large/006tKfTcly1fiix28rb6mj30ru0c8jsb.jpg)

注意：

* 出于效率的考虑，ViewGroup 默认会绕过 draw() 方法，换而直接执行 dispatchDraw()，以此来简化绘制流程。所以如果你自定义了某个 ViewGroup 的子类（比如 LinearLayout）并且需要在它的除  dispatchDraw() 以外的任何一个绘制方法内绘制内容，你可能会需要调用 View.setWillNotDraw(false) 这行代码来切换到完整的绘制流程（是「可能」而不是「必须」的原因是，有些 ViewGroup 是已经调用过 setWillNotDraw(false) 了的，例如 ScrollView）。
* 有的时候，一段绘制代码写在不同的绘制方法中效果是一样的，这时你可以选一个自己喜欢或者习惯的绘制方法来重写。但有一个例外：如果绘制代码既可以写在 onDraw() 里，也可以写在其他绘制方法里，那么优先写在 onDraw() ，因为 Android 有相关的优化，可以在不需要重绘的时候自动跳过  onDraw() 的重复执行，以提升开发效率。享受这种优化的只有 onDraw() 一个方法。

###总结

![总结](https://ws3.sinaimg.cn/large/006tKfTcly1fii5jk7l19j30q70e0di5.jpg)

* 在 ViewGroup 的子类中重写除 dispatchDraw() 以外的绘制方法时，可能需要调用  setWillNotDraw(false)；
* 在重写的方法有多个选择时，优先选择 onDraw()。

##属性动画（上）

参考：[HenCoder 自定义绘制的第 1-6 期：属性动画 Property Animation（上手篇）](http://hencoder.com/ui-1-6/)

###Interpolator 

其实就是速度设置器，设置动画运行的速度。

##属性动画（下）

参考：[HenCoder 自定义绘制的第 1-7 期：属性动画 Property Animation（进阶篇）](http://hencoder.com/ui-1-7/)

###TypeEvaluator

其他类型属性动画

###ArgbEvaluator

颜色渐变动画，在 Android 5.0 （API 21） 加入了新的方法 ofArgb()。

###自定义 Evaluator

###ofObject()

借助于 TypeEvaluator，属性动画就可以通过 ofObject() 来对不限定类型的属性做动画了。

TypeEvaluator：针对特殊的属性来做属性动画，它可以让你「做到本来做不到的动画」，针对复杂的属性关系来做动画，它可以让你「能做到的动画做起来更简单」

###PropertyValuesHolder 同一个动画中改变多个属性

如果有多个属性需要修改，可以把它们放在不同的 PropertyValuesHolder 中，然后使用 ofPropertyValuesHolder() 统一放进  Animator。

###AnimatorSet 多个动画配合执行

###PropertyValuesHolders.ofKeyframe() 把同一个属性拆分

除了合并多个属性和调配多个动画，你还可以在 PropertyValuesHolder 的基础上更进一步，通过设置  Keyframe （关键帧），把同一个动画属性拆分成多个阶段。

「关于复杂的属性关系来做动画」，就这么三种：

* 使用 PropertyValuesHolder 来对多个属性同时做动画；
* 使用 AnimatorSet 来同时管理调配多个动画；
* PropertyValuesHolder 的进阶使用：使用 PropertyValuesHolder.ofKeyframe() 来把一个属性拆分成多段，执行更加精细的属性动画。

###ValueAnimator 最基本的轮子

ValueAnimator 就是一个不能指定目标对象版本的 ObjectAnimator。

ViewPropertyAnimator、ObjectAnimator、ValueAnimator 这三种 Animator，它们其实是一种递进的关系：从左到右依次变得更加难用，也更加灵活。

它们的性能是一样的，因为 ViewPropertyAnimator 和 ObjectAnimator 的内部实现其实都是 ValueAnimator，ObjectAnimator 更是本来就是 ValueAnimator 的子类，它们三个的性能并没有差别。它们的差别只是使用的便捷性以及功能的灵活性。所以在实际使用时候的选择，只要遵循一个原则就行：尽量用简单的。能用 View.animate() 实现就不用 ObjectAnimator，能用 ObjectAnimator 就不用 ValueAnimator。