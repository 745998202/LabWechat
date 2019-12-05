---
description: 小程序的Flex布局详解
---

# 布局

Flex 布局

如果小程序要求兼容到iOS8以下版本，需要开启样式自动补全。开启样式自动补全，在“设置”—“项目设置”—勾选“上传代码时样式自动补全”

![&#x4E0A;&#x4F20;&#x4EE3;&#x7801;&#x6837;&#x5F0F;&#x81EA;&#x52A8;&#x8865;&#x5168;](.gitbook/assets/image%20%2819%29.png)

### 基本概念

flex的概念最早在2009年被提，目的是提供一种更加灵活的布局模型，使容器能够通过改变里面项目的高宽，顺序，来对可用空间实现最佳的填充，方便适配不同大小的内容区域。

在不固定高度信息的例子中，只需要在容器中设置以下两个属性即可实现内容不确定下的垂直居中

```text
.container{
display: flex;
flex-direction: column;
justify-content: center;
}
```

flex不单是一个属性，他包含了一套属性集。属性集包括用于设置容器，和用于设置项目两部分

设置容器的属性有

```text
display : flex
flex-direction :row(默认值) | row-reverse | column | column-reverse
flex-wrap:nowrap(默认值) | wrap | wrap-reverse
justify-content: flex-start(默认值) | flex-end | center | space-between | space-around | space-evenly
align-items:stretch(默认值) | center | flex-end | baseline | flex-start
align-content: stretch(默认值) | flex-start | center | flex-end | space-between | space-around | space-enevly
```

设置项目的属性有：

```text
order:0 (define) | <integer>
flex-shrink:1 (define) | <number>
flex-grow: 0 | <number>
flex-basis: auto | length
flex: none | auto | @flex-grow @flex-shrink @flex-basis
align-self: auto | flex-start | flex-end | center | baseline | stretch
```

在开始介绍各个属性之前，我们要先明确一个坐标轴。默认的情况下，水平方向是主轴，垂直方向是交叉轴

![&#x5750;&#x6807;&#x8F74;](.gitbook/assets/image%20%2817%29.png)

项目是在主轴上排列，排满后在交叉轴方向换行。需要注意的是，交叉轴垂直于主轴，它的方向取决于主轴方向

![&#x4EA4;&#x53C9;&#x8F74;&#x5782;&#x76F4;&#x4E8E;&#x4E3B;&#x8F74;](.gitbook/assets/image%20%2813%29.png)

接下来的例子如无特殊声明，我们都以默认情况下的坐标轴为例

#### 属性容器

设置容器，用于统一管理容器内项目布局，也就是管理项目的排列方式和对齐方式

### flex-direction 属性

> 通过设置坐标轴，来设置项目的排列方向

| 值 | 含义 |
| :--- | :--- |
| row | 主轴横向，方向为从左指向右。项目沿主轴排列，从左到右排列 |
| row-reverse | row的反方向，方向为从右指向左。项目沿主轴排列，从右到左排列 |
| column | 主轴纵向，方向从上指到下。项目沿主轴排列，从上到下排列 |
| column-reverse | column的反方向。主轴纵向，方向从下指向上。项目沿主轴排列，从下向上排列 |

![](.gitbook/assets/image.png)

### flex-wrap 属性

> 设置是否允许项目多行排列，以及多行排列时换行的方向

```text
.contain{
flex-wrap : nowrap | wrap | wrap-reverse
}
```

| 值 | 含义 |
| :--- | :--- |
| nowrap | 不换行，如果单行内容过多则溢出容器 |
| wrap | 容器单行容不下所有项目时，换行排列 |
| wrap-reverse | 容器单行容不下所有项目时，换行排列。换行方向为wrap时的反方向 |

![flex-wrap](.gitbook/assets/image%20%2814%29.png)

### justify-content 属性

> 设置项目在主轴方向上对齐方式，以及分配项目之间及其周围多余的空间

```text
.container {
justify-content : flex-start(默认值) | flex-end | center | space-between | space-around | space-evenly
}

```

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x503C;</th>
      <th style="text-align:left">&#x542B;&#x4E49;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">flex-start</td>
      <td style="text-align:left">&#x9879;&#x76EE;&#x5BF9;&#x9F50;&#x4E3B;&#x8F74;&#x8D77;&#x70B9;&#xFF0C;&#x9879;&#x76EE;&#x4E0D;&#x7559;&#x7A7A;&#x9699;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">center</td>
      <td style="text-align:left">
        <p>&#x9879;&#x76EE;&#x5728;&#x4E3B;&#x8F74;&#x4E0A;&#x5C45;&#x4E2D;&#x6392;&#x5217;&#xFF0C;&#x9879;&#x76EE;&#x4E0D;&#x7559;&#x7A7A;&#x9699;&#x3002;&#x4E3B;&#x8F74;&#x4E0A;&#x7B2C;&#x4E00;&#x4E2A;&#x9879;&#x76EE;&#x79BB;&#x4E3B;&#x8F74;&#x8D77;&#x70B9;&#x8DDD;&#x79BB;&#x7B49;&#x4E8E;&#x6700;&#x540E;</p>
        <p>&#x4E00;&#x4E2A;&#x9879;&#x76EE;&#x79BB;&#x4E3B;&#x8F74;&#x7684;&#x7EC8;&#x70B9;&#x8DDD;&#x79BB;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">flex-end</td>
      <td style="text-align:left">&#x9879;&#x76EE;&#x5BF9;&#x9F50;&#x4E3B;&#x8F74;&#x7EC8;&#x70B9;&#xFF0C;&#x9879;&#x76EE;&#x95F4;&#x4E0D;&#x7559;&#x7A7A;&#x9699;</td>
    </tr>
    <tr>
      <td style="text-align:left">space-between</td>
      <td style="text-align:left">&#x9879;&#x76EE;&#x95F4;&#x95F4;&#x8DDD;&#x76F8;&#x7B49;&#xFF0C;&#x7B2C;&#x4E00;&#x4E2A;&#x9879;&#x76EE;&#x79BB;&#x4E3B;&#x8F74;&#x8D77;&#x70B9;&#x548C;&#x6700;&#x540E;&#x4E00;&#x4E2A;&#x9879;&#x76EE;&#x79BB;&#x4E3B;&#x8F74;&#x7EC8;&#x70B9;&#x8DDD;&#x79BB;&#x4E3A;0</td>
    </tr>
    <tr>
      <td style="text-align:left">space-around</td>
      <td style="text-align:left">
        <p>&#x4E0E;space-between&#x76F8;&#x4F3C;&#x3002;&#x4E0D;&#x540C;&#x7684;&#x70B9;&#x4E3A;&#xFF0C;&#x7B2C;&#x4E00;&#x4E2A;&#x9879;&#x76EE;&#x79BB;&#x4E3B;&#x8F74;&#x8D77;&#x70B9;&#x548C;&#x6700;&#x540E;&#x4E00;&#x4E2A;&#x9879;&#x76EE;&#x8DDD;&#x79BB;&#x4E3B;&#x8F74;</p>
        <p>&#x7EC8;&#x70B9;&#x7684;&#x8DDD;&#x79BB;&#x4E3A;&#x4E2D;&#x95F4;&#x9879;&#x76EE;&#x95F4;&#x8DDD;&#x7684;&#x4E00;&#x534A;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">space-evenly</td>
      <td style="text-align:left">&#x9879;&#x76EE;&#x95F4;&#x95F4;&#x8DDD;&#x3001;&#x7B2C;&#x4E00;&#x4E2A;&#x9879;&#x76EE;&#x79BB;&#x4E3B;&#x8F74;&#x8D77;&#x70B9;&#x548C;&#x6700;&#x540E;&#x4E00;&#x4E2A;&#x9879;&#x76EE;&#x79BB;&#x4E3B;&#x8F74;&#x7EC8;&#x70B9;&#x8DDD;&#x79BB;&#x7B49;&#x4E8E;&#x9879;&#x76EE;&#x95F4;&#x95F4;&#x8DDD;</td>
    </tr>
  </tbody>
</table>![justify-content](.gitbook/assets/image%20%2818%29.png)

### align-items 属性

> 设置项目在行中的对齐方式

```text
.container{

align-items : stretch(默认值) | flex-start | center | flex-end | baseline

}
```

| 值 | 含义 |
| :--- | :--- |
| strech | 项目拉伸默认填满行高 |
| flex-start | 项目顶部与行起点对齐 |
| center | 项目在行中居中对齐 |
| flex-end | 项目底部与行终点对齐 |
| baseline | 项目的第一行文字的基线对齐 |

![](.gitbook/assets/image%20%2820%29.png)

![](.gitbook/assets/image%20%2816%29.png)

### align-content 属性

多行排列时,设置行在交叉轴方向上的对齐方式，以及分配行之间及其周围多余的空间

```text
.container {
 
 align-content: strech(默认值) | flex-start | center | flex-end | space-between | space-evenly
 
}
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x503C;</th>
      <th style="text-align:left">&#x542B;&#x4E49;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">strech</td>
      <td style="text-align:left">
        <p>&#x5F53;&#x672A;&#x8BBE;&#x7F6E;&#x9879;&#x76EE;&#x5C3A;&#x5BF8;&#xFF0C;&#x5C06;&#x5404;&#x884C;&#x4E2D;&#x7684;&#x9879;&#x76EE;&#x62C9;&#x4F38;&#x81F3;&#x586B;&#x6EE1;&#x4EA4;&#x53C9;&#x8F74;&#x3002;&#x5F53;&#x8BBE;&#x7F6E;&#x4E86;&#x9879;&#x76EE;&#x5C3A;&#x5BF8;&#xFF0C;</p>
        <p>&#x9879;&#x76EE;&#x5C3A;&#x5BF8;&#x4E0D;&#x53D8;&#xFF0C;&#x9879;&#x76EE;&#x884C;&#x62C9;&#x4F38;&#x81F3;&#x586B;&#x6EE1;&#x4EA4;&#x53C9;&#x8F74;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">flex-start</td>
      <td style="text-align:left">&#x9996;&#x884C;&#x5728;&#x4EA4;&#x53C9;&#x8F74;&#x8D77;&#x70B9;&#x5F00;&#x59CB;&#x6392;&#x5217;&#xFF0C;&#x884C;&#x95F4;&#x4E0D;&#x7559;&#x95F4;&#x8DDD;</td>
    </tr>
    <tr>
      <td style="text-align:left">center</td>
      <td style="text-align:left">&#x884C;&#x5728;&#x4EA4;&#x53C9;&#x8F74;&#x4E2D;&#x70B9;&#x6392;&#x5217;&#xFF0C;&#x884C;&#x95F4;&#x4E0D;&#x7559;&#x95F4;&#x8DDD;&#xFF0C;&#x9996;&#x884C;&#x79BB;&#x4EA4;&#x53C9;&#x8F74;&#x8D77;&#x70B9;&#x548C;&#x5C3E;&#x884C;&#x79BB;&#x4EA4;&#x53C9;&#x8F74;&#x7EC8;&#x70B9;&#x8DDD;&#x79BB;&#x76F8;&#x7B49;</td>
    </tr>
    <tr>
      <td style="text-align:left">flex-end</td>
      <td style="text-align:left">&#x5C3E;&#x884C;&#x5728;&#x4EA4;&#x53C9;&#x8F74;&#x7EC8;&#x70B9;&#x5F00;&#x59CB;&#x6392;&#x5217;&#xFF0C;&#x884C;&#x95F4;&#x4E0D;&#x7559;&#x95F4;&#x8DDD;</td>
    </tr>
    <tr>
      <td style="text-align:left">space-between</td>
      <td style="text-align:left">&#x884C;&#x4E0E;&#x884C;&#x95F4;&#x8DDD;&#x76F8;&#x7B49;&#xFF0C;&#x9996;&#x884C;&#x79BB;&#x4EA4;&#x53C9;&#x8F74;&#x8D77;&#x70B9;&#x548C;&#x5C3E;&#x884C;&#x79BB;&#x4EA4;&#x53C9;&#x8F74;&#x7EC8;&#x70B9;&#x8DDD;&#x79BB;&#x4E3A;0</td>
    </tr>
    <tr>
      <td style="text-align:left">space-around</td>
      <td style="text-align:left">&#x884C;&#x4E0E;&#x884C;&#x95F4;&#x8DDD;&#x76F8;&#x7B49;&#xFF0C;&#x9996;&#x884C;&#x79BB;&#x4EA4;&#x53C9;&#x8F74;&#x8D77;&#x70B9;&#x548C;&#x5C3E;&#x884C;&#x79BB;&#x4EA4;&#x53C9;&#x8F74;&#x7EC8;&#x70B9;&#x8DDD;&#x79BB;&#x4E3A;&#x884C;&#x4E0E;&#x884C;&#x95F4;&#x8DDD;&#x7684;&#x4E00;&#x534A;</td>
    </tr>
    <tr>
      <td style="text-align:left">space-evenly</td>
      <td style="text-align:left">&#x884C;&#x95F4;&#x95F4;&#x8DDD;&#xFF0C;&#x4EE5;&#x53CA;&#x9996;&#x884C;&#x79BB;&#x4EA4;&#x53C9;&#x8F74;&#x8D77;&#x70B9;&#x548C;&#x5C3E;&#x884C;&#x79BB;&#x4EA4;&#x53C9;&#x8F74;&#x7EC8;&#x70B9;&#x8DDD;&#x79BB;&#x76F8;&#x7B49;</td>
    </tr>
  </tbody>
</table>![](.gitbook/assets/image%20%285%29.png)

![](.gitbook/assets/image%20%284%29.png)

![align-content](.gitbook/assets/image%20%283%29.png)

### 项目属性

设置项目，用于设置项目的尺寸、位置，以及对项目的对齐方式做特殊设置

### order属性

设置项目沿主轴方向上的排列顺序，数值越小，排列越靠前，属性值为整数

```text
.item {
order: 0 | <integer>
}
```

![order&#x5C5E;&#x6027;](.gitbook/assets/image%20%2810%29.png)

### flex-shrink属性

当项目在主轴方向上溢出时，通过设置项目收缩因子来压缩项目适应容器。属性值为项目的收缩因子，属性值取非负值

```text
.item{
    flex-shrink: 1 | <number>
}

.item1{

  width: 120px;

  flex-shrink: 2;

}

.item2{

  width: 150px;

  flex-shrink: 3;

}

.item3{// 项目3未设置flex-shrink，默认flex-shrink值为1

  width: 180px;

}
```

为了加深理解，举个例子：

一个宽度为400px的容器，里面三个项目width分别为120px,150px,180px.分别对应这项目1和项目2设置flex-shrink值为2和3

```text
.container{
display : flex;
width : 400px;//容器宽度为400px
}
```

在这个例子中，项目溢出400 - \(120 + 150 + 180\) = -50px。计算压缩总量时为各个项目的宽度乘以flex-shrink的总和，这个例子压缩总权重为120\*2+150\*3+180\*1 = 870。各个项目压缩空间大小为总溢出空间乘以flex-shrink除以总权重：

item1的最终宽度为：120-50\*120\*2/870 ≈ 106px

item2的最终宽度为：150-50\*150\*3/870 ≈ 124px

item3的最终宽度为：180-50\*180\*1/870 ≈ 169px

_**其中计算时候如果为小数，则向下取整**_

![flex-shrink](.gitbook/assets/image%20%289%29.png)

{% hint style="info" %}
需要注意的是，当项目中的压缩因子相加小于1时，参与计算的溢出空间不等于完整的溢出空间。在上面的例子的基础上，我们改变各个项目的flex-shrink
{% endhint %}

```text
.container{

  display: flex;

  width: 400px; // 容器宽度为400px

}

.item1{

  width: 120px;

  flex-shrink: 0.1;

}

.item2{

  width: 150px;

  flex-shrink: 0.2;

}

.item3{

  width: 180px;

  flex-shrink: 0.3;

}
```

总权重为：120\*0.1+150\*0.2+180\*0.3 = 96。参与计算的溢出空间不再是50px,而是50\*\(0.1+0.2+0.3\)/1 = 30:

item1的最终宽度为：120-30\*120\*0.1/96 ≈ 116px

item2的最终宽度为：150-30\*150\*0.2/96 ≈ 140px

item3的最终宽度为：180-30\*180\*0.3/96 ≈ 163px

### flex-grow属性

> 当项目在主轴空间上还有剩余空间时，通过设置项目扩张因子进行剩余空间的分配，属性值为扩张因子，属性值取非负值

```text
.item{

    flex-grow : 0 | <number>

}
```

举个栗子

一个宽度为400px的容器，里面的三个项目分别为80,120,140px，分别对项目1和项目2设置flex-grow属性值为3和1

```text
.container{
display : flex;

width: 400px;//容器宽度为400px

}

.item1 {
width: 120px;
flex-grow: 1;

}

.item2 {
width : 120px;
flex-grow: 1;
}

.item3{ //项目3未设置flex-grow,默认flex-grow值为0

width:140px;

}
```

在这个例子中，容器的剩余空间为400 - \(80+120+140\) = 60px。剩余空间按照 60/\(3+1+0\) = 15px进行分配

item1的最终宽度为：80+（15\*3）= 125px

item2的最终宽度为：120+\(15\*1\) = 135px

item3的最终宽度为：140+\(15\*0\) = 140px

![flex-grow](.gitbook/assets/image%20%286%29.png)

需要注意一点，当项目扩张因子相加小于1时，剩余空间按除以1进行分配。在上面例子的基础上，我们改变各个项目flex-grow

```text
.container{

  display: flex;

  width: 400px; // 容器宽度为400px

}

.item1{

  width: 50px;

  flex-grow: 0.1;

}

.item2{

  width: 80px;

  flex-grow: 0.3;

}

.item3{

  width: 110px;

  flex-grow: 0.2;

}
```

在这个例子中，容器的剩余空间为400-（20+80+110）=160px。由于项目的flex-grow相加0.1+0.3+0.2 = 0.6小于1，剩余空间按照160/1=160px划分。例子中的项目宽度分别为：

item1的最终宽度为：50+\(160\*0.1\) = 66px

item2的最终宽度为: 80 + \(160\*0.3\) = 128px

item3的最终宽度为: 110 + \(160\*0.2\) = 142px

### flex-basis属性

当容器设置flex-direction为row或row-reverse时，flex-basis和width同时存在，flex-basis优先级高于width，也就是此时flex-basis代替项目的width属性。

当容器设置flex-direction为column或column-reverse时，flex-basis和height同时存在，flex-basis优先级高于height，也就是此时flex-basis代替项目的height属性。

需要注意的是，当flex-basis和width（或height），其中一个属性值为auto时，非auto的优先级更高。

```text
.item{

  flex-basis: auto（默认值） | <number>px

}
```

![flex-basis](.gitbook/assets/image%20%282%29.png)

### flex 属性

> flex-grow flex-shrink flex-basis的简写方式。值设置为none，等价于00 auto。值设置为auto，等价于11 auto

```text
.item {

flex : none | auto @flex-grow@flex-shrink@flex-basis

}
```

### align-self属性

设置项目行中交叉轴方向上的对齐方式，用于覆盖容器的align-items,这么做可以对项目的对齐方式做特殊处理。默认属性值为auto,继承容器的align-items值，当容器没有设置align-items时，属性值为stretch。

```text
.item{

  align-self: auto（默认值） | flex-start | center | flex-end | baseline |stretch

}
```

![](.gitbook/assets/image%20%287%29.png)

