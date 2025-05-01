<script setup>
import PasswordProtect from '../components/PasswordProtect.vue'
</script>

<PasswordProtect password="9527" title="请输入查看密码">

## 自我介绍

来自xxx,

上家公司做的前端开发,参与主要产品和内部平台的开发和维护,都是web项目,

主要产品,负责5,6个模块和部分公共页的开发和维护,也独立负责了一个内部平台的开发

都是使用的vue, 我个人比较熟悉vue,虽然没怎么用过react,我觉得我也比较熟悉js,上手也会很快

## vue: 在渲染过程中，父子组件的生命周期是什么样的？更新的过程？

当父子组件首次渲染时，生命周期钩子的执行顺序如下

* 父 beforeCreate->父 created->父 beforeMount->子 beforeCreate->子 created->子 beforeMount->子 mounted->父 mounted

当父组件状态变化导致重新渲染时：

* 父 beforeUpdate->子 beforeUpdate->子 updated->父 updated

当父组件销毁时：

* 父 beforeDestroy->子 beforeDestroy->子 destroyed->父 destroyed

## vue:v-model是什么东西

用于实现**双向数据绑定**的指令，本质上是一个语法糖

v-model 的工作原理可以概括为：

1. **数据到视图**：通过 `v-bind` 将数据绑定到表单元素的 value/checked 等属性
2. **视图到数据**：通过 `v-on` 监听输入事件（如 input、change）来更新数据
3. 相当于帮你做了这样的操作: **文本输入框（input/textarea）**：绑定 `value` 属性，监听 `input` 事件,**复选框（checkbox）**：绑定 `checked` 属性，监听 `change` 事件

**自定义组件上的 v-model**
在 Vue2 中，自定义组件使用 v-model 需要：

1. 接收一个名为 `value` 的 prop
2. 在需要更新值时触发 `input` 事件

### .sync 修饰符

Vue2 还提供了 `.sync` 修饰符作为 v-model 的替代方案，允许"双向绑定"多个 prop：

```
<child-component :title.sync="pageTitle"></child-component>
<!-- 等价于 -->
<child-component 
  :title="pageTitle" 
  @update:title="pageTitle = $event">
</child-component>
```

子组件通过 `this.$emit('update:title', newTitle)` 来更新



Vue3 里的:
**默认 prop 和事件名变更**：

- 默认 prop 名从 `value` 改为 `modelValue`
- 默认事件名从 `input` 改为 `update:modelValue`
- 移除sync修饰符

## vue:nextick做什么的？怎么实现的
## 浏览器：a标签与b标签通信，sessionstorage怎么样可以不同标签共享

不能吧,

**sessionStorage 不能在多个窗口或标签页之间共享数据，但是当通过 window.open 或链接打开新页面时(不能是新窗口)，新页面会复制前一页的 sessionStorage。**(协议,域名,端口都会产生不同的域, 页面关闭就清除对应的域)

## es6:Map与weakMap 有什么区别

| 特性     | Map                           | WeakMap                      |
| :------- | :---------------------------- | :--------------------------- |
| 键类型   | 任意类型                      | 只能是对象（不包括基本类型） |
| 垃圾回收 | 不会自动释放内存              | 键对象无引用时自动回收       |
| 可枚举性 | 可遍历（keys/values/entries） | 不可遍历                     |
| 大小查询 | 有 size 属性                  | 无 size 属性                 |

## webpack:loader 与plugin 有什么区别？为什么需要loader的

loader:让webpack能够处理非js文件(自身职能理解js)，转换成js,然后你就可以利用 webpack 的打包能力，对它们进行处理。
例如：css-loader、style-loader、postcss-loader、sass-loader

plugins:从打包优化和压缩，一直到重新定义环境中的变量.比如 生成HTML文件需要使用HtmlWebpackPlugin
例如：uglify-webpack-plugin、clean-webpack-plugin、babel-polyfill

## webpack:treeshaking怎么实现的

webpack实现tree shaking的原理是基于ES6模块化语法的静态特性。

在编译阶段，Webpack会根据模块的依赖关系，通过AST（抽象语法树）进行静态分析，识别出那些代码块（函数、变量、对象等）被引用并且使用了。然后将这些代码块打包输出到最终的打包文件中。在这个过程中，Webpack会自动将未被引用的代码块进行剔除，这个过程就是tree shaking。

具体来说，当Webpack在打包时遇到一个ES6模块导入语句（import），它会自动去加载这个模块并分析其导出对象。然后它会分析项目中哪些导出对象被引用了。如果一个导出对象没有被引用，那么Webpack会直接把它从最终的代码中剔除掉。

需要注意的是，tree shaking只对ES6模块生效，对于CommonJS等其他模块化规范，由于其动态加载特性，无法在静态分析阶段确定哪些代码块被引用，因此无法进行tree shaking。

另外，为了使Webpack能够正确识别和剔除未引用的代码块，开发者也需要做出一定的努力，例如将代码编写为纯函数的形式，避免使用全局变量等副作用等。

## 换肤功能怎么实现

用到scss, 在html元素上 setAttribute('theme-mode'), 写样式的使用 用属性选择器[theme-mode='light'] 匹配样式即可, 可以按主题分开放样式文件,比较好维护



## tsconfig.json 配置



## vue2到vue3变了什么东西

v-model 从绑定 value 和input 事件, 变成 modelValue 和 update:modelValue

.sync 去除

filter去除,可用methods 或 computed 

新的改用composition api, 旧的继续先保持options写法
新版没有this, 方法还要 defineExpose 出去, 才能被调用

computed watch等变成按需引入

composition api生命周期和options 不一样

##想反问的东西

评价一下我的表现,有什么不足之处呢

想了解一下贵司react的使用情况,大概会到什么水平可以胜任岗位呢


</PasswordProtect>