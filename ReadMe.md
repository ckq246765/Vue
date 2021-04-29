# Vue
## 开发规范
### BEM命名方式(css)：
- block :代表块
- element :代表元素
- modifier :代表修饰符

``` css
  .block {}
  .block__element {}
  .block--modifier {}
```
### Eslint(检查JavaScript编程语法错误：编程规范，控制代码质量)
- 首先安装Node.js(>=4.x),nmp version 2+
- 全局安装或者本地安装
- 安装npx

``` js
  npm i npx -g
  yarn global add npx
  //npx执行指令的时候，先在当前目录查找=>全局=>零时安装
  //推荐facebook包管理yarn
  npm i yarn -g
  npm install elint //本地安装
  yarn add eslint //本地安装

  yarn global add eslint //全局安装
  npm install eslint -g //全局安装

  yarn config set registry http://registry.npm.taobao.org //设置淘宝镜像
  //让eslint成为项目构建的一部分，可以选择本地安装
  npm install eslint --save-dev
  //设置配置文件
  npx eslint --init
  //项目运行eslint:执行检测
  npx eslint yourfile.js
  npx eslint yourfile.js --fix //执行可以代码修复

  //全局安装eslint:
  npm install -g eslint
  eslint --init //设置配置文件
  npx eslint yourfile.js  //项目运行eslint:执行检测
```

## MVVM
- V ：视图
- VM ：同步Model和View的对象（监听）
- M ：数据模型

1. {{}} 插值表达式
2. v-text 转义输出
3. v-html 不转义输出

``` html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
  </head>
  <body>
      <div id="app">
          <fieldset>
              <legend>Vue测试</legend>
              <!-- {{}}  插值表达式： -->
              <h2>{{test01}}{{test02}}</h2>
              <h2>{{test01}}</h2>
              <h2>{{test02}}</h2>
              <!-- <h2>{{test02 = test02 + "测试"}}</h2> -->
              <h2>{{num + 20}}</h2>
              <h2>{{age > 18? "恭喜成年" : "未成年"}}</h2>
              <h2>{{test01.split('').reverse().jion('')}}</h2>
              <!-- v-text 转义输出内容 -->
              <!-- v-html 不转义输出 -->
              <h2 v-text="html"></h2>
              <h2 v-html="html"></h2>
          </fieldset>
      </div>
  </body>
  </html>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.js"></script>
  <script>
      //Vue是一个MVVM的架构，是一个响应式的架构，响应式的原理是ES5的Object.definedProperty()
      //1. 实例化Vue
      const vm = new Vue({
          el: "#app", //vue的挂载区域，vue编译的范围
          data: {     //数据层：Model
              test01: "我是测试数据01",
              test02: "我是测试数据02",
              num: 80,
              age: 18,
              html: `<img src='http://123456.jgp' alt='图片'>`
          }
      })
  </script>
```

4. v-bind 可以给html元素或者组件动态地绑定一个或多个特性，例如动态绑定style和class
``` html
    1、作用：可以给html元素或者组件动态地绑定一个或多个特性，例如动态绑定style和class
    2、举例：
        1、img的src从imageSrc变量中取得
            <img v-bind:src="imageSrc" >

        2、从classA, classB两个变量的值作为class的值，
          结果是：<div class="A B">classA, classB</div>        
          <div v-bind:class="[classA, classB]">classA, classB</div>

          3、isRed变量如果为true，则class的值为red，否则没有
          <div v-bind:class="{ red: isRed }">isred</div>

          4、div的class属性值一定有classA变量的值，而是否有classB和classC变量的值取决于isB和isC是否为true，二者一一对应
          <div v-bind:class="[classA, { classB: isB, classC: isC }]">数组对象</div>

          5、变量加常量
          <div v-bind:style="{ fontSize: size + 'px' }">size22</div>

          6、变量加常量的另一种写法
          <img v-bind="{src:imageSrc+'?v=1.0'}" >

          7、对象数组
          <div v-bind:style="[styleObjectA, styleObjectB]">styleObjectA, styleObjectB</div>

    3、缩写形式
        <img :src="imageSrc">
        <div :class="[classA, classB]">classA, classB</div>
        <div v-bind:class="{ red: isRed }">isred</div>
        <div v-bind:class="[classA, { classB: isB, classC: isC }]">数组对象</div>
        <div v-bind:style="{ fontSize: size + 'px' }">size22</div>
        <img v-bind="{src:imageSrc+'?v=1.0'}" >
        <div v-bind:style="[styleObjectA, styleObjectB]">styleObjectA, styleObjectB</div>


      vue对象初始化
      <script>
        // 实例化vue对象（MVVM中的View Model）
        new Vue({
            // vm控制的区块为id为app的div，此div中的所有vue指令均可以被vm解析
            el:'#app',
            data:{
            // 数据 （MVVM中的Model）   
            imageSrc:'http://157.122.54.189:8998/vue/vuebase/chapter1/imgs/d1-11.png',
            isRed:true,
            classA:'A',
            classB:'B',
            isB:true,
            isC:true,
            size:22,
            styleObjectA:{color:'red'},
            styleObjectB:{fontSize:'30px'}
            },
            methods:{

            }
        })
    </script>
```

5. v-for(渲染列表)
``` html
  <!-- 
    v-for用法：
      item in Array
      (item, index) in Array
      value in Object
      (value, key, index) in Object
    :key属性具有唯一性，不能重复，它能够唯一标识数组的每一项
    将来数据中的某一项的值发生了改变，则v-for只会更新当前项对应的这个dom元素的值，而不是更新整个数据，从而提高效率，参考https://www.zhihu.com/question/61064119/answer/183717717

    注意：以下变动不会触发视图更新
      1. 通过索引给数组设置新值
      2. 通过length改变数组
    解决：
      1. Vue.set(arr, index, newValue)
      2. arr.splice(index, 1, newValue)
    -->
```

``` html
    <ul>
      <li v-for="item in user">{{item.name}}</li>
      <li v-for="(item, index) in user" :key="index">{{index}}.{{item.name}}</li>
      <li>---------------华丽的分割线---------------</li>
      <li v-for="value in boss">{{value}}</li>
      <li v-for="(value, key, index) in boss"> {{index}}.{{key}}:{{value}}</li>
    </ul>
    <script>
    var vm = new Vue({
      el: '#app',
      data: {
        user: [
          {name: 'jack'},
          {name: 'neil'},
          {name: 'rose'}
        ],
        boss: {
          name: '马云',
          age: 50,
          money: 1000000002030
        }
      }
    })
    </script>
```   

6. v-show v-if
``` html
  作用:控制元素(组件)的显示状态 当值为true则显示 false不显示

  使用场景:切换消耗较大的时候用v-show 初次渲染消耗较大的时候用v-if

  v-else必须配合v-if使用 他们是排他关系

  v-else-if多个条件

  v-if v-else-if v-else都是排他关系
```

7. v-model
``` html
  1、在表单控件或者组件上创建双向绑定
  2、v-model仅能在如下元素中使用：
    input
    select
    textarea
    components（Vue中的组件）
  3、举例：
    <input type="text" v-model="uname" />
  new Vue({
      data:{
          uname:'' //这个属性值和input元素的值相互一一对应，二者任何一个的改变都会联动的改变对方
        }
  })
```

7. v-on 
``` html
1、作用：绑定事件监听，表达式可以是一个方法的名字或一个内联语句，
  如果没有修饰符也可以省略，用在普通的html元素上时，只能监听 原生
  DOM 事件。用在自定义元素组件上时，也可以监听子组件触发的自定义事件。

  2、常用事件：
      v-on:click
      v-on:keydown
      v-on:keyup
      v-on:mousedown
      v-on:mouseover
      v-on:submit
      ....

  3、示例：
    <!-- 方法处理器 -->
    <button v-on:click="doThis"></button>
    <!-- 内联语句 -->
    <button v-on:click="doThat('hello', $event)"></button>
    <!-- 阻止默认行为，没有表达式 -->
    <form v-on:submit.prevent></form>

   5、v-on的缩写形式：可以使用@替代 v-on:
    <button @click="doThis"></button>

  6、按键修饰符
    触发像keydown这样的按键事件时，可以使用按键修饰符指定按下特殊的键后才触发事件
    写法：
      <input type="text" @keydown.enter="kd1">  当按下回车键时才触发kd1事件

      由于回车键对应的keyCode是13，也可以使用如下替代
      <input type="text" @keydown.13="kd1">  当按下回车键时才触发kd1事件

      但是如果需要按下字母a（对应的keyCode=65）才触发kd1事件，有两种写法：
      1、由于默认不支持a这个按键修饰符，需要Vue.config.keyCodes.a = 65 添加一个对应,所以这种写法为：
      Vue.config.keyCodes.a = 65
      <input type="text" @keydown.a="kd1">  这样即可触发

      2、也可以之间加上a对应的数字65作为按键修饰符
      <input type="text" @keydown.65="kd1">  这样即可触发

      1. 键盘上对应的每个按键可以通过 http://keycode.info/ 获取到当前按下键所对应的数字
      2. 获取 $event(当前的事件)的keyCode

  7.v-on按键修饰符
  1.作用说明: 
      文档地址：https://cn.vuejs.org/v2/guide/events.html#键值修饰符
      在监听键盘事件时，我们经常需要监测常见的键值。 Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：
      .enter
      .tab
      .delete (捕获 “删除” 和 “退格” 键)
      .esc
      .space
      .up
      .down
      .left
      .right
  2.可以自定义按键别名
    在Vue2.0版本中扩展一个f1的按键修饰符写法：
    Vue.config.keyCodes.f1 = 112

    // 使用
    <button @keyup.f1="someFunc"></button>
  3.阻止默认事件
    通过按键修饰符 .prevent 阻止默认事件
```  

### 可以安装插件：快速生成Vue模板
``` html
  npm i nrm -g   //全局安装nrm
  nrm ls  //查看已有的镜像源
  nrm use yarn   //切换镜像源
  nrm use tao
  npm i quickvue2001 -g  //快速生成Vue模板插件

  npm install -g cnpm --registry=https://registry.npm.taobao.org  //设置淘宝镜像源
  cnpm install nrm //利用淘宝源安装nrm

  快速生成Vue模板，使用方式：
  qfv2001 --html fileName-过滤器-study.html --title 过滤器

  npm官方插件及使用方式：
  https://www.npmjs.com/settings/iloved123/packages
```    
### 若出现下面的错误：nrm use taobao
找到第四行找到cli.js17行，改成
``` html
  //const NRMRC = path.join(process.env.HOME, '.nrmrc'); (删除)
  const NRMRC = path.join(process.env[(process.platform == 'win32') ? 'USERPROFILE' : 'HOME'], '.nrmrc');
```   

``` html
C:\Windows\system32>nrm
  internal/validators.js:124
      throw new ERR_INVALID_ARG_TYPE(name, 'string', value);
      ^

  [TypeError [ERR_INVALID_ARG_TYPE]: The "path" argument must be of type string. Received undefined
    at validateString (internal/validators.js:124:11)
    at Object.join (path.js:375:7)
    at Object.<anonymous> (D:\Program Files\nodejs\node_global\node_modules\nrm\cli.js:17:20)
    at Module._compile (internal/modules/cjs/loader.js:1063:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1092:10)
    at Module.load (internal/modules/cjs/loader.js:928:32)
    at Function.Module._load (internal/modules/cjs/loader.js:769:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:72:12)
    at internal/main/run_main_module.js:17:47
  ] {
    code: 'ERR_INVALID_ARG_TYPE'
  }
```
### 若出现下面的错误：qfv2001 --html fileName-过滤器-study.html --title 过滤器
  此系统上禁止运行脚本，说没有权限
  在该命令行执行：get-ExecutionPolicy  
  返回：Restricted
  执行命令（赋予权限）：  Set-ExecutionPolicy -Scope CurrentUser   
  输出：
      位于命令管道位置 1 的 cmdlet Set-ExecutionPolicy
      请为以下参数提供值:
      ExecutionPolicy:
  输入：RemoteSigned
  验证一下：get-ExecutionPolicy 
  输出：RemoteSigned
  到此就有权限了，再次执行上面的：qfv2001 --html fileName-过滤器-study.html --title 过滤器
``` html
  PS D:\StudyDataSource\Vue\Vue\Code\Vueday01> qfv2001 --html fileName-过滤器-study.html --title 过滤器
  qfv2001 : 无法加载文件 D:\Program Files\nodejs\node_global\qfv2001.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 
  中的 about_Execution_Policies。
  所在位置 行:1 字符: 1
  + ~~~~~~~
      + FullyQualifiedErrorId : UnauthorizedAccess
```

## 过滤器  
作用:用于处理模板数据的一些业务逻辑
``` html
  1. 私有过滤器
  可以在 new Vue({filters：{}})中的filters中注册一个私有过滤器
  定义格式：
  new Vue({
    el:'#app',
    filters:{        
        '过滤器名称':function(管道符号|左边对象的值,参数1,参数2,....) {
          return 对管道符号|左边参数的值做处理以后的值
        })    
    }
    });
    Vue2.0 调用过滤器传参写法： {{ msg | 过滤器id('参数1','参数2' ....) }}

  2. 全局过滤器
  定义方式
  可以用全局方法 Vue.filter() 注册一个全局自定义过滤器，它接收两个参数：过滤器 ID 和过滤器函数。过滤器函数以值为参数，返回转换后的值
  定义格式： Vue.filter('过滤器名称', function (管道符号|左边参数的值,其他参数1,其他参数2,....) { return 对管道符号|左边参数的值做处理以后的值 })
  Vue2.0 使用：参数调用时用()，多个参数中间使用逗号分开 {{ msg | 过滤器名称('参数1','参数2' ....) }}
```  

(应用示例)自定义私有过滤器实现日期格式化
``` html
  1、 定义私有的日期格式化过滤器：
  new Vue({
      el:'#app',
      data:{
          time:new Date()
      },
      filters:{
          //定义在 VM中的filters对象中的所有过滤器都是私有过滤器
          datefmt:function(input,splicchar){
              var date = new Date(input);
              var year = date.getFullYear();
              var m = date.getMonth() + 1;
              var d = date.getDate();            
              var fmtStr = year+splicchar+m +splicchar+d;
              return fmtStr; //返回输出结果
          }
      }
  });
  2、使用
  <div id="app">
    {{ time | datefmt('-') }} //Vue2.0传参写法
  </div>
```  

(应用示例)自定义全局过滤器实现日期格式化
``` html
  1、 定义全局的日期格式化过滤器：
  Vue.filter('datefmt',function(input,splicchar){
      var date = new Date(input);
      var year = date.getFullYear();
      var m = date.getMonth() + 1;
      var d = date.getDate();            
      var fmtStr = year+splicchar+m +splicchar+d;
      return fmtStr; //返回输出结果
  });    

  2、使用
      <div id="app">
        {{ time | datefmt('-') }} //Vue2.0传参写法
      </div>

    <script>  
        new Vue({
            el:'#app1',
            data:{
                time:new Date()
            }
        });
    </script>
```  
## 自定义指令  
注册一个全局自定义指令 `v-focus` 指令名字不要使用驼峰  
``` html
  Vue.directive('focus', {
    // 当被绑定的元素插入到 DOM 中时……
    inserted: function (el) {
      // 聚焦元素
      el.focus()
    }
  })
```  

注册局部指令，组件中也接受一个 directives 的选项：
``` html
    directives: {
    focus: {
      // 指令的定义
      inserted: function (el) {
        el.focus()
      }
    }
  }
```

然后可以在模板中任何元素上使用新的 v-focus 属性，如下：
``` html
  <input v-focus>
```
### 钩子函数   
 一个指令定义对象可以提供如下几个钩子函数 (均为可选)：
- bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
- inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
- update：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。   

> 在bind中是无法访问到元素的parentNode的  

指令钩子函数会被传入以下参数：
- el：指令所绑定的元素，可以用来直接操作 DOM 。
- binding:
  一个对象，包含以下属性：
  + name：指令名，不包括 v- 前缀。
  + value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。
  + oldValue：指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。无论值是否改变都可用。
  + expression：字符串形式的指令表达式。例如 v-my-directive="goods" 中，表达式为 "goods"。
  + arg：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"。
  + modifiers：一个包含修饰符的对象。例如：v-my-directive.foo.bar 中，修饰符对象为 { foo: true, bar: true }。 
- vnode：Vue 编译生成的虚拟节点(了解)
- oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。(了解)  

## 动态指令参数：  
指令的参数可以是动态的。例如，在 v-mydirective:[argument]="value" 中，argument 参数可以根据组件实例数据进行更新！这使得自定义指令可以在应用中被灵活使用。

例如想要创建一个自定义指令，用来通过固定布局将元素固定在页面上。我们可以像这样创建一个通过指令值来更新竖直位置像素值的自定义指令：
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Vue-test-03-自定义指令-动态指令.html</title>
  </head>
  <style>
      p,body {
          margin:0;
      }
      p{
          width: 150px;
      }
      #app {
          height: 200px;
          width: 300px;
          overflow-y: scroll;
          border:1px solid #ccc;

      }
    
      p:nth-child(1){
          height: 500px;
      }
      p:nth-child(2){
          color:#ccc;
          font-weight: bold;
      }
  </style>
  <body>
      <div id="app">
          <p>向下滚动页面</p>
          <p v-pin="100">粘在页面顶部100px</p>
        </div>
  </body>

  </html>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
      Vue.directive('pin', {
          bind: function (el, binding, vnode) {
              el.style.position = 'fixed'
              el.style.top = binding.value + 'px'
          }
      })

      const vm = new Vue({
          el: "#app",
          data: {
              msg: "那是我日夜思念,深深爱着的人啊,到底我该如何表达,她会接收我吗"
          }
      })
  </script>
```     

这会把该元素固定在距离页面顶部 100 像素的位置。但如果场景是我们需要把元素固定在左侧而不是顶部又该怎么办呢？这时使用动态参数就可以非常方便地根据每个组件实例来进行更新。  
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Vue-test-03-自定义指令-动态指令.html</title>
  </head>
  <style>
      p,body {
          margin:0;
      }
      p{
          width: 150px;
      }
      #app {
          height: 200px;
          width: 300px;
          overflow-y: scroll;
          border:1px solid #ccc;

      }
    
      p:nth-child(1){
          height: 500px;
      }
      p:nth-child(2){
          color:#ccc;
          font-weight: bold;
      }
  </style>
  <body>
      <div id="app">
  <!--         
          <p>向下滚动页面</p>
          <p v-pin="100">粘在页面顶部100px</p> -->

          <div id="dynamicexample">
              <h3>在这模块里面滚动 ↓</h3>
              <p v-pin:[direction]="100">我被钉在这页纸的左边100像素处。</p>
          </div>
        </div>
  </body>

  </html>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
      // Vue.directive('pin', {
      //     bind: function (el, binding, vnode) {
      //         el.style.position = 'fixed'
      //         el.style.top = binding.value + 'px'
      //     }
      // })

      // const vm = new Vue({
      //     el: "#app",
      //     data: {
      //         msg: "那是我日夜思念,深深爱着的人啊,到底我该如何表达,她会接收我吗"
      //     }
      // })


      Vue.directive('pin', {
          bind: function (el, binding, vnode) {
              el.style.position = 'fixed'
              var s = (binding.arg == 'left' ? 'left' : 'top')
              el.style[s] = binding.value + 'px'
          }
      })

      new Vue({
          el: '#app',
          data: function () {
              return {
              direction: 'left'
              }
          }
      })
  </script>
```

## (进阶)Object.defineProperty()    
用于对属性进行劫持
  用法:Object.defineProperty(obj, prop, descriptor)
  obj 要在其上定义属性的对象。
  prop 要定义或修改的属性的名称。
  descriptor 将被定义或修改的属性描述符。  
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>

  <body>
      <div id="app">{{age}}</div>
  </body>

  </html>
  <script>
      let data = {
          name: "李雷",
          age: 12,
          clothes: {
              color: 'red',
              size: 'small'
          }
      }

      function observe(obj) {
          if (!obj || typeof obj !== 'object') {
              return
          }
          Object.keys(obj).forEach(key => {
              defineReactive(obj, key, obj[key])
          })
      }

      function defineReactive(obj, key, value) {
          observe(value)
          Object.defineProperty(obj, key, {
              enumerable: true,
              configurable: true,
              get: function reactiveGetter() {
                  console.log('get value')
                  return value
              },
              set: function reactiveSetter(newVal) {
                  console.log('change value')
                  value = newVal
                  updateDom('#app', key, value)
              }
          })
      }

      function updateDom(el, key, value) {
          let dom = document.querySelector(el) || ""
          if (!dom) {
              throw new Error("没有dom")
          }
          let childNode = dom.childNodes[0];
          if (!childNode['key']) {
              let regExp = new RegExp("\{\{(" + key + ")\}\}")
              let res = regExp.exec(childNode.nodeValue)
              console.log(res)
              childNode['key'] = res[1];
          }
          if (key === childNode['key'] && data[key]) {
              childNode.nodeValue = value;
          }
      }
      
      observe(data)

      updateDom('#app', 'age', 12)
      // data.name = "李1"
      // data.age = 12
  </script>
```

## computed   
计算属性出现的目的是解决模板中放入过多的逻辑会让模板过重且难以维护的问题；计算属性是基于它们的依赖进行缓存的   
``` html
  // 计算属性是基于它们的响应式依赖进行缓存的。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。
  computed: {
      reversedMessage: function () {
      return this.message.split('').reverse().join('')
    }
  }
  //这也同样意味着下面的计算属性将不再更新，因为 Date.now() 不是响应式依赖：

  computed: {
    now: function () {
      return Date.now()
    }
  }
  //相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。
```  

### 计算属性的 setter    
计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：   
现在再运行 vm.fullName = 'John Doe' 时，setter 会被调用，vm.firstName 和 vm.lastName 也会相应地被更新。
``` html
    computed: {
    fullName: {
      // getter
      get: function () {
        return this.firstName + ' ' + this.lastName
      },
      // setter
      set: function (newValue) {
        var names = newValue.split(' ')
        this.firstName = names[0]
        this.lastName = names[names.length - 1]
      }
    }
  }
```

## watch 侦听器(重要)     
侦听器（watch）用来观察和响应 Vue 实例上的数据变动
``` html
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
```   
注意：在模板业务逻辑较重的时候,使用computed,在需要响应参数实时变化的时候使用watch
## 深度监听
``` html
  var vm = new Vue({
        el: '#app',
        data: {
          user: {
            name: 'jack'
          }
        },
        watch: {
          // 监听对象不能使用下面这种写法，要使用深度监听
          // user(newVal, oldVal) {
          //   console.log('改变了');
          // }

          user: {
            // handler这个函数名字固定
            handler (newval) {
              console.log('改变了');
              console.log(newval.name);
            },
            // deep:true表示深度监听
            deep: true
          }
        }
      })
```
## 组件(重点)   
从代码的角度上考虑就是 将一些可以复用的结构和逻辑封装在一起,在以后的项目中可以直接使用,类似于积木,使用组件可以拼接出一个页面   
组件的特点:
 - 高复用性
 - 低耦合
 ``` html
  1.定义一个全局组件 Vue.component("name",{})
  2.定义局部组件 在全局组件里面再注册组件
 ```     
data 必须是一个函数  
每个组件的都需要有独立的作用域,使用函数可以可以维护一份被返回对象的独立的拷贝,这样组件之间的值就不会混乱污染    
一个组件的 data 选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝    
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>
  <script src="./vue.js"></script>

  <body>
      <div id="app">
          <my-com></my-com>
      </div>
  </body>
  <!--定义模板需要使用template标签包裹起来 并且给上id 模板里面的内容需要用容器包裹起来-->
  <template id="global">
      <div>
          <h1>全局组件</h1>
          <h2>展示信息是{{msg}}</h2>
          <button @click="show">点击</button>
          <!--使用子组件-->
          <my-child></my-child>
      </div>
  </template>

  <!--子组件-->
  <template id="child">
      <div>
          <h5>{{msg}}</h5>
          <ul>
              <li>新闻</li>
              <li>娱乐</li>
              <li>体育</li>
          </ul>
      </div>
  </template>

  </html>
  <script>

      //为什么需要组件 解耦合 可复用性 代码易维护....
      //全局组件 第一个值 定义的名称
      //组件还需要定义模板 模板就是当前组件所展示的内容

      //组件里面还有一个属性 叫components 里面可以注册子组件
      Vue.component("my-com", {
          template: "#global",
          data() {
              return {
                  msg: "全局组件"
              }
          },
          methods: {
              show() {
                  alert(1)
              }
          },
          components: {
              "my-child": {
                  template: "#child",
                  data() {
                      return {
                          msg: "子组件的msg"
                      }
                  }
              }
          }
      })
      let vm = new Vue({
          el: "#app",
          data: {
              msg: "组件"
          }
      })
  </script>
```  
## 组件之间的通信     
1. 父组件→子组件传值  
  在父组件的模板作用域里面,给子组件的 组件标签绑定一个属性 :fa = "msg"  
  再在 子组件里面 定义一个props:[ "fa"]来接收父组件传过来的值  
``` html
   //父组件
  data() {
              return {
                  msg_global: "全局组件",
                  gift: "这是给子组件的礼物" //需要传送的值
              }
          },
  //父组件模板
  <template id="global">
      <div>
          <h1>全局组件</h1>
          <h2>展示信息是{{msg_global}}</h2>
          <button @click="show">点击</button>
          <slot></slot>
          <!--使用子组件-->
          <my-child :fa="msg_global" :resive="gift"></my-child>
      </div>
  </template>
  //子组件接收
  components: {
              "my-child": {
                  template: "#child",
                  data() {
                      return {
                          msg_child: "子组件的msg",
                          data: ""
                      }
                  },
                  props: ["fa","resive"] //接收父组件传来的值
              }
          }
```   
2. 子传父  
- 子传父需要在子组件的模板里面定义一个方法,调用该方法之后,通过 this.$emit("sendmsg","需要传递的参数")    
- 在父组件的模板里面 找到子组件的标签 绑定一个事件名字就是 在$emit里面自定义的那个名字  
- @sendmsg = "父组件里面定义的一个方法"  
- 定义的方法不要用驼峰命名法 不然不生效*  

3. 兄弟组件传值  
``` html
  //兄弟组件之间的传值
  //  const app = new Vue({})
  //  app.test = "测试";
  //通过把需要传送的值挂载在 vue实例上 其它组件可以通过app.属性的方式去获得它
```  
4. Bus总线  
为了提高组件的独立性与重用性，父组件会通过 props 向下传数据给子组件，当子组件有事情要告诉父组件时会通过 $emit 事件告诉父组件。如此确保每个组件都是独立在相对隔离的环境中运行，可以大幅提高组件的维护性。  
4.1 EventBus的简介  
EventBus 又称为事件总线。在Vue中可以使用 EventBus 来作为沟通桥梁的概念，就像是所有组件共用相同的事件中心，可以向该中心注册发送事件或接收事件，所以组件都可以上下平行地通知其他组件，但也就是太方便所以若使用不慎，就会造成难以维护的灾难，因此才需要更完善的Vuex作为状态管理中心，将通知的概念上升到共享状态层次。  
4.2 初始化
``` html
  // event-bus.js
  var EventBus = new Vue()
```  
另外一种方式，可以直接在项目中的 main.js 初始化 EventBus ：   
这种方式初始化的 EventBus 是一个 全局的事件总线
``` html
  // main.js
  Vue.prototype.$EventBus = new Vue()
``` 
4.3 发送事件 eventBus.js  
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>

  <body>
      <div id="app">

          <my-com1></my-com1>
          <my-com2></my-com2>
      </div>
  </body>

  </html>
  <script src="./vue.js"></script>
  <script src="./eventBus.js"></script>
  <script>
      Vue.component('myCom1', {
          data() {
              return {
                  num: 1,
                  deg: 180
              }
          },
          template: `<div>
                  组件1
                  <button @click="send">点击</button>
              </div>`,
          methods: {
              send() {
                  eventBus.$emit("sended", {
                      num: this.num,
                      deg: this.deg
                  })
              }
          }
      })

  </script>
``` 
4.4 接收
``` html
   Vue.component('myCom2', {
        template: '<div>组件2</div>',
        mounted() {
        	//执行myCom1发送的事件
            eventBus.$on("sended", function (input) { //resived
                console.log(input)
            })
        }
    })
```  
4.5 移除事件监听者  
如果想移除事件的监听，可以像下面这样操作：
``` html
  EventBus.$off('sended', {})
```  
4. 在组件上使用 v-model(掌握)  
自定义事件也可以用于创建支持 v-model 的自定义输入组件。记住：  
``` html
  <input v-model="searchText">
``` 
等价于：
``` html
  <input
    v-bind:value="searchText"
    v-on:input="searchText = $event.target.value"
  >
```  
当用在组件上时，v-model 则会这样：
``` html
  <custom-input
    v-bind:value="searchText"
    v-on:input="searchText = $event"
  ></custom-input>
```  
为了让它正常工作，这个组件内的 <input> 必须：  
- 将其 value 特性绑定到一个名叫 value 的 prop 上
- 在其 input 事件被触发时，将新的值通过自定义的 input 事件抛出    
写成代码之后是这样的： 
``` html
  Vue.component('custom-input', {
    props: ['value'],
    template: `
      <input
        v-bind:value="value"
        v-on:input="$emit('input', $event.target.value)"
      >
    `
  })
``` 
现在 v-model 就应该可以在这个组件上完美地工作起来了：  
``` html
  <custom-input v-model="searchText"></custom-input>
```  
### 案例: 
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>

  <body>
      <div id="app">
          <my-input v-model="text"></my-input>
          {{text}}
      </div>
  </body>

  </html>
  <template id="input">
      <input :value="xixi" @input="input1" type="text">
  </template>
  <script src="./vue.js"></script>
  <script>
      //自定义组件上双向数据绑定的时候 会默认的连接一个叫做value的属性 并且接受一个叫做input的事件
      let vm = new Vue({
          el: "#app",
          data: {
              text: "传给子组件的值"
          },
          methods: {
              resive(input) {
                  console.log(input)
                  this.text = input;
              }
          },
          components: {
              "myInput": {
                  model: {
                      event: "haha",
                      prop: "xixi"
                  },
                  template: "#input",
                  props: ['xixi'],
                  methods: {
                      input1($event) {
                          //组件里面要使用v-model 这里必须传递input
                          this.$emit('haha', $event.target.value)
                      }
                  }
              },
          }
      })
  </script>
```

## 1.插槽slot(重点)  
Vue 实现了一套内容分发的 API，将 <slot> 元素作为承载分发内容的出口。   
下面是一个案例:
``` html
  v-slot 也有缩写，即把参数之前的所有内容 (v-slot:) 替换为字符 #。例如 v-slot:header 可以被重写为 #header：
```
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>

  <body>
      <div id="app">
          <my-com>
              <!-- v2.5具名插槽的写法 -->
              <!-- <h1 slot="header">导航栏</h1> -->
              <!-- v2.6传参方式 一定要用template包裹被分发的内容,再使用v-slot进行传递 v-slot的值可以是任意变量,并且也支持结构赋值 -->
              <template slot="header">
                  <h1>v2.6具名插槽</h1>
              </template>
              <template v-slot:main={arr}>
                  <h1>轮播图</h1>
                  <h1>{{arr}}</h1>
                  <ul>
                      <li v-for="item in arr">{{item}}</li>
                  </ul>
              </template>
              <h1 slot="footer">联系方式</h1>
              <!-- 非具名插槽的内容直接分发到默认slot -->
              <template v-slot={val}>
                  <h1>传递的参数是:{{val}}</h1>
                  <h3>default</h3>
              </template>
          </my-com>
      </div>
  </body>

  </html>
  <template id="com">
      <fieldset>
          <legend>slot</legend>
          <div class="header">
              <slot name="header"></slot>
          </div>
          <div class="main">
              <slot name="main" :arr="arr"></slot>
          </div>
          <div class="footer">
              <slot name="footer"></slot>
          </div>
          <slot :val="msg"></slot>
      </fieldset>
  </template>
  <script src="./vue.js"></script>
  <script>
      //slot的定义 分发组件内容
      //slot可分发任意内容
      //具名插槽
      Vue.component("my-com", {
          data() {
              return {
                  arr: ["吃饭", "睡觉", "打豆豆"],
                  msg: "非具名插槽传递参数"
              }
          },
          template: "#com"
      })

      // let obj = {
      //     name: "韩没灭"
      // }

      // const { name } = obj

      // console.log(name);

      const vm = new Vue({
          el: "#app"
      })
  </script>
```
### 1.1 es6模块化语法&断点调试
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>

  <body>
      <div id="app">
          {{msg}}
      </div>
  </body>

  </html>
  <!-- <script src="./vue.js"></script> -->
  <script type="module">
      import "./vue.js"//不支持file协议
      debugger //debugger 调试错误执行到这步自动进入断点调试
      const vm = new Vue({
          el: "#app",
          data: {
              msg: "123"
          }
      })
  </script>
``` 
### 1.2 customLink案例 
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>

  <body>
      <div id="app">
          <custom-link to="http://www.baidu.com">百度</custom-link>
          <custom-link to="http://www.taobao.com">淘宝</custom-link>
          <custom-link to="http://www.1000phone.com">千锋</custom-link>
      </div>
  </body>

  </html>
  <!-- <script src="./vue.js"></script> -->
  <template id="link">
      <div>
          {{to==selected?"✈":""}}
          <a :href="to" @click.prevent="active">
              <slot></slot>
          </a>
      </div>
  </template>
  <script type="module">
      import "./vue.js"//不支持file协议
      // debugger //debugger 调试错误执行到这步自动进入断点调试
      //需求 被选中的项 加上一个符号 ✈
      let bus = new Vue();

      Vue.component("custom-link", {
          template: "#link",
          props: ['to'],
          data() {
              return {
                  selected: "http://www.baidu.com"
              }
          },
          created() {
              const _this = this;
              bus.$on("addfj", function (input) {
                  _this.selected = input
              })
          },
          methods: {
              active() {
                  bus.$emit("addfj", this.to)
              }
          }
      })


      const vm = new Vue({
          el: "#app",
          data: {
              msg: "123"
          }
      })
  </script>
```
## 2.动态组件(重点)
有的时候，在不同组件之间进行动态切换是非常有用的，比如在一个多标签的界面里：    
Home component  
上述内容可以通过 Vue 的 <component> 元素加一个特殊的 is 特性来实现：
``` html
  <!-- 组件会在 `currentTabComponent` 改变时改变 -->
  <component v-bind:is="currentTabComponent"></component>
```
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>

  <body>
      <div id="app">
          <p :is="com"></p>
      </div>
  </body>

  </html>
  <script src="./vue.js"></script>
  <script>
      //is属性可以动态的切换组件
      Vue.component("my-com1", { template: "<h1>组件1</h1>" })
      Vue.component("my-com2", { template: "<h1>组件2</h1>" })
      Vue.component("my-com3", { template: "<h1>组件3</h1>" })

      const vm = new Vue({
          el: "#app",
          data: {
              com: "my-com2"
          }
      })
  </script>
```
在上述示例中，currentTabComponent 可以包括    
已注册组件的名字，或 一个组件的选项对象  
### tab切换案例
``` html
  <!DOCTYPE html>
  <html lang="en">

  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>
  <script src="./vue.js"></script>
  <style>
      .tab-button {
          padding: 6px 10px;
          border-top-left-radius: 3px;
          border-top-right-radius: 3px;
          border: 1px solid #ccc;
          cursor: pointer;
          background: #f0f0f0;
          margin-bottom: -1px;
          margin-right: -1px;
      }

      .tab-button:hover {
          background: #e0e0e0;
      }

      .tab-button.active {
          background: hotpink;
      }

      .tab {
          border: 1px solid #ccc;
          padding: 10px;
      }
      h1 {
          margin:0;
      }
  </style>

  <body>
      <!-- is属性动态切换组件 tab切换案例-->
      <!-- is属性里面写组件的名字 -->
      <!-- is属性所在的标签会替换成组件内容 -->
      <!-- 1.定义一个数据,用于渲染按钮 -->
      <!-- 2.点击按钮添加hotpink背景色 -->
      <!-- 动态组件会替换掉元素的标签 但会继承元素的属性 -->
      <div id="app" class="demo">
          <button @click="selected=item" :class="['tab-button',{active:item==selected}]"
              v-for="item in tabs">{{item}}</button>
          <div class="tab" :is="tab_com"></div>
      </div>
  </body>
  <script>
      Vue.component("tab-email", { template: "<h1>邮箱</h1>" })
      Vue.component("tab-mine", { template: "<h1>个人中心</h1>" })
      Vue.component("tab-home", { template: "<h1>主页</h1>" })
      Vue.component("tab-charge", { template: "<h1>充值中心</h1>" })


      const vm = new Vue({
          el: "#app",
          data: {
              tabs: ["email", "mine", "home", "charge"],
              selected: "home"
          },
          computed: {
              tab_com() {
                  return "tab-" + this.selected
              }
          }
      })
  </script>
  </html>
```
## 3.keep-alive
我们之前曾经在一个多标签的界面中使用 is attribute 来切换不同的组件：
``` html
  <component v-bind:is="currentTabComponent"></component>
```  
当在这些组件之间切换的时候，你有时会想保持这些组件的状态，以避免反复重渲染导致的性能问题。  
使用keep-alive包裹动态组件,可以缓存组件状态
``` html
<keep-alive>
  <div class="content" :is="dynamic"></div>
</keep-alive>
```
#### 钩子函数
###### activated
- 类型：Function
- 详细：  
  被 keep-alive 缓存的组件激活时调用。  
##### deactivated
- 类型：Function
- 详细：
  被 keep-alive 缓存的组件停用时调用。
  ___ 该钩子在服务器端渲染期间不被调用。 ___  

## 4.Ref $root $parent $nextTick() (熟记)
### ref
- 预期：string   
  ref 被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 $refs对象上。如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例：
  ``` html
    <input type="text" v-model="msg" ref="ipt">

  //js
    addfocus() {
                  console.log(this.$refs);
                  this.$refs.ipt.focus()
            }
  ```
  当 v-for 用于元素或组件的时候，引用信息将是包含 DOM 节点或组件实例的数组。   
  ___ 关于 ref 注册时间的重要说明： ___ 因为 ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问它们 - 它们还不存在！$refs 也不是响应式的，因此你不应该试图用它在模板中做数据绑定。

### vm.$root
- 类型：Vue instance
- 只读
- 详细：    
当前组件树的根 Vue 实例。如果当前实例没有父实例，此实例将会是其自己。
### vm.$parent  
可以获取父组件 支持链式编程 如果没有父组件得到的是undefined
``` html
  console.log(this.$parent.$parent.$parent)
```  
## Vue.nextTick
- 参数：  
{Function} [callback]    
{Object} [context]  
- 用法：  
在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。
``` html
   mounted() {
            var h = document.getElementById("pp").innerText
            console.log(h);
            this.msg = "值发生改变了"
            // console.log(h) 只会得到dom修改前的值
            // nextTick用于获取更新之后的dom元素
            // this.$nextTick(function () {
            //     console.log(document.getElementById("pp").innerText);
            // })
            //v2.6 nextTick原理:setTimeout
            setTimeout(function () {
                console.log(document.getElementById("pp").innerText);
            }, 20)
        }
```  
## 5.实例生命周期钩子(重点)    
每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。hook 钩子 回调函数  

## 6.进入/离开 & 列表过渡动画(了解)  
### 单元素/组件的过渡  
Vue 提供了 transition 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡  
- 条件渲染 (使用 v-if)
- 条件展示 (使用 v-show)
- 动态组件
这里是一个典型的例子：  
``` html
  <div id="demo">
  <button v-on:click="show = !show">
    Toggle
  </button>
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
  </div>
  new Vue({
    el: '#demo',
    data: {
      show: true
    }
  })
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }
```  
当插入或删除包含在 transition 组件中的元素时，Vue 将会做以下处理：  
1. 自动嗅探目标元素是否应用了 CSS 过渡或动画，如果是，在恰当的时机添加/删除 CSS 类名。
2. 如果过渡组件提供了 JavaScript 钩子函数，这些钩子函数将在恰当的时机被调用。
3. 如果没有找到 JavaScript 钩子并且也没有检测到 CSS 过渡/动画，DOM 操作 (插入/删除) 在下一帧中立即执行。(注意：此指浏览器逐帧动画机制，和 Vue 的 nextTick 概念不同)
4. v-enter：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
5. v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
6. v-enter-to: 2.1.8版及以上 定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter 被移除)，在过渡/动画完成之后移除。
7. v-leave: 定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
8. v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
9. v-leave-to: 2.1.8版及以上 定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave 被删除)，在过渡/动画完成之后移除。


对于这些在过渡中切换的类名来说，如果你使用一个没有名字的 <transition>，则 v- 是这些类名的默认前缀。如果你使用了 <transition name="my-transition">，那么 v-enter 会替换为 my-transition-enter。   
v-enter-active 和 v-leave-active 可以控制进入/离开过渡的不同的缓和曲线
### css过渡
``` html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
  </head>
  <style>
      .fade-enter,
      .fade-leave-to {
          opacity: 0;
      }

      .fade-enter-active,
      .fade-leave-active {
          transition: all 2s;
      }

      .fade-enter-to,
      .fade-leave {
          opacity: 1;
      }

      .bounce-enter-active {
          animation: bounce-in .5s;
      }

      .bounce-leave-active {
          animation: bounce-in .5s reverse;
      }

      @keyframes bounce-in {
          0% {
              transform: scale(0);
          }

          50% {
              transform: scale(1.5);
          }

          100% {
              transform: scale(1);
          }
      }
  </style>

  <body>
      <div id="app">
          <!-- css过度 -->
          <transition name="fade">
              <h1 v-if="isShow">动画</h1>
          </transition>
          <button @click="isShow=!isShow">点击</button>
          <!-- css 动画 -->
          <transition name="bounce">
              <h1 v-if="isShow2">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris facilisis enim libero, at
                  lacinia diam fermentum id. Pellentesque habitant morbi tristique senectus et netus.</h1>
          </transition>
      </div>
  </body>

  </html>
  <script src="vue.js"></script>
  <script>
      let vm = new Vue({
          el: "#app",
          data: {
              isShow: true,
              isShow2: true,
              show: true
          }
      })
  </script>
```  
### 结合animate.css库使用
``` html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Document</title>
      <!-- cdn方式引入animate.css -->
      <link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">
  </head>
  <style>

  </style>

  <body>
      <div id="app">
          <!-- enter-active-class=""
              leave-active-class=""
              可以自定义类名 -->
          <transition name="custom" enter-active-class="animated rubberBand" leave-active-class="animated jello">
              <p v-if="isShow">
                  Lorem ipsum dolor sit amet consectetur adipisicing elit. Deserunt assumenda maxime quod
                  possimus quos
                  veritatis aut necessitatibus quia excepturi obcaecati.
              </p>
          </transition>
          <button @click="isShow=!isShow">点击</button>
      </div>
  </body>

  </html>
  <script src="./vue.js"></script>
  <script>
      // 6个类名
      // enter enter-active enter-to leave leave-active leave-to 
      let vm = new Vue({
          el: "#app",
          data: {
              isShow: true
          }
      })
  </script>
```

### 1.案例最终(重点)
``` html
    <style>
        #app {
            width: 600px;
            margin: 10px auto;
        }

        .tb {
            border-collapse: collapse;
            width: 100%;
        }

        .tb th {
            background-color: #0094ff;
            color: white;
        }

        .tb td,
        .tb th {
            padding: 5px;
            border: 1px solid black;
            text-align: center;
        }

        .add {
            padding: 5px;
            border: 1px solid black;
            margin-bottom: 10px;
        }
    </style>

    <body>
        <div id="app">
            <div class="add">
                编号:<input type="text" v-model="code">
                品牌名称:<input type="text" v-model="productName">
                <input type="button" value="添加" @click="add">
            </div>

            <div class="add">
                品牌名称:<input type="text" placeholder="请输入搜索条件" v-model="searchText">
            </div>

            <div>
                <table class="tb">
                    <tr>
                        <th>编号</th>
                        <th>品牌名称</th>
                        <th>创立时间</th>
                        <th>操作</th>
                    </tr>
                    <tr v-for="(item,index) in arr">
                        <td>{{item.code}}</td>
                        <td>{{item.productName}}</td>
                        <td>{{item.timer}}</td>
                        <td>
                            <button @click="del(index)">删除</button>
                        </td>
                    </tr>
                    <tr v-show="!arr.length">
                        <td colspan="4">没有品牌数据</td>
                    </tr>
                    <!-- 动态生成内容tr -->
                </table>
            </div>
        </div>
    </body>
    <script src="./vue.js"></script>
    <script>
        //1.渲染数据
        //2.增加数据
        //3.删除数据
        const vm = new Vue({
            el: "#app",
            data: {
                oldArr: [],
                searchText: "",
                code: "",
                productName: "",
                arr: [
                    {
                        id: 1,
                        code: 1,
                        productName: "品如的衣柜",
                        timer: new Date().toLocaleTimeString()
                    },
                    {
                        id: 2,
                        code: 2,
                        productName: "蔡徐坤的篮球",
                        timer: new Date().toLocaleTimeString()
                    },
                    {
                        id: 3,
                        code: 3,
                        productName: "关羽的马",
                        timer: new Date().toLocaleTimeString()
                    },
                ]
            },
            watch: {
                searchText(newVal) {
                    // console.log(newVal)
                    // let str = "蔡徐坤"
                    // console.log(str.indexOf(""));
                    //通过newVal去数组里面检索,挨个遍历,检索每个对象之中的productName属性
                    //返回数组中productName.indexOf(newVal)!==-1
                    this.arr = this.oldArr.filter(item => item.productName.indexOf(newVal) !== -1)
                }

            },
            created() {
                this.oldArr = [...this.arr]
            },
            methods: {
                del(i) {
                let res = this.arr.splice(i, 1)
                    // this.oldArr = this.oldArr.filter(item => item.id !== id)
                    this.oldArr = this.oldArr.filter(item => item.id !== res[0].id)
                    // console.log(res);
                },
                add() {
                    if (!this.code || !this.productName) {
                        alert("编号或者产品名称不能为空")
                        return
                    }

                    const code = this.code;
                    const productName = this.productName;
                    console.log(code, productName)
                    //随机生成id
                    function randomId(num = 4) {
                        if (isNaN(num) || typeof num == "string") {
                            throw new Error("传入的值必须是Number类型")
                        }
                        if (num >= 10) {
                            num = 10
                        }
                        let str = "0123456789"
                        let idArr = [];
                        for (var i = 0; i < num; i++) {
                            let randomIndex = Math.floor(Math.random() * 10)
                            idArr.push(str[randomIndex])
                        }
                        let id = idArr.join("")
                        return id
                    }
                    let id = randomId(9)
                    console.log(id)
                    let obj = {
                        id,
                        code,
                        productName,
                        timer: new Date().toLocaleTimeString()
                    }

                    this.arr.push(obj)
                    this.oldArr.push(obj)
                    //添加完成之后清空输入框的值
                    this.code = ""
                    this.productName = ""
                }
            }
        })
    </script>
```
## 2.Render节点、树以及虚拟 DOM    
虚拟DOM:框架中的概念,利用js提供的api(document.createElement()),创建结构,模拟真实dom的嵌套结构  
虚拟DOM的作用:由于虚拟dom是运行于内存中,并非操作的真实dom,所以在更新的时候可以在内存中计算出最后的结果,再一次的更新到dom上,提高性能  
在深入渲染函数之前，了解一些浏览器的工作原理是很重要的。以下面这段 HTML 为例：  
``` html
    <div> 
        <h1>My title</h1>
        Some text content
        <!-- TODO: Add tagline -->
    </div>
```  
当浏览器读到这些代码时，它会建立一个[“DOM 节点”](https://javascript.info/dom-nodes)树来保持追踪所有内容，如同你会画一张家谱树来追踪家庭成员的发展一样。    
上述 HTML 对应的 DOM 节点树如下图所示：    
每个元素都是一个节点。每段文字也是一个节点。甚至注释也都是节点。一个节点就是页面的一个部分。就像家谱树一样，每个节点都可以有孩子节点 (也就是说每个部分可以包含其它的一些部分)。
##### 虚拟 DOM  
Vue 通过建立一个虚拟 DOM 来追踪自己要如何改变真实 DOM。请仔细看这行代码：  
``` html
    return createElement('h1', "我打篮球贼溜")
```  
createElement 到底会返回什么呢？其实不是一个实际的 DOM 元素。它更准确的名字可能是 createNodeDescription，因为它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点，包括及其子节点的描述信息。我们把这样的节点描述为“虚拟节点 (virtual node)”，也常简写它为“VNode”。“虚拟 DOM”是我们对由 Vue 组件树建立起来的整个 VNode 树的称呼。  
##### createElement 参数    
接下来你需要熟悉的是如何在 createElement 函数中使用模板中的那些功能。这里是 createElement 接受的参数：  
``` html
    // @returns {VNode}
    createElement(
        // {String | Object | Function}
        // 一个 HTML 标签名、组件选项对象，或者
        // resolve 了上述任何一种的一个 async 函数。必填项。
        'div',

        // {Object}
        // 一个与模板中属性对应的数据对象。可选。
        {
            // (详情深入数据对象)
        },

        // {String | Array}
        // 子级虚拟节点 (VNodes)，由 `createElement()` 构建而成，
        // 也可以使用字符串来生成“文本虚拟节点”。可选。
        [
            '先写一些文字',
            createElement('h1', '一则头条'),
            createElement(MyComponent, {
            props: {
                someProp: 'foobar'
            }
            })
        ]
    )
```
##### 深入对象
``` html
    {
        // 与 `v-bind:class` 的 API 相同，
        // 接受一个字符串、对象或字符串和对象组成的数组
        'class': {
            foo: true,
            bar: false
        },
        // 与 `v-bind:style` 的 API 相同，
        // 接受一个字符串、对象，或对象组成的数组
        style: {
            color: 'red',
            fontSize: '14px'
        },
        // 普通的 HTML 特性
        attrs: {
            id: 'foo'
        },
        ........
    }
```
让我们深入一个简单的例子，这个例子里 render 函数很实用。假设我们要生成一个表格,并且让用户通过slot的方式随意的传入tr,但是在H5的渲染中,tr必须放在tobody,普通写法会造成tr和tbody并行的情况,这个时候用render最合适不过  
``` html
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <style>
        td {
            border: 1px solid red;
        }
    </style>
    <body>
        <div id="app">
            <qf-table>
                <qf-table-row></qf-table-row>
                <qf-table-row></qf-table-row>
                <qf-table-row></qf-table-row>
                <qf-table-row></qf-table-row>
                <qf-table-row></qf-table-row>
                <qf-table-row></qf-table-row>
                <qf-table-row></qf-table-row>
            </qf-table>
        </div>
    </body>

    </html>
    <template id="qftr">
        <tr>
            <td>7777</td>
            <td>7777</td>
            <td>7777</td>
            <td>7777</td>
        </tr>
    </template>
    <script src="./vue.js"></script>
    <script>
        Vue.component('qf-table', {
            created() {
                console.log(this)
            },
            render(h) {
                return h('table', {
                    style: {
                        border:'1px solid red'
                    }
                }, [h('tbody', this.$slots.default)])
            }
        })

        Vue.component('qf-table-row', {
            template: "#qftr"
        })

        const vm = new Vue({
            el: "#app"
        })
    </script>
``` 