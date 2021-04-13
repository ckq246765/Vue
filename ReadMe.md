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