# Vue Component Spec
Vue 组件规格

A `*.vue` file is a custom file format that uses HTML-like syntax to describe a Vue component. Each `*.vue` file consists of three types of top-level language blocks: `<template>`, `<script>` and `<style>`:

一个以 `*.vue` 为后缀名的文件就是一个 Vue 组件, 在组件内你可以使用与 HTML 相似的语法, 高亮, 格式化. 每个 `*.vue` 文件包含三种类型的语言块, 模板, 脚本, 样式, 你可以将其对应为 HTML, JavaScript, CSS.

``` html
<template>
  <div class="example">{{ msg }}</div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Hello world!'
    }
  }
}
</script>

<style>
.example {
  color: red;
}
</style>
```

`vue-loader` will parse the file, extract each language block, pipe them through other loaders if necessary, and finally assemble them back into a CommonJS module whose `module.exports` is a Vue.js component options object.

`vue-loader` 会解释这个文件, 然后最终将其打包为一个 CommonJS 模块.

`vue-loader` supports using non-default languages, such as CSS pre-processors and compile-to-HTML template languages, by specifying the `lang` attribute for a language block. For example, you can use SASS for the style of your component like this:

`vue-loader` 支持非默认语言, 只要能被 Webpack 预处理的应该都可以, 你只需要像下面那样标明语言.

``` html
<style lang="sass">
  /* write SASS! */
</style>
```

More details can be found in [Using Pre-Processors](../configurations/pre-processors.md).

### Language Blocks

#### `<template>`

- Default language: `html`.

- Each `*.vue` file can contain at most one `<template>` block at a time.
  一个 `*.vue` 文件最多只能包含一个 `<template>` 块.

- Contents will be extracted as a string and used as the `template` option for the compiled Vue component. 

#### `<script>`

- Default language: `js` (ES2015 is supported by default via Babel).

- Each `*.vue` file can contain at most one `<script>` block at a time.
  一个 `*.vue` 文件最多只能包含一个 `<script>` 块.

- The script is executed in a CommonJS like environment (just like a normal `.js` module bundled via Webpack), which means you can `require()` other dependencies. And because ES2015 is supported by default, you can also use ES2015 `import` and `export` syntax.

- The script must export a Vue.js component options object. Exporting an extended constructor created by `Vue.extend()` is also supported, but a plain object is preferred.

#### `<style>`

- Defalt Language: `css`.

- Multiple `<style>` tags are supported in a single `*.vue` file.
  一个 `*.vue` 文件中可以有多个 `<style>` 块.

- By default, contents will be extracted and dynamically inserted into the document's `<head>` as an actual `<style>` tag using `style-loader`. It's also possible to [configure Webpack so that all styles in all components are extracted into a single CSS file](../configurations/extract-css.md).

### Src Imports

If you prefer splitting up your `*.vue` components into multiple files, you can use the `src` attribute to import an external file for a language block:
如果你想要拆分你的 `*.vue` 组件到多个文件当中, 你可以使用 `src` 属性.

``` html
<template src="./template.html"></template>
<style src="./style.css"></style>
<script src="./script.js"></script>
```

Beware that `src` imports follow the same path resolution rules to CommonJS `require()` calls, which means for relative paths you need to start with `./`, and you can import resources directly from installed NPM packages, e.g:
注意, `src` 的导入方法关于路径的解释和 CommonJS 中 `require()` 方法相似, 也就是说你要使用以 `./` 开头的相对路径. 你也可以直接从 NPM 包种导入资源, 比如:

``` html
<!-- import a file from the installed "todomvc-app-css" npm package -->
<style src="todomvc-app-css/index.css">
```

### Syntax Highlighting
语法高亮

Currently there are syntax highlighting support for [Sublime Text](https://github.com/vuejs/vue-syntax-highlight), [Atom](https://atom.io/packages/language-vue), [Vim](https://github.com/posva/vim-vue) and [Visual Studio Code](https://marketplace.visualstudio.com/items/liuji-jim.vue). Contributions for other editors/IDEs are highly appreciated! If you are not using any pre-processors in Vue components, you can also get by by treating `*.vue` files as HTML in your editor.
目前主流的编辑器都有 `*. vue` 文件的语法高亮插件.
