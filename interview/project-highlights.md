
# 项目难点亮点

## 工厂模式重构代码

**情境**: 开发了一个 cloudos, OpenStack 资源所用的表格页面, 两个资源的表格构成基本相同,代码就都写在一个组件文件内, 但后续有多个资源需要实现类似的表格页面, 且表格配置,行列各有差异

**任务**: 开发时若继续沿用之前写在同一文件的模式, 传给表格的配置对象就很乱, 而且一些(crud)方法中也需要去判断资源类型,重复代码多, 后面也不好去维护. 想有一种减少重复代码,结构清晰的方案

**行动**:  利用工厂模式重构了代码, 

1. 创建 TableFactory 类作为统一入口, 统一 new 它生成表格和行列配置对象, new时传入资源类型等参数
2. 再写一个 Base类, 初始化通用的配置对象
    有 initOptions initColumns 方法
3. 各种资源类型继承 Base, 如果有不同,就在子类里重写 Base 中方法

**结果**: 组件中减少了很多重复判断代码, 结构也更清晰,每个资源有自己单独的文件,后续再新加资源也很方便

## 资源界面前端实现联动查询

**情境**: 新版资源界面需优化用户体验, 要支持多条件联动查询主机和资源

**任务:** 
  后台只返回主机,资源,实例的结果, 联动查询需在前端去实现;
  上方需要统计主机状态,资源状态,实例类型,标签等,下方需要根据条件显示主机结果
   [描述一些样式]

* 因为主机>资源>实例, 数据中后者会包含前者的uuid, 怎么快速过滤出符合结果的主机是个问题, 比如选了主机在线, 资源已激活,实例类型mysql, 先去循环判断主机, 再从得出的结果中循环判断资源,再去判断实例,就比较多层了
    解决方法: 主机,资源,实例会存到vuex中, resources页面用 computed 获取,
    单独写了个hostFilter方法用于判断, 将资源和实例的状态数据作为一个参数 传入,形式是host uuid作为key, val是cache对象,
* 上方显示资源状态的时候已经计算过一次, 下方计算cache又算了一次.
    于是上方用v-model双向绑定了一个变量,用来传递计算后的结果,格式如: { online: [ host_uuid.....  ]}, 下方计算的时候,就判断是否在uuid数组里, 在就带上 key(online) , 然后此数组就作为上面 val cache 的一个字段, 后面 hostFilter 方法判断的时候就可以直接字符串比对这个数组
    相当于之前 hostFilter 判断是 { online: [ uuid...] } , 用 hostuuid 遍历看是否包含其中, 现在 hostuuid 对应单独一个 cache 对象,直接从里面的instance_states 数据判断是否有 online 字符串就可以进行筛选
* 界面样式想实现窗口缩小宽度时, 放不下的项隐藏掉, 改用一个 ... 按钮代替,点击按钮展示所有项和勾选情况. 我将一行分为左右两部分,左边循环渲染选项,右边是 ... 按钮,用v-show控制显示, 用 window.addeventlistener 监听 resize 事件,因为左边是flex布局且换行,当换行后左部分的scrollHeight 会大于 clientHeight,就可以让按钮进行展示了下拉选择框,
    同时下拉选择框是一个库,样式不太匹配,我就用:deep 样式穿透 隐藏了,然后监听按钮的点击来调用打开关闭来实现交互

**行动:**如上

**结果:**

## 封装高级搜索组件

**情境**: 

**任务:**

**行动:**

**结果:**



## 项目优化情况

* 节流防抖
* 代码分割
* 路由懒加载
* localstorage 缓存数据
* 避免重排/重绘
* 用 css 动画代替 js 动画
* gzip压缩, vite用vite-plugin-compression
* tree shaking, vite默认启用的
* 区分生产环境和开发环境做特殊处理
* 图片压缩
* css避免深层嵌套
