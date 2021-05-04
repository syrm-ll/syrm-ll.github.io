# transition：渐变

## 兼容性

| 属性           | Chrome                | Edge | 火狐               | Safari               | 欧朋              |
| -------------- | --------------------- | ---- | ------------------ | -------------------- | ----------------- |
| **transition** | 26.0<br/>4.0 -webkit- | 10.0 | 16.0<br/>4.0 -moz- | 6.1<br/>3.1 -webkit- | 12.1<br/>10.5 -o- |

紧跟在 -webkit-, -ms- 或 -moz- 前的数字为支持该前缀属性的第一个浏览器版本号。

**语法**

```css
transition: 属性名：必须   持续时间：默认：0   时间曲线   延迟时间：默认0
transition: property duration timing-function delay;
或
transition: property duration timing-function delay，
			property duration timing-function delay，
			property duration timing-function delay;

默认值：	all 0 ease 0
```

四个属性可以拆开写为：

```css
transition-property			属性
transition-duration			时间
transition-timing-function	时间曲线
transition-delay			延迟时间
```

## 子属性列表

### transition-property：属性名

transition-property属性指定CSS属性的nametransition效果（transition效果时将会启动指定的CSS属性的变化）。

**语法**

```css
transition-property: none | all | property;
```

| 值       | 描述                                |
| -------- | ----------------------------------- |
| none     | 空，不设置任何属性的过渡效果。      |
| all      | 所有， 设置全部属性的过渡效果。     |
| property | CSS属性名，设置指定样式的过渡效果。 |

---

### transition--duration：过渡时间

transition-duration 属性规定完成过渡效果需要花费的时间（以秒或毫秒计）。

**语法**

```css
transition-duration: time;
```

| 值   | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| time | 完成过渡效果需要的时间，以秒（s） 或 毫秒（ms）为单位。<br />意味着没有任何效果。 |

---

### transtition-timing-function：时间曲线

transition-timing-function属性指定切换效果的速度。

**语法**

```css
transition-timing-function: liner | ease | ease-in | ease-out
							| ease-in-out | cubic-bezier(n,n,n,n);
```

| 值                    | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| linear                | 匀速，水平速度曲线；等效于cubic-bezier(0,0,1,1)              |
| ease                  | 慢-快-慢，抛物速度曲线；等效于 cubic-bezier(0.25, 0.1, 0.25, 1) |
| ease-in               | 慢速开始，等效于：cubic-bezier(0.42, 0, 1, 1)                |
| ease-out              | 慢速结束，等效于：cubic-bezier(0, 0, 0.58, 1)                |
| ease-in-out           | 慢速开始和结束，等效于：cubic-bezier(0.42, 0, 0.58, 1)       |
| cubic-bezier(n,n,n,n) | 自定义，n介于 [0-1]                                          |

### transition-delay：延迟时间

ransition-delay 属性指定何时将开始切换效果。

transition-delay值是指以秒为单位（S）或毫秒（ms）。

**语法**

```css
transition-delay：time；
```

| 值   | 描述                           |
| ---- | ------------------------------ |
| time | 指定等待多久开始执行切换效果。 |

# transform：变形

Transform属性应用于元素的2D或3D转换。这个属性允许你将元素旋转，缩放，移动，倾斜等。

## 兼容性

| 属性            | Chrome                 | Edge              | 火狐               | Safari               | 欧朋                                |
| --------------- | ---------------------- | ----------------- | ------------------ | -------------------- | ----------------------------------- |
| transform（2D） | 36.0<br/>4.0 -webkit-  | 10.0<br/>9.0 -ms- | 16.0<br/>3.5 -moz- | 9.0<br/>3.2 -webkit- | 23.0<br/>15.0 -webkit-<br/>10.5 -o- |
| transform（3D） | 36.0<br/>12.0 -webkit- | 12.0              | 10.0               | 16.0<br/>10.0 -moz-  | 9.0<br/>4.0 -webkit-                |

**语法**

```css
transform: none | transform-functions;
```

| 值                           | 描述                               |
| ---------------------------- | ---------------------------------- |
| none                         | 空， 没有变化效果。                |
| matrix()                     | 2D转换，使用6个值的矩阵。          |
| matrix3d()                   | 3D转换，使用16个值的矩阵。         |
| **translate(x, y)**          | 移动                               |
| **translate3d(x, y, z)**     |                                    |
| **translateX(x)**            |                                    |
| **translateY(y)**            |                                    |
| **translateZ(z)**            |                                    |
| scale(x, [y])                | 缩放                               |
| scale3d(x, y, z)             |                                    |
| scaleX(x)                    |                                    |
| scaleY(y)                    |                                    |
| scaleZ(z)                    |                                    |
| **rotate(angle)**            | 旋转， angle 为角度， 单位 **deg** |
| **rotate3d(x, y, z, angle)** |                                    |
| **rotateX(angle)**           |                                    |
| **rotateY(angle)**           |                                    |
| **rotateZ(angle)**           |                                    |
| skew(x-angle, y-angle)       | 变型                               |
| skewX(angle)                 |                                    |
| akewY(angle)                 |                                    |
| perspective(n)               |                                    |

# transform-origin：变形原点

transform-Origin属性允许您更改转换元素的位置。

2D转换元素可以改变元素的X和Y轴。 3D转换元素，还可以更改元素的Z轴。



**语法**

```css
transform-origin: x-axis y-axis z-axis;
```

| 值     | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| x-axis | 定义原点X坐标， 可能为： left \| center \| right \| length \| % |
| y-axis | 定义原点X坐标， 可能为：left \| center \| right \| length \| % |
| z-axis | 定义原点X坐标， 可能为： length , Z 轴屏幕内为  -   远离屏幕为 + |

# transform-style：变形样式

transform--style属性指定嵌套元素是怎样在三维空间中呈现。

**注意：** 使用此属性必须先使用 [transform](https://www.runoob.com/cssref/css3-pr-transform.html) 属性.

**语法**

```css
transform-style: flat|preserve-3d;
```

| 值          | 描述                           |
| :---------- | :----------------------------- |
| flat        | 表示所有子元素在2D平面呈现。   |
| preserve-3d | 表示所有子元素在3D空间中呈现。 |

# perspective：透视距离

多少像素的3D元素是从视图的perspective属性定义。这个属性允许你改变3D元素是怎样查看透视图。

定义时的perspective属性，它是一个元素的子元素，透视图，而不是元素本身。

**注意：**perspective 属性只影响 3D 转换元素。

**提示:** 请与 [perspective-origin](https://www.runoob.com/cssref/css3-pr-perspective-origin.html) 属性一同使用该属性，这样您就能够改变 3D 元素的底部位置。

**语法**

```css
perspective: number | none;
```

| 值     | 描述                         |
| ------ | ---------------------------- |
| number | 元素距离视图的距离，单位像素 |
| none   | 无透视效果。                 |

# perspective：透视点位置

perspective-origin 属性定义 3D 元素所基于的 X 轴和 Y 轴。该属性允许您改变 3D 元素的底部位置。

定义时的perspective -Origin属性，它是一个元素的子元素，透视图，而不是元素本身。

**注意：** 对于Chrome和Safari用户：为了更好地理解[perspective](https://www.runoob.com/cssref/css3-pr-perspective.html)属性!



**语法**

```css
perspective-origin: *x-axis y-axis*;
```

| 值       | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| *x-axis* | 定义该视图在 x 轴上的位置。默认值：50%。可能的值：leftcenterright*length**%* |
| *y-axis* | 定义该视图在 y 轴上的位置。默认值：50%。可能的值：topcenterbottom*length**%* |

# backface-visivility：背向控制

backface-visibility 属性定义当元素不面向屏幕时是否可见。

如果在旋转元素不希望看到其背面时，该属性很有用。

**语法**

```css
backface-visibility: visible|hidden;
```

| 值      | 描述             |
| :------ | :--------------- |
| visible | 背面是可见的。   |
| hidden  | 背面是不可见的。 |

# --- 动画

# animation：动画

给选定元素绑定动画效果

**语法**

```css
animation: name duration timing-function delay iteration-count direction fill-mode play-state;
```



| 值                                                           | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| *animation-name*                                             | 指定要绑定到选择器的关键帧的名称                             |
| *animation-duration*                                         | 动画指定需要多少秒或毫秒完成                                 |
| *animation-timing-function*                                  | 设置动画将如何完成一个周期                                   |
| *animation-delay*                                            | 设置动画在启动前的延迟间隔。                                 |
| *animation-iteration-count*                                  | 定义动画的播放次数。                                         |
| *animation-direction*                                        | 指定是否应该轮流反向播放动画。                               |
| [animation-fill-mode](https://www.runoob.com/cssref/css3-pr-animation-fill-mode.html) | 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。 |
| *animation-play-state*                                       | 指定动画是否正在运行或已暂停。                               |
| initial                                                      | 设置属性为其默认值。 [阅读关于 *initial*的介绍。](https://www.runoob.com/cssref/css-initial.html) |
| inherit                                                      | 从父元素继承属性。 [阅读关于 *initinherital*的介绍。](https://www.runoob.com/cssref/css-inherit.html) |

# @keyframes：动画声明

使用@keyframes规则，你可以创建动画。

创建动画是通过逐步改变从一个CSS样式设定到另一个。

在动画过程中，您可以更改CSS样式的设定多次。

指定的变化时发生时使用％，或关键字"from"和"to"，这是和0％到100％相同。

0％是开头动画，100％是当动画完成。

为了获得最佳的浏览器支持，您应该始终定义为0％和100％的选择器。

**注意:** 使用animation属性来控制动画的外观，使用选择器绑定动画。

**语法**

```css
@keyframes animationname {
    keyframes-selector {
        css-styles;
    }
}

/* 例如：*/
@keyframs shwoInLeft {
    0% {
        stytle;
    }
    100% {
        style;
    }
}
```



| 值                   | 说明                                                         |
| :------------------- | :----------------------------------------------------------- |
| *animationname*      | 必需的。定义animation的名称。                                |
| *keyframes-selector* | 必需的。动画持续时间的百分比。合法值：0-100% from (和0%相同) to (和100%相同)**注意：** 您可以用一个动画keyframes-selectors。 |
| *css-styles*         | 必需的。一个或多个合法的CSS样式属性                          |



# 弹性容器布局

采用Flex布局的元素，称为Flex容器（flex container），简称”容器”。

它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称”项目”。



容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。

主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；

交叉轴的开始位置叫做cross start，结束位置叫做cross end。

项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

![](CSS动画.assets/8017344-6b31409ba46fba6c.png)

