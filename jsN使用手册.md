# jsN使用手册

> 20190321更新，待完善

### 1、环境搭建

``` bash
1.1、先安装light
https://document.lightyy.com/lighting_ref/content/jiao_shou_jia_an_zhuang.html
1.2、安装light插件
https://document.lightyy.com/lighting_ref/content/jiao_shou_jia_cha_jian_an_zhuang.html
1.3、light插件安装和更新指令：
light plugin -a 插件名称
备注：light的插件更新历史，可以在淘宝镜像上查看，不过你只能在上面看更新历史，其他的没有任何信息，写的最详细的地方，就是官网了；
https://npm.taobao.org/browse/keyword/lighting-plugin
```

### 2、jsNative开发所以来的插件以及插件的版本
``` bash
-lighting > 1.4.7 (第一个使用：light -v； 后面三个使用light plugin --list可以插件到)
-lighting-plugin-type-vue >= 1.0.34
-lighting-plugin-jsnative >= 1.0.7
-lighting-plugin-native >= 1.0.15
```

### 3、light指令
``` bash
light -h 你可以看到所有的light 支持的指令
 ```
 
### 4、jsN编译窗口
``` bash
light项目运行默认编译窗口为3000，一旦3000端口被占用，项目就无法运行，也就是同一时间，你只能运行一个jsN项目；配置项全部是插件集成，没有暴露给用户，想要定制，需要改插件源码
 ```
### 5、jsN手机端调试lightView安装地址
 ``` bash
https://app.lightyy.com/appDist/ShowSoloApp?appName=com.hundsun.light.lightIn.appstore
  ```
### 6、jsN模板和事件说明
 ``` bash
- jsN只支持按照自己规定的语法编写的模板，类似于微信小程序，只支持自己定义的模板，或者完全按照light要求编写的模板或者组件；
- jsN模板和vue模板结构基本类似，只是模板语言不完全相同；
- 具体参照如下（这里罗列了所有不支持的情况，我验证了一遍，文档描述准确）：
- https://document.lightyy.com/app_dev_jsn/content/kai_fa_ye_mian_mo_kuai.html
- jsN不支持和支持的事件总结如下，目前
- https://document.lightyy.com/app_dev_jsn/content/JSNative-references/common-event.html  
 ```
 ### 7、使用UI框架
  ``` bash
 目前只支持lighting-ui和按照jsN要求编写的ui组件，lighting-ui安装及使用方式如下
 https://document.lightyy.com/app_dev_jsn/content/shi_yong_UI_kong_jian.html
  ```
 ### 8、连接ajax服务
``` bash
 light提供了ajax方法去请求数据，但是仅仅是ajax, 要实现异步回调需要结合es6的Promise，既创建一个新的Promise对象，里面执行ajax请求，然后在resolve和reject里面添加回调处理
 https://document.lightyy.com/H5_dev/content/kai_fa_Ajax_fu_wu.html
 Light.ajax(options)
 Light.ajax.get(url,params,callback) get请求
 Light.ajax.post(url,params,callback) post请求
 jsN具体用法：
 let getToken = new Promise(function (resolve, reject) {
     Light.ajax.get("接口地址", function (data) {
         resolve(data)
     });
 });
 虽然light集成了ajax请求，但是这个太基础了，我们要想统一处理接口请求，还要重新拿出来封装和定制；
```
  ### 9、jsN路由
``` bash
vue-router是集成在light里面的，在jsN里面使用完全是是两码事了，页面的跳转要在index.html里面根据页面的名称添加view标签，view标签的id值对应页面的名称，添加完新的页面，需要打包运行才能生效；
在index.html里面添加新的视图：

click事件实现路由跳转：
itemClick(src, title) {
   Light.navigate(src, {}, {title:  title})
}
路由拦截在工程的app.js里面，其filter回调函数提供了from, to, next，这里可以统一定制路由拦截内容
```
### 10、jsN生命周期
``` bash
目前测试支持：beforeRouteEnter, beforeRouteLeave, created, beforeMount, mounted; 不支持activated, beforeRouteUpdate, 实在想知道原因，可以问一下light开发，建议不要自己看源码；
```
### 11、使用SDK
``` bash
连接原生API，jsN只是让你能够使用定制的js完成原生页面开发，要完成一个完整的app，依然还是需要按照light的规矩，使用light-sdk和壳子交互（light-sdk是当做插件引入的，和lighting-ui一样），然后打壳子，改入口，该配置，上传，打包；和我们之前开发壳子内嵌h5一样；
light-sdk文档：https://document.lightyy.com/app_dev_jsn/content/shi_yong_duan_SDK.html
app配置文档（文档比较老，打包时遇到问题要问light开发）：
https://document.lightyy.com/app_dev_jsn/content/she_zhi_introduction.html
```
### 12、light图表库
``` bash
目前只支持light-chart（是light结合f2开发的一个图表库），这个插件目前没有文档，怎么用只能看项目示例，12点会给出
https://document.lightyy.com/light_chart_ref/index.html
```
### 13、项目示例：
``` bash
 因为light官网给出的示例太旧，现在已经运行不起来了，去询问light开发，他们给出了一个新的网址：
 https://git01.hundsun.com/light/light-demo-os/tree/master，目前这个网址上也只有一个demo（chart.js）能够运行起来：
```
 
 
### 总结：
 - jsN必须依赖light框架，至于整个体系怎么编译运行了，要去看第2点那几个插件；因为全部是插件集成，所以jsN开发不支持项目构建的定制化；
 - 页面开发依赖light-sdk, lighting-ui, light-chart这些插件, light提供的ajax方法太简单，需要我们重新封装；
 - app打包依照light-app打包方式；
 - 可能是开发和light官方文档维护的不是同一个人，导致官方文档更新不及时，准确性降低，jsN的demo也只有一个能够运行起来，具体开发遇到问题需要完全依赖light开发解决，使用框架的人完全被light模式束缚了；
 - light-chart可能因为没解决canvas位图在不同分辨率上失真的问题，导致图表失真，要等开发者修复；
 - 因为开发中遇到问题需要完全依赖light解决，沟通成本非常大；其次，高度的拔插组件致人类存在的意义降低，隐藏的坑可能需要我们完全的开发完一个项目之后，会慢慢暴露出来；任重道远，如果我们需要原生开发，可以考虑使用社区强大的reactNative；
