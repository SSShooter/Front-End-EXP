# Front-End-EXP
无限期更新前端的一些坑，一些记录，一些冷知识

> 大概从 17 年的六月份开始记录吧，也已经一年了，其中也包含了一些很简单的知识，以前还觉得挺难的，现在看起来有点谜之感慨...

## JavaScript
- 所有对象都有 `__proto__` 属性，都指向创造对象的函数对象的 `prototype`。
- 上传文件要使用 `formdata` 包装。
- `Promise.prototype.catch` 方法是 `.then(null, rejection)` 的别名。
- 同一个 EventTarget 注册了多个相同的 EventListener，那么重复的实例会被抛弃。所以这么做不会使得 EventListener 被调用两次，也不需要用 removeEventListener 手动清除多余的EventListener，因为重复的都被自动抛弃了。
- 当使用 `addEventListener()` 为一个元素注册事件的时候，句柄里的 `this` 值是该元素的引用。其与传递给句柄的 `event` 参数的 `currentTarget` 属性的值一样,而 `target` 是直接接受事件的子元素。
- `scrollIntoView()` 使 div 底部滚动到视窗。
- 使用 const 的对象和数组的内容是可以被修改的，但数据结构不可变。
- 在 webpack 里，所有的文件都是模块。loader 的作用就是把文件转换成 webpack 可以识别的模块。
- 关于事件循环，执行下一个 task 之前总会清空 microtask。
- npm 新旧版本覆盖可能会造成迷之问题，这个时候可以试试 node_module 整个删掉重装。
- *、/ 和 - 操作符都是数字运算专用的。当这些运算符与字符串一起使用时，会强制转换字符串为数字类型的值。
- `document.title` 可以直接修改当前 html 的标题。
- 利用对象浅拷贝修改对象，指向同一对象的两个变量修改对象的效果一样。
- 脑洞题：1 和 2 只用一次的情况下怎么得到 4  答案：1<<2。
- 连等赋值从右到左。
- `compositionstart` 事件触发于一段文字的输入之前（类似于 keydown 事件，但是该事件仅在若干可见字符的输入之前，而这些可见字符的输入可能需要一连串的键盘操作、语音识别或者点击输入法的备选词）。
- `void 0`（void后面加任何东西）用于生成一个绝对的 `undefined`（不会被重定义），但跟函数会有副作用
- 注意 `localStorage` 保存的只能是字符串，所以是没有 `null` 的。
- 坑一个
    ```
    typeof [] === 'object' // true
    typeof null === 'object' // true
    ```
- ~~import 同步，require异步（待补充）~~     
    在 issue 中讨论后确定这一条的定义没有意义，不过可以确定的是 require 可以适用于异步加载，两者更多区别可以在[这里](https://stackoverflow.com/questions/31354559/using-node-js-require-vs-es6-import-export/31355283#31355283)了解。
- `new()` 做了些什么？ 
    ```javascript
    var obj = new Base();
    var obj  = {};
    obj.__proto__ = Base.prototype;
    Base.call(obj);
    ```
- stage 0 到 4 的含义：
    > stage 0 is "i've got a crazy idea",    
    stage 1 is "this idea might not be stupid",    
    stage 2 is "let's use polyfills and transpilers to play with it",    
    stage 3 is "let's let browsers implement it and see how it goes",    
    stage 4 is "now it's javascript".    
- `Object.getOwnPropertyNames(obj).length === 0` 判断 `obj` 是不是空对象。
- `getBoundingClientRect()` 用于获取元素宽高以及距离页面边框距离，十分方便。
- `&&` 的使用场景：左边为真继续执行右边，如 `isDog && bark()`。
- `||` 的使用场景：左边为假继续执行右边，如 `let i = value || defalutValue`。

## Vue.js
- v-model 会自动 bind 一个值，其变量名为 value。
- 多个特性的元素应该分多行撰写，每个特性一行。以下为 vscode 里 vetur 的设置：
    ```
    "vetur.format.defaultFormatterOptions": {
        "js-beautify-html": {
          "wrap_attributes": "force" 
        }
      }
    ```
- 组件 destroy 时触发自定义指令的 unbind，destory 的时机：diff 之后的 patch,如 v-if，v-for（key不同时，先销毁原来的，再挂载新的（推测））
- 自定义组件 v-model watch 自动匹配（存疑
- 组件的 data 属性要用函数返回的原因：创建实例时如果 data 是一个对象的话，所有实例都会引用同一个对象，而对象返回不会有此问题。在浏览器中可以这么做是因为根实例只有一个。
- .vue 文件中使用 `/deep/` 覆盖子组件 css

## GitHub
- 从 commit 关闭 issues 的方法：commit 信息写 `Fixes #33`，其他关键字还有 `close closes closed fix fixes fixed resolve resolves resolved`
- `git reset --soft HEAD^` 回退一次 commit

## CSS
- div 的默认宽度是父元素宽的 100%
- 逐帧动画 `animation: animate-name 3s steps(每次循环的帧数) infinite;`
    ```css
    @keyframes animate-name{
        from{
        <!--原位-->
            background-position: 0 0; 
        }
        to{
        <!--最后一帧-->
            background-position: -1540px 0 ;
        }
    }
    ```
- "Break out" of a parent's containing width to take the full screen of a page w/this nice utility class:
    ```css
    .full-width {
      width: 100vw;
      position: relative;
      left: 50%;
      right: 50%;
      margin-left: -50vw;
      margin-right: -50vw;
    }
    ```
- 行内元素与下一个元素中间有空格（正常排版）,会引起一些诡异的缝隙，常见的例如元素之间有间隔，或看起来空了一行（像加了padding）
- pre 标签设置自动换行  `white-space: pre-wrap;`
- 隐藏一个元素，同时让这个元素的宽度可获取：设置 `overflow: hidden` 可以根据元素高度裁剪视区，设置 `height: 0; overflow: hidden` 虽然文档流中占用了位置，由于高度为 0，最终表现特征达到了我们期望的 `display: none`。此时该元素 `clientHeight`、`offsetHeight` 为 0，但是 `scrollHeight` 是有值的。`scrollHeight` 是一个元素没有滚动条时的所有内容高度,当一个元素没有滚动条时 `scrollHeight === offsetHeight`。
- 当 Render Tree 中部分或全部元素的尺寸、结构、或某些属性发生改变时，浏览器重新渲染部分或全部文档的过程称为回流。
- 当页面中元素样式的改变并不影响它在文档流中的位置时（例如：color、background-color、visibility等），浏览器会将新样式赋予给元素并重新绘制它，这个过程称为重绘。
- 回流必将引起重绘，重绘不一定会引起回流。
- 移动端优化常用 CSS 属性：
    ```
    user-select: none; // 禁止文字被选中
    outline:none; // 去除点击外边框,点击无轮廓
    -webkit-touch-callout: none; // 长按链接不弹出菜单
    -webkit-tap-highlight-color: rgba(0,0,0,0); // 去除点击高亮
    ```
- @keyframes 的属性要前后对应，否则不形成动画
- img 元素图像自适应居中，与 `background-size` 效果一样
    ```
    object-fit: cover; 
    ```
- `<img />` 标签千万记得写宽高，不然会花式塌陷
- `flex-grow` 所在元素如果未定宽度的话，宽度会被子元素撑开
- 一个英文单词默认不换行，无论多长，所以要设置 `word break`
- 多行文字居中
    ```
    .mulit_line{ 
        border:1px dashed #cccccc; 
        padding-left:5px;
    }
    .mulit_line span{ 
        display:inline-block; 
        line-height:14px; 
        vertical-align:middle;
    }
    ```
- safari 中控制惯性滚动 -webkit-overflow-scroll
- 滚动条样式可能使滚动条强制显示（未确定）
- flex 布局不换行加 `flex-shrink: 0;` 实现 div 不压缩无限并排，可以用于 swiper 等场景。
- 巧用猫头鹰选择器 * + * 
- `float` 自带 `display: block`
- 鼠标禁用 `.disabled { pointer-events: none; }`
- 注意 :last-child 与 :last-of-type 的区别
- ::after 表示法是在 CSS3 中引入的，:: 符号是用来区分伪类和伪元素的。支持CSS3的浏览器同时也都支持 CSS2 中引入的表示法 :after。
- 父元素如果存在 transform 属性，子元素的 position: fixed 属性无效
- less 中的 calc 问题：`height: calc(~"100% - 50px");`
- vh 在部分浏览器中包含地址栏部分，小心使用。

## VSCode
- alt + shift + 鼠标点击 纵向选择
- vetur 分号问题: 安装 prettier，然后配置 `"prettier.singleQuote":true,"prettier.semi": false`
- 可以使用插件 document this 方便写注释
- html tag 属性分行 wrap_attributes:force
- 选定变量后按 F12 找到定义位置

## 其他
- 魔法隧道用 webpack 代理会 502
- 在组件化编程中，悼念被同名组件浪费了几个钟的 debug 时间[蜡烛]
- 局域网连不通的话，先试试，开共享，关闭防火墙
- 局域网连不通的话，还可以试试，在 webpack.config.js 文件的 devServer 里加上`host:'0.0.0.0'`。
- iOS 的回弹效果，动的是 body 部分，html 是不动的
- 学习一个语言之前先看规范
- safari 的 formdata 只支持 append，其他方法需要 polyfill
- rc 的意思是 run commands
- 导航栏高度 88px，标签栏高度 98px（iphone5和6常用）
- 关于 HTTP 304 Not Modified，简单来说，请求内容没有发生变化的时候，根据设置，服务器可能直接取缓存返回
