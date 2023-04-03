# grid布局

## 概述

它将网页划分成一个个网格，可以任意组合不同的网格，做出各种各样的布局。Grid 布局是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是**二维布局**。Grid 布局远比 Flex 布局强大。

## 常用父级属性

``display``

| key         | value                      |
| ----------- | -------------------------- |
| grid        | 设置grid布局，容器为块     |
| inline-grid | 设置grid布局，容器为行内块 |

### ``grid-template-rows``

划分行数

```css
grid-template-rows: 50px 20% 50px 50px;
/* 网格线名称 */
grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
```

### ``grid-template-colums``

划分列数

```css
grid-template-columns: 50px 50px 50px 50px;
```

### ``repeat()``

grid-template-columns grid-template-rows和缩写

```css
grid-template-columns: repeat(3, 33.33%);
grid-template-rows: repeat(3, 33.33%);

/* 或者重复某一种模式 */
grid-template-columns: repeat(2, 100px 20px 80px);
```

### ``auo-fill``

如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用`auto-fill`关键字表示自动填充

```css
/* 以 100px进行尽可能多的扩列 */
grid-template-columns: repeat(auto-fill, 100px);
```

### ``auto``

占剩余宽度。

### ``fr关键字``

如果两列的宽度分别为`1fr`和`2fr`，就表示后者是前者的两倍。如果全是fr则等比划分。

```css
grid-template-columns: 150px 1fr 2fr;
```

### ``minmax()``

`minmax()`函数产生一个长度范围，表示长度就在这个范围之中。它接受两个参数，分别为最小值和最大值。

```css
grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```

### ``grid-row-gap``

行间隔

### ``grid-column-gap``

列间距

### ``grid-gap``

`grid-gap`属性是`grid-column-gap`和`grid-row-gap`的合并简写形式

### ``grid-auto-flow``

排列顺序

| key          | value                              |
| ------------ | ---------------------------------- |
| row          | 横向排版                           |
| column       | 纵向排版                           |
| row dense    | 表示"先行后列"，并且尽量填满空格。 |
| column dense | 表示"先列后行"，并且尽量填满空格。 |



### ``grid-template-areas``

定义区域，如果某些区域不需要利用，则使用"点"（`.`）表示。

### ``justify-items``

单元格内容的水平方向对齐方式

| key     | value                                  |
| ------- | -------------------------------------- |
| start   | 对齐单元格的起始边缘。                 |
| end     | 对齐单元格的结束边缘。                 |
| center  | 单元格内部居中。                       |
| stretch | 拉伸，占满单元格的整个宽度（默认值）。 |

### ``align-items``

单元格内容的垂直方向对齐方式

| key     | value                                  |
| ------- | -------------------------------------- |
| start   | 对齐单元格的起始边缘。                 |
| end     | 对齐单元格的结束边缘。                 |
| center  | 单元格内部居中。                       |
| stretch | 拉伸，占满单元格的整个宽度（默认值）。 |

### ``justify-content``

整个内容区域在容器里面的水平位置

| key           | value                                                        |
| ------------- | ------------------------------------------------------------ |
| start         | 对齐容器的起始边框。                                         |
| end           | 对齐容器的结束边框                                           |
| center        | 容器内部居中                                                 |
| stretch       | 项目大小没有指定时，拉伸占据整个网格容器                     |
| space-around  | 每个项目两侧的间隔相等，项目之间的间隔比项目与容器边框的间隔大一倍。 |
| space-between | 项目与项目的间隔相等，项目与容器边框之间没有间隔。           |
| space-evenly  | 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。 |

### ``align-content``

整个内容区域在容器里面的垂直位置

| key           | value                                                        |
| ------------- | ------------------------------------------------------------ |
| start         | 对齐容器的起始边框。                                         |
| end           | 对齐容器的结束边框                                           |
| center        | 容器内部居中                                                 |
| stretch       | 项目大小没有指定时，拉伸占据整个网格容器                     |
| space-around  | 每个项目两侧的间隔相等，项目之间的间隔比项目与容器边框的间隔大一倍。 |
| space-between | 项目与项目的间隔相等，项目与容器边框之间没有间隔。           |
| space-evenly  | 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。 |

### ``grid-auto-rows``

浏览器自动创建的多余网格的列宽和行高。

### ``grid-auto-columns``

浏览器自动创建的多余网格的列宽和行高。

## 常用子级属性

### ``grid-row-start``&&``grid-row-end``

左边框所在的垂直网格线，右边框所在的垂直网格线

### ``grid-column-start``&&``grid-column-end``

上边框所在的水平网格线，下边框所在的水平网格线

### ``justify-self``

属性设置单元格内容的水平位置（左中右），跟`justify-items`属性的用法完全一致，但只作用于单个项目。

### ``align-self``

属性设置单元格内容的垂直位置（上中下），跟`align-items`属性的用法完全一致，也是只作用于单个项目。

### 