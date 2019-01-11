### less
Less（Leaner Style Sheets 的缩写）是一门向后兼容的CSS扩展语言。这里呈现的是Less的官方文档（中文版），包含了Less语言以及利用 JavaScript开发的用于将Less样式转换成CSS样式的Less.js工具

#### 嵌套
```bash
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
```
用较少的方式，我们也可以这样写：
```bash
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```
就像div盒子布局一样 一层套一层 代码更简洁

#### 变量
```bash
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```
编译为：
```bash
#header {
  width: 10px;
  height: 20px;
}
```
#### 混合
```bash
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```
我们希望在其他规则集中使用这些属性。好吧，我们只需输入我们想要属性的类的名称，如下所示：
```bash
#menu a {
  color: #111;
  .bordered();
}
.post a {
  color: red;
  .bordered();
}
```
#### 嵌套at-规则和冒泡
规则，如@media或@supports可以与选择器相同的方式嵌套。at-规则放在顶部，与同一规则集中的其他元素的相对顺序保持不变。这叫做冒泡。
```bash
.component {
  width: 300px;
  @media (min-width: 768px) {
    width: 600px;
    @media  (min-resolution: 192dpi) {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    width: 800px;
  }
}
```
编译为：
```bash
.component {
  width: 300px;
}
@media (min-width: 768px) {
  .component {
    width: 600px;
  }
}
@media (min-width: 768px) and (min-resolution: 192dpi) {
  .component {
    background-image: url(/img/retina2x.png);
  }
}
@media (min-width: 1280px) {
  .component {
    width: 800px;
  }
}
```