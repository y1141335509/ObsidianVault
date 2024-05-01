
## Liquid 模板语言
LiquidJS 是一个简单的、安全的、兼容 Shopify 的、纯 JavaScript 编写的模板引擎。这个项目的目的是为 JavaScript 社区提供一个 Liquid 模板引擎的实现。Liquid 最初用 Ruby 实现并用于 Github Pages, Jekyll 和 Shopify，参考 [和 Shopify/liquid 的区别](https://liquidjs.com/zh-cn/tutorials/differences.html)。
LiquidJS 语法相对简单。LiquidJS 中有两种标记：
- **标签**。标签由标签名和参数构成，由 `{%` 和 `%}` 包裹。
- **输出**。输出由一个值和一组可选的过滤器构成，由 `{{` 和 `}} 包裹。

> **在线示例**
> 在进一步了解细节之前，这里有一个在线示例：[https://liquidjs.com/playground.html](https://liquidjs.com/playground.html)。

### 输出
**输出** 用于转换和输出变量到 HTML。下面的模板将会把 `username` 的值插入到 input 的 `value`：
```liquid
<input type="text" name="user" value="{{username}}">
```

_输出_ 里的值可以在输出之前经过若干个 **过滤器** 的转换。比如在变量后面追加一个字符串：
```liquid
{{ username | append: ", welcome to LiquidJS!" }}
```

过滤器可以级联，用起来像管道一样：
```liquid
{{ username | append: ", welcome to LiquidJS!" | capitalize }}
```

[这里](https://liquidjs.com/zh-cn/filters/overview.html) 是 LiquidJS 支持的完整的过滤器列表。

### 标签
**标签** 用于控制模板渲染过程，操作模板变量，和其他模板交互等。例如 `assign` 可以用来定义一个模板中可以使用的变量：
```liquid
{% assign foo = "FOO" %}
```

一般标签成对地出现，一个开始标签和一个对应的结束标签，比如：
```liquid
{% if foo == "FOO" %}
    Variable `foo` equals "FOO"
{% else %}
    Variable `foo` not equals "FOO"
{% endif %}
```
[这里](https://liquidjs.com/zh-cn/tags/overview.html) 是 LiquidJS 支持的完整的标签列表。


## 安装和使用
如果你还不了解 Liquid 模板语言，请参考 [Liquid 模板语言简介](https://liquidjs.com/zh-cn/tutorials/intro-to-liquid.html)。
### 在Node.js 里使用
通过 NPM 安装：
```bash
npm install --save liquidjs
```

```javascript
var { Liquid } = require('liquidjs');
var engine = new Liquid();

engine
    .parseAndRender('{{name | capitalize}}', {name: 'alice'})
    .then(console.log);     // 输出 'Alice'
```

> **示例**
> 这里有一个 LiquidJS 在 Node.js 里使用的例子：[liquidjs/demo/nodejs/](https://github.com/harttle/liquidjs/blob/master/demo/nodejs/).

LiquidJS 的类型定义也导出并发布到了 NPM 包里，写 TypeScript 的项目可以直接这样使用：
```typescript
import { Liquid } from 'liquidjs';
const engine = new Liquid();

engine
    .parseAndRender('{{name | capitalize}}', {name: 'alice'})
    .then(console.log);     // 输出 'Alice'
```

> **示例**
> 这里有一个 LiquidJS 在 TypeScript 下的例子：[liquidjs/demo/typescript/](https://github.com/harttle/liquidjs/blob/master/demo/typescript/).

### 在浏览器里使用

LiquidJS 预先构建了 UMD Bundle，可以通过 jsDelivr CDN 来引用：
```html
<script src="https://cdn.jsdelivr.net/npm/liquidjs/dist/liquid.browser.min.js"></script>     <!--生产环境-->
<script src="https://cdn.jsdelivr.net/npm/liquidjs/dist/liquid.browser.umd.js"></script>     <!--开发环境-->
```

> **示例**
> 这里有一个 jsFiddle 上的在线例子：[jsfiddle.net/x43eb0z6](https://jsfiddle.net/x43eb0z6/)，其源码也可以在 [liquidjs/demo/browser/](https://github.com/harttle/liquidjs/blob/master/demo/browser/) 找到。

> **兼容性**
> 在类似 IE 和 Android UC 这样的浏览器中，你可能需要引入 [Promise polyfill](https://github.com/taylorhakes/promise-polyfill)，参看 [caniuse 的统计](http://caniuse.com/#feat=promises)。

### 在命令行里使用
你还可以在命令行里使用 LiquidJS：
```bash
echo '{{"hello" | capitalize}}' | npx liquidjs
```

模板来自标准输入，数据则来自参数，这个参数可以是一个 JSON 文件的路径，也可以是一个 JSON 字符串：
```bash
echo 'Hello, {{ name }}.' | npx liquidjs '{"name": "Snake"}'
```

### 其他
[@stevenanthonyrevo](https://github.com/stevenanthonyrevo) 还提供了一个 ReactJS demo，请参考 [liquidjs/demo/reactjs/](https://github.com/harttle/liquidjs/blob/master/demo/reactjs/)。































