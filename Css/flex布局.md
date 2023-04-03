# flex布局

## 概述

flexible box 弹性布局，通过设置父盒子为flex容器来控制每一个子元素的布局。任何一个容器都可以指定为flex布局。所有子元素为容器成员。

## 布局原理

- 父元素设置为flex布局后，子元素的float、clear、vertical-align属性将失效。

## 常见父盒子属性

### ``flex-direction``

设置主轴方向

| key            | value        |
| -------------- | ------------ |
| row            | 横向从左到右 |
| row-reserve    | 横向从右到左 |
| column         | 纵向从上到下 |
| column-reserve | 纵向从下到上 |

### ``flex-wrap``

设置是否换行。当元素没有设置min-width的时候，如果不换行，元素宽度会被压缩。如果设置了则会溢出。

| key    | value  |
| ------ | ------ |
| wrap   | 换行   |
| nowrap | 不换行 |

### ``justify-content``

设置主轴上的子元素排列方式

| key           | value                    |
| ------------- | ------------------------ |
| flex-start    | 默认值，靠主轴前端       |
| flex-end      | 靠主轴末端               |
| center        | 居中                     |
| space-around  | 平分剩余空间             |
| space-between | 两边贴边，再平分剩余空间 |

### ``align-content``

设置侧轴上的子元素排列方式(多行)

| key           | value                    |
| ------------- | ------------------------ |
| flex-start    | 默认值，靠主轴前端       |
| flex-end      | 靠主轴末端               |
| center        | 居中                     |
| space-around  | 平分剩余空间             |
| space-between | 两边贴边，再平分剩余空间 |
| stretch       | 平分父元素高度           |

### ``align-items``

设置侧轴上的子元素排列方式(单行)

| key        | value            |
| ---------- | ---------------- |
| flex-start | 默认值，从上到下 |
| flex-end   | 从下到上         |
| center     | 居中             |
| stretch    | 拉伸             |

### ``flex-flow``

复合属性，相当于同时设置flex-direction和flex-wrap

## 常见子项属性

### ``flex``

子项目分配剩余空间占的份数

### ``align-self``

子项目自己在侧轴的排列方式，参照align-items

### ``order``

子项的排列顺序