
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


## 选项
[Liquid](https://liquidjs.com/api/classes/Liquid.html) 构造函数接受一个参数对象，用来定义各种模板引擎行为。这些参数都是可选的，比如我可以指定其中一个参数 `cache`：
```javascript
const { Liquid } = require('liquidjs')
const engine = new Liquid({
    cache: true
})
```

> **API 文档**
> 下面的所有选项的概述，希望了解具体的类型和签名，请前往 [LiquidOptions | API](https://liquidjs.com/api/interfaces/LiquidOptions.html).

### 缓存
**cache** 用来指定是否缓存曾经读取和处理过的模板来提升性能。在生产环境模板会重复渲染的情况会很有用。

默认是 `false`，当设置为 `true` 时会启用一个大小为 1024 的 LRU 缓存。当然也可以传一个数字来指定缓存大小。此外还可以是一个自定义的缓存实现，LiquidJS 会通过它来查找和读写文件。详情请参考 [Caching](https://liquidjs.com/zh-cn/tutorials/caching.html)。

### 布局和片段

**root** 用来指定 LiquidJS 查找和读取模板的根目录。可以是单个字符串，也可以是一个数组 LiquidJS 会顺序查找。详情请参考 [Render Files](https://liquidjs.com/zh-cn/tutorials/render-file.html)。

**layouts** 和 `root` 具有一样的格式，用来指定 `{% layout %}` 所使用的目录。没有指定时默认为 `root`。

**partials** 和 `root` 具有一样的格式，用来指定 `{% render %}` 和 `{% include %}` 所使用的目录。没有指定时默认为 `root`。

**relativeReference** 默认为 `true` 用来允许以相对路径引用其他文件。注意被引用的文件仍然需要在对应的 root 目录下。例如可以这样引用一个文件 `{% render ../foo/bar %}`，但需要确保 `../foo/bar` 处于 `partials` 目录下。

### 动态引用

> 注意由于历史原因这个选项叫做 dynamicPartials，但它对 layout 也起作用。

**dynamicPartials** 表示是否把传给 [include](https://liquidjs.com/zh-cn/tags/include.html), [render](https://liquidjs.com/zh-cn/tags/render.html), [layout](https://liquidjs.com/zh-cn/tags/layout.html) 标签的文件名当做变量处理。默认为 `true`。例如用上下文 `{ file: 'foo.html' }` 渲染下面的模板将会引入文件 `foo.html`：
```liquid
{% include file %}
```

设置 `dynamicPartials: false` 后 LiquidJS 将会尝试去读取 `file`。当你的模板之间都是静态引入关系时会很有用：
```liquid
{% liquid foo.html %}
```

> **常见陷阱**
> LiquidJS 把这个选项默认值设为 `true` 以兼容于 shopify/liquid，但如果你在使用 [eleventy](https://github.com/11ty/eleventy) 它会设置默认值 `false` （参考 [Quoted Include Paths](https://www.11ty.dev/docs/languages/liquid/#quoted-include-paths)）以兼容于 Jekyll。

### Jekyll include
v9.33.0

[jekyllInclude](https://liquidjs.com/api/interfaces/LiquidOptions.html#jekyllInclude) 用来启用 Jekyll-like include 语法。默认为 `false`，当设置为 `true` 时：
- 默认启用静态文件名：`dynamicPartials` 的默认值变为 `false`（而非 `true`）。但你也可以把它设置回 `true`。
- 参数的键和值之间由 `=` 分隔（本来是 `:`）。
- 参数放到了 `include` 变量下，而非当前作用域。

例如下面的模板中，`name.html` 没有带引号，`header` 和 `"HEADER"` 以 `=` 分隔，`header` 参数通过 `include.header` 来引用。更多详情请参考 [include](https://liquidjs.com/zh-cn/tags/include.html)。
```liquid
// entry template
{% include article.html header="HEADER" content="CONTENT" %}

// article.html
<article>
  <header>{{include.header}}</header>
  {{include.content}}
</article>
```

### extname
**extname** 定义了默认的文件后缀，当传入文件名不包含后缀时自动追加。默认值是 `''` 也就是说默认是禁用的。如果设置为 `.liquid`：
```liquid
{% render "foo" %}  没有后缀，添加 ".liquid" 并加载 foo.liquid
{% render "foo.html" %} 已经有后缀了，直接加载 foo.html
```

> **旧版行为**
> 在 2.0.1 之前，`extname` 默认值为 `.liquid`。要禁用它需要明确设置为 `extname: ''`。详情参考 [#41](https://github.com/harttle/liquidjs/issues/41)。

### fs
**fs** 用来自定义文件系统实现，详情请参考 [Abstract File System](https://liquidjs.com/zh-cn/tutorials/render-file.html#Abstract-File-System)。

### globals
**globals** 用来定义对所有模板可见的全局变量。包括 [render tag](https://liquidjs.com/zh-cn/tags/render.html) 引入的子模板，见 [3185](https://github.com/harttle/liquidjs/issues/185)。

### jsTruthy
**jsTruthy** 用来使用 Javascript 的真值判断，默认为 `false` 使用 Shopify 方式。
例如，空字符串在 JavaScript 中为假（`jsTruthy` 为 `true` 时），在 Shopify 真值表中为真。

### outputEscape
[outputEscape](https://liquidjs.com/api/interfaces/LiquidOptions.html#outputEscape) 用来自动转义输出。它的值可以是 `"escape"`、`"json"` 或 `(val: unknown) => string`，默认为 `undefined`。
- 如果被输出的变量不被信任，可以设置 `outputEscape: "escape"` 来自动把它们 HTML 转义。如果要直接输出则需要使用 [raw](https://liquidjs.com/zh-cn/filters/raw.html) 过滤器。
- 如果你在用 LiquidJS 来生产 JSON 文件，可以设置为 `"json"`。
- `outputEscape` 甚至可以是函数，你可以借此控制整个 LiquidJS 的变量输出。注意函数的输入不一定是字符串，因为过滤器的返回值可以不是字符串，你的函数将会接到这个值。

### 时间日期和时区
**timezoneOffset** 用来指定一个和你当地时区不同的时区，所有日期和时间输出时都转换到这个指定的时区。例如设置 `timezoneOffset: 0` 将会把所有日期按照 UTC/GMT 00:00 来输出。

**preserveTimezones** 是一个布尔值，只影响时间戳字面量。当设置为 `true` 时，所有字面量的时间戳字符串会在输出时保持原状，即不论输入时采取怎样的时区，输出时仍然采用那一时区（和 Shopify Liquid 的行为一致）。注意这是一个解析器参数，渲染时传入的数据中的日期的输出不会受此参数影响。注意 `preserveTimezones` 比 `timezoneOffset` 的优先级更高。

**dateFormat** 用于指定输出日期的默认格式. `%A, %B %-e, %Y at %-l:%M %P %z` 如果未指定，将使用. 例如，设置 `dateFormat: %Y-%m-%dT%H:%M:%S:%LZ` 以输出 [JavaSrcipt Date.toJson()][[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/toJSON]](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/toJSON]) 格式.

### 换行和缩进
**greedy**, **trimOutputLeft**, **trimOutputRight**, **trimTagLeft**, **trimTagRight** 选项用来移除 Liquid 语法周围的换行和缩进，详情请参考 [Whitespace Control](https://liquidjs.com/zh-cn/tutorials/whitespace-control.html)。

### 自定义分隔符
**outputDelimiterLeft**, **outputDelimiterRight**, **tagDelimiterLeft**, **tagDelimiterRight** 用来自定义 LiquidJS 中 [标签和过滤器](https://liquidjs.com/zh-cn/tutorials/intro-to-liquid.html) 的分隔符。例如设置了 `outputDelimiterLeft: <%=, outputDelimiterRight: %>` 后我们可以避免跟其他模板引擎冲突：
```ejs
<%= username | append: ", welcome to LiquidJS!" %>
```
### 严格模式
**strictFilters** 用来启用过滤器的严格模式，如果设置为 `true` 过滤器不存在时解析会抛出异常。默认为 `false`，这时会跳过不存在的过滤器。

**strictVariables** 用来启用变量严格模式。如果设置为 `true` 变量不存在时渲染会抛出异常，默认为 `false` 这时不存在的变量会被渲染为空字符串。

**ownPropertyOnly** 用来隐藏原型上的变量，如果你需要把未经处理过的对象传递给模板时，可以设置 `ownPropertyOnly` 为 `true`，默认为 `false`。

> **不存在的标签** 
> 不存在的标签总是会抛出一个解析异常，这一行为无法自定义。

### 参数顺序
默认会忽略参数出现的顺序，例如 `{% for i in (1..8) reversed limit:3 %}` 里总是会先执行 `limit` 再执行 `reversed`，虽然 `reversed` 先出现。为了让 LiquidJS 按顺序执行参数，需要设置 **orderedFilterParameters** 为 `true`。它的默认值为 `false`。




## References:
[1] https://liquidjs.com/zh-cn/tutorials/options.html
























