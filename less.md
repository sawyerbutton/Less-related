# Less基础教程

## 变量
```less
@variable-name: value;
```
比如:
```less
@mainColour: #309296;
body { color: @mainColour; }
blockquote { border: 2px solid @mainColour; }
```
转义成css为:
```css
body { color: #309296; }
blockquote { border: 2px solid #309296; }
```

## mixins
> 除了将值与变量重用之外，您还可以在整个样式表中重用整个CSS块。这些被称为mixins
```less
.roundedBox {
  border: 1px solid #309296;
  background-color: #eee;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
  padding: 10px;
}
 
blockquote {
  font-size: 1.5em;
  .roundedBox;
}
 
.promo {
  width: 10em;
  .roundedBox;
}
```
转义成css为:
```css
.roundedBox {
  border: 2px solid #309296;
  background-color: #eee;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
  padding: 10px;
}
 
blockquote {
  font-size: 1.5em;
  border: 2px solid #309296;
  background-color: #eee;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
  padding: 10px;
}
 
.promo {
  width: 10em;
  border: 2px solid #309296;
  background-color: #eee;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
  padding: 10px;
}
```
> 你也可以向mixins中添加参数使他们看起来更像是方法
```less
.roundedBox( @borderRadius, @borderColour ) {
  border: 2px solid @borderColour;
  background-color: #eee;
  -moz-border-radius: @borderRadius;
  -webkit-border-radius: @borderRadius;
  border-radius: @borderRadius;
  padding: 10px;
}
 
blockquote {
  font-size: 1.5em;
  .roundedBox( 5px, #6c6 );
}
 
.promo {
  width: 10em;
  .roundedBox( 10px, #fa6 );
}
```

> 还可以为mixin参数指定默认值就像是为函数添加默认值一样有趣
```less
.roundedBox( @borderRadius: 5px, @borderColour: #6c6 ) {
  /* declarations here */
} 
```
>  如果你不希望mixins作为一项规则输出到你的css文件中，可以通过在mixins后面添加括号来实现
```less
#circle(){
  background-color: #4CAF50;
  border-radius: 100%;
}

#small-circle{
  width: 50px;
  height: 50px;
  #circle
}

#big-circle{
  width: 100px;
  height: 100px;
  #circle
}

```
> 编译后的css将不会有mixins规则
```css
#small-circle {
  width: 50px;
  height: 50px;
  background-color: #4CAF50;
  border-radius: 100%;
}
#big-circle {
  width: 100px;
  height: 100px;
  background-color: #4CAF50;
  border-radius: 100%;
}
```

## 更加紧密的嵌套规则
> 举例来说
```css
ul#nav {
  list-style: none;
}
 
ul#nav li {
  display: inline;
  margin: 0; 
  padding: 0;
}
 
ul#nav li a {
  color: #aaa;
  font-size: 1em;
}   
 
ul#nav li a:hover {
  color: #000;
}
```
> 上面这段css代码显得很冗余并且逻辑并不清晰

> 通过less改写这段代码后整个代码结构整洁很多
```less
ul#nav {
  list-style: none;
 
  li {
    display: inline;
    margin: 0;
    padding: 0;
 
    a {
      color: #aaa;
      font-size: 1em;
    //   注意这个&帮助你纠正正确的css方案
      &:hover { color: #000; }
    }
  }
}
```
> 如果使用伪类的同时不使用&会导致编译后的文件显示
```css
a {
  color: #aaa;
  font-size: 1em;
}
 
a :hover { color: #000; }
```

## 使用操作符处理计算
> 数字运算符
```
+ - * / 
```
> lighten(@colour, x%) 返回亮度提高x%的颜色

> darken(@colour, x%) 返回暗度提高x%的颜色

> saturate(@colour, x%)	返回饱和度提高x%的颜色

> desaturate(@colour, x%)	返回饱和度减少x%的颜色

> fadein(@colour, x%)	返回提高x%不透明的颜色

> fadeout(@colour, x%)	返回提高x%透明的颜色

> spin(@colour, x)	返回色调在色轮上移动了x度的颜色

> hue(@colour)	返回颜色的色调

> saturation(@colour)	返回颜色值的饱和度

> lightness(@colour)	返回颜色的亮度

> 也不仅限于应用函数，但我们可以一次定义两个函数

```less
.box.desaturate {
  background-color: desaturate(spin(@color, 123), 50%);
}
```

> 与此同时你也可以使用mix函数混合颜色，此功能将输出由两种颜色混合产生的颜色，就像是绿色在大自然由蓝色和黄色构成

```less
.box.mix {
  background-color: mix(@blue, @yellow, 50%);
}
```
- 其中前两个参数为被选中要混合的颜色，而最后一个参数是每个颜色的比重，0%将会显示第一个参数的颜色而最后一个参数将会显示后者参数的颜色

- 算术运算符举例
```less
@h1Size: 5em;
 
h1 { font-size: @h1Size; }
h2 { font-size: @h1Size * .8; }
h3 { font-size: @h1Size * .6; }
h4 { font-size: @h1Size * .4; }
h5 { font-size: @h1Size * .2; }
```
- 计算颜色
```less
@mainColour: #631;
 
h1 { color: @mainColour; }
h2 { color: lighten(@mainColour, 10%); }
h3 { color: lighten(@mainColour, 20%); }
h4 { color: lighten(@mainColour, 30%); }
h5 { color: lighten(@mainColour, 40%); }
```

- 计算容器宽度
```less
@outerWidth: 960px;
@borderWidth: 10px;
@padding: 10px;
 
#container {
  width: @outerWidth - ( @borderWidth + @padding ) * 2;
  border: @borderWidth solid #999;
  padding: @padding;
} 
```

## 使用命名空间对内容进行分组
- 可以将变量和mixin分组到命名空间下，以使它们与其他规则集分开。如果您有很多LESS规则或包含多个LESS样式表，并且您希望避免命名冲突，这可能很方便
```less
#myNamespace {
  .selector1 { ... }
  #selector2 { ... }
}
```
 > 然后可以使用子选择器>引用命名空间中的规则

```less
#myNamespace > .selector1;
#myNamespace > #selector2;
```
- 比如从名为#articlePage的命名空间的最后一节封装我们的容器宽度示例
```less
#articlePage {
 
  @outerWidth: 960px;
  @borderWidth: 10px;
  @padding: 10px;
 
  #containerStyle {
    width: @outerWidth - ( @borderWidth + @padding ) * 2;
    border: @borderWidth solid #999;
    padding: @padding;
  }
 
}
```
> 现在可以将整个#containerStyle规则集混合到#container规则中以设置我们实际容器的样式
```less
#container { 
  #articlePage > #containerStyle;
}
```

## Less的注释
- 可以以两种不同的方式编写注释，比如常规CSS注释显示在LESS编译器生成的CSS输出中
```less
/* This comment will appear in the compiled CSS */
```
- 或者以less自己的方式
```less
// This comment will not appear in the compiled CSS
```

## 导入其他LESS和CSS文件
- 如果LESS样式表变得非常大，那么可能希望将其拆分为几个.less文件。然后使用@import指令将单个.less文件导入主样式表
```less
@import "common.less";
@import "forums.less";
```
- 如果要导入常规CSS文件而不通过LESS编译器运行它，只需确保文件具有.css扩展名
```less
@import "standard.css";  
// This file won't be parsed by LESS
```

## 使用e函数转义内容
- 函数允许转义值，以便它直接传递给已编译的CSS，而不会被LESS编译器解析。当使用LESS不理解的非标准CSS时，这可能很方便
```less
body {
  filter: e("progid:DXImageTransform.Microsoft.gradient(startColorstr='#aaaaaa', endColorstr='#666666')");
}
```
> 将会转义为css
```css
body {
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#aaaaaa', endColorstr='#666666');
}
```
## less新版本特性>1.5(current v 3.0)
### 引用reference
- 在老版本的less中，如果你在当前less文件中倒入了其他的less文件并希望引用其中的属性
```less
@import "external";
 
.box {
  .square;
}
```
编译为css文件后会生成.square mixin的样式规则
```css
.square {
  width: 100px;
  height: 100px;
  margin: 0 auto;
  background-color: red;
}
.box {
  width: 100px;
  height: 100px;
  margin: 0 auto;
  background-color: red;
}
```
> 而在1.5的新版本中，LESS提供了reference,用于指示LESS仅将导入文件用于参考，而不输出内容
```less
@import (reference) "external";
```
> 编译为css文件后显示为
```css
.box {
  width: 100px;
  height: 100px;
  margin: 0 auto;
  background-color: red;
}
```

### 继承
- entend就像是oo语言中的继承一样，它将具有相同样式规则的选择器分组，从而产生更清晰，更有条理的CSS的能力
> 比如说我们设定了如下样式
```css
.box {
  width: 100px;
  height: 100px;
  margin: 0 auto;
  border: 1px solid black;
  background-color: transparent;
}
.box2 {
  width: 100px;
  height: 100px;
  margin: 0 auto;
  border: 1px solid black;
  background-color: red;  
}
```
> 上述的样式是很冗余的，less在1.5版本中提供了@extend实现优化，该功能可以约等于&：extend()在1.4版本中
```less
@import (reference) "external";
.box {
	&:extend(.square);
	background-color: transparent;
}
.box2 {
	&:extend(.square);	
	background-color: red;
}
```
> 或者
```less
@import (reference) "external";
.box {
	@extend .square;
	background-color: transparent;
}
.box2 {
	@extend .square;
	background-color: red;
}
```

### 属性合并
- 此功能适用于接受多个值的CSS属性，如transform，transition和box-shadow。顾名思义，它将合并属于同一属性的值
> CSS3 Box Shadow属性接受多个阴影值 通过使用此合并属性 可以轻松构建阴影效果并使其更易于管理
```less
.inner-shadow {
  box-shadow+: inset 10px 10px 5px #ccc;
}
.outer-shadow {
  box-shadow+: inset 10px 10px 5px #ccc;
}
```
> +是允许使用属性合并的关键
```less
.box {
  .inner-shadow;
  .outer-shadow;
}
.box2 {
  .inner-shadow;
}
.box3 {
  .outer-shadow;
}
```
> 编译为css后会生成
```css
.box {
  box-shadow: inset 10px 10px 5px #ccc, 10px 10px 5px #ccc;
}
.box2 {
  box-shadow: inset 10px 10px 5px #ccc;
}
.box3 {
  box-shadow: 10px 10px 5px #ccc;
}
```
