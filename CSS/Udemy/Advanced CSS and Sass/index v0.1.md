# 工程化模式

## `HTML` 与 `CSS` 的基本编写原则

1. 响应式设计。

   - 虽然媒体查询很好用，但不能过分依赖。

     尽可能的将需要修改的部分控制到最小。

   - 使用正确的单位，比如 `rem`。

2. 可维护、可扩展的代码。

3. 关注性能。

## `Sass`

### 7-1 分区方式

定义：七个文件夹和一个主文件。

1. `base`：初始化设置、字体的基本配置。

2. `components`：组件，可以应用于各种地方。

3. `layout`：布局，针对版面做切分。

4. `page`：放一些复用性不高的，只针对于单个页面的样式。

5. `themes`：主题。

6. `abstracts`：

7. `vendors`：

8. `main.scss`：唯一一个不以下划线开头的 scss 文件。

   只应当包含 `@import` 和注释。

## `npm`

## ` BEM`

Block Element Modifier：所有类名以 Block__Element--Modifier 的形式组织。

- Block：拥有具体意义的块。
- Element：独立的话无意义。
- Modifier：相同元素或块的不同版本

# 代码编写

## 初始化 `CSS`

1. 简单的通过 `*` 实现。

   注意伪元素不能通过通配符匹配，需使用 `*::after, *::before`。

2. 浏览器默认设置 `font-size: 16px`。

   为了方便全局使用 `rem` 作为单位，可在 `html` 选择器中设定 `font-size: 62.5%`。

   等于设置 `font-size: 10px`，虽然等价，但是一般认为是更好的实践。

3. 在 body 中设置字体相关属性。通过继承实现默认设置。

   同时也可以设置 `box-sizing: border-box`。
   然后在通配符设置中，通过 `inherit` 属性实现强制继承。
   虽然基本也等价，但是也通常被认为是更好的实践。

## `CSS` 常用属性

1. `clip-path`：对元素进行裁剪。

2. `transform`：对元素进行二次定位。

3. `backface-visibility`：如果动画中出现抖动，可以试试关闭这个属性（设置为 `hidden`）。

   原意是设置元素背后的可见度。

4. 动画：

   - 通过 `transition` 实现：控制元素在不同状态时的切换效果。
   - 通过 `animation` 实现：配合 `@keyframes` 实现对整个动画的控制。
     - animation-fill-mode：设置 `CSS` 动画在执行之前和之后如何将样式应用于其目标。

## 代码组织

1. 控制 `inline` 元素就和控制普通文本一样，相关的控制手段都可以用。
2. `span` 元素可以很好的用来分割文本。
3. 

# 底层知识

## 加载页面的基本过程

1. 加载 `HTML` 文件。
2. 解析 `HTML` 文件，生成 `DOM (Document Object Model)` 树。
   1. 如果发现加载的 `CSS` 文件，则先进行加载。
   2. 进行层叠，计算出最终值等一系列操作，生成 `CSSOM (CSS Object Model)` 树。
3. 组合 `DOM` 和 `CSSOM`，形成渲染树（`render tree`）。
4. 为了进行最终的渲染，通过渲染树形成可视化格式模型（`visual formatting model`）。
5. 最终渲染出界面。

- 可视化格式模型用于处理各类盒子。

## `CSS` 权重

- 以下权重由重到轻：
  1. 来源：
     - User `!important` declarations
     - Author `!important` declarations
     - Author declarations
     - User declarations
     - Default browser declarations
  2. 特异性：
     - `inline style`
     - `IDs`
     - `Classes`、`pseudo-classes`、`attribute` 
     - `Elements`、`pseudo-elements`
  3. 代码先后顺序。
- 通配符没有优先级。

## `CSS` 值处理过程

- 最终，`HTML` 中的每一个元素都有全部的 `CSS` 值。

- 处理过程：

  1. Declared Value：定义值：

     可能在多个地方都有定义一个元素的某个 `CSS` 属性值。

  2. Cascaded Value：层叠值：

     定义值经过层叠后获得的值。

     注意，这一层可能会有浏览器默认设定的值。

  3. Specified Value：默认的值：

     与浏览器默认设定的值不同，是 `CSS` 的默认值。

     如果当前属性可以通过继承获得，那就采用继承的值。

  4. Computed Value：计算值：

     将有单位的值换算成具体的数值（百分比不算单位）。

  5. Used Value：最终计算出来的值：

  6. Actual Value：最终使用的值：
  
- `calc` 为什么强大，因为他可以单位混算。

  为什么可以单位混算呢？因为必须等到可视化模型计算出来后，统一转换成像素才能进行计算。因此 SASS 不行。

## 单位转换过程

- `% (fonts)`：相对于父组件的 `font-size`。
- `% lengths`：相对于父组件计算值的 `width`。
- `em (fonts)`：相对于父组件的 `font-size`。
- `em (length)`：相对于当前组件的 `font-size`。
- `rem`：相对于根元素的 `font-size`。
- `vh`：相对于视口的长度。
- `vw`：相对于视口的宽度。

## 盒子模型

1. `box-sizing`：通过此属性，定义 `border-box` 或是 `content-box`。

   两种模式下，盒子的 `padding`, `border` 所占据的空间不同。

2. `box-type`：盒子的不同种类：

   - `display: block`
   - `display: inline-block`
   - `display: inline`

   盒子的表现形式不同。

3. 定位方式：

   - 正常文档流：
   - 浮动元素：被移除正常文档流。
   - 绝对定位：被移除正常文档流。相对父元素进行二次定位。

4. 层叠上下文：

   - 通过不同方式，创建层叠上下文确定元素的重叠情况。



# UnSorted Notes

1. `<figure>`：在语义上更好一点。
2. `<p>`：作为 paragraph
3. shape-outside：设定形状的属性。用这个属性只能使用浮动，还必须定义宽高？
4. video 配合 source 使用。可以加一些属性来控制。
5. object-cover：对于子元素使用，有点像 background-cover
6. input 相关伪元素：input-placeholder、focus、invalid、checked。
7. input 元素的字体相关属性不是继承的，需要显示声明。
8. HTML：单选要求 name 属性，没有办法给单选框加样式
9. column: count
10. column-rule