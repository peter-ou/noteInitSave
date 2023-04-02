[toc]

  

### 基础回顾

  

假设作为一个机智后端开发的你已经了解了如何使用 Maven 去创建一个项目，并且用 JSP 写过前后端不分离的业务系统。

  

在讲解枯燥的技术细节之前，我们不妨先回顾一下基础知识放松一下

  

首先了解一下 Web 网页的加载原理。我们都知道 Web 网站最初是只有 HTML 和 CSS 的，HTML 是一个 Document，由 Body 和 Header 组成。

  

Document 的层次结构又称为 DOM 树。像下面这样：

  

```html

<html>

    <header>

    </header>

    <body>

        <div>aaaa</div>

        <table>

        </table>

        <div>bbb</div>

    </body>

  </html>

  

```

  

HTML 只能作为静态展现，没有交互效果。所以最初网页的跳转是通过链接标签实现的。

  

JavaScript 的出现让动态的修改 DOM 结构成为现实。这样网页就有了新的生命 -> 交互。

  

最早的 JavaScript 代码，是通过 `document.getElementById("id");` 这种方式进行 DOM 元素的选择。选择完 DOM 后通过 DOM 元素本身的 Api 来控制 DOM 的行为的。如果是复杂的网页，那么写一个页面都会非常的耗费时间，即使后面的 jQuery 时代， 其也只不过方便了 DOM 元素的选取。对于一些组件之间的参数传递和全局系统状态维护是没有封装的。

  

现在比较流行的单页面开发方式，比如 React、Vue、Angular 提供了数据和 DOM 元素的双向绑定，视图和数据项分离的开发方式，将之前手工维护全局状态和组件直接数据传递的开发方式改成由框架来维护。大大的简化了开发方式。

  

当打开一个 Vue 项目时，看到那些杂乱的打包配置和路由配置，一个个不认识的标签，又怎么去从整体上认识这些呢。如果没有人去专门讲解这些，对于一个专职的后端开发来讲其实理解起来是很困难的。但是作为一个后端如果对于现在的前端开发一点都不懂，那么实际在开发过程中经常会出现给前端的数据格式不方便，引起前端的吐糟。所以哪怕不去专职做前端，理解和懂得原理也是一个优秀的工程师应该去做的事情，本文不致力于一篇文章让你精通前端开发，旨在提供学习的思路和思考方式。

  

为了方便快速的理解和开发出能看到的效果，可以去下载个后端的脚手架工程，自己开发不需要从头造轮子。[vue-admin-template](https://github.com/PanJiaChen/vue-admin-template) 或者 [Ant Design Pro Vue](https://github.com/sendya/ant-design-pro-vue.git)，这两个是已经搭建好了的后台项目基本模板工程。任何一个都可以，我们以第一个这个作为讲解的基础，这个以 Element-UI 为基础，用的比较多一些。

  

Node.js 环境需要提前搭建好。本文不再讲解。Node.js 本身有一个版本管理工具 [NVM](https://github.com/nvm-sh/nvm)。如果版本有问题可以通过 NVM 来切换 Node.js 的版本。

  

### 后端思维

  

#### 入口

  

假设正在阅读的你对于这个工程一无所知，那么我们从建一个后端项目开始。

  

打开熟悉的 IDEA 新建一个 Maven 项目，起好名字，点击完成。现在最基础的一个文件是 pom.xml，pom.xml 文件是 Maven 管理我们项目依赖的各种组件和 jar 包的。

  

打开刚才下载的模板项目在项目的根目录下有个 package.json 文件。这个文件就相当于 pom.xml 文件，是管理前端项目依赖的组件的，其中 dependencies 是前端项目在运行时依赖的组件。devDependencies 里面的组件是开发时使用的，相当于 Maven dependency 里面的 test。

  

scripts 是什么呢，还是回到后端思维，pom.xml 是给 Maven 用的，Maven 执行 `mvn install clean` 等命令来进行打包部署测试。scripts 是一样的，这个是给 Node.js 用的，Node.js 使用这个命令来进行打包、测试。

  

项目的 readme 里面 `npm run dev` 的 dev 就是对应这个 scripts 里面的 `dev : vue-cli-service serve`。

  

安装新的依赖项命令 `npm install xxx` 会看到 package.json 文件，会更新在 dependencies 节点里面，自动添加了版本和依赖项。这么来看前端比后端还更智能一些。

  

package.json 是项目的入口，跟 pom.xml 一样，通过这个文件我们能知道我们的项目都依赖了哪些插件。这么来看前端和后端除了语法不太一样，思想是一样的。

  

#### 配置

  

##### **环境**

  

后端项目创建完成之后，我们在 resources 下面通常都会创建几个配置文件 application-dev.yml,application-pro.yml 来区分多环境下的变量，如果不是 Spring Boot 项目，那么会在 pom.xml 下创建多个配置，像下面这样：

  

```java

<profiles>

        <profile>

            <id>dev</id>

            <properties>

                <xxx></xxx>

            </properties>

        </profile>

        <profile>

            <id>test</id>

            <properties>

               <xxx></xxx>

            </properties>

        </profile>

</profiles>

```

  

现在的项目前后端分离，也就是说前端可以自己玩了，那么像前端请求地址之类的会跟后端一样，不同的环境有不同的配置。这个配置就是 .env 开头的那几个文件了，跟后端的 application-dev.yml 之类的用途一模一样。

  

##### **风格**

  

说完了多环境，我们再来说说编程规范配置。众所周知，一个优秀的团队一定有相同的编程风格。说到编程规范我们就不得不提一下 Google 的 Java 编程风格，为了使大家都能写出漂亮的代 (ge) 码 (shi)。有一个项目 checkStyle，通过 XML 方式来进行检查，并且有相应的 Maven 插件，可以将代码的格式进行约束。

  

IDEA 支持配置 Google.xml 的文件，并且通过格式化快捷键，自动格式化成 Google 的代码风格。相关的学习资料如下，建议好好看一下。

  

- [https://github.com/checkstyle/checkstyle](https://github.com/checkstyle/checkstyle)

- [https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml](https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml)

- [https://github.com/spring-cloud/spring-cloud-gateway](https://github.com/spring-cloud/spring-cloud-gateway)：这个是 Spring 源代码要求的配置格式教程。希望作为后端的你熟悉这种操作。

  

优秀的东西处处相似，现在前端也有这样的东西，这个文件就是 .eslintrc.js。如果不按照规范，写个前端连编译都过不去了。作为后端是你，是不是也要有些思考呢，如果平时不注意编程习惯，是不是要严谨起来了。如果你用 webstorm 去开发前端的时候，默认是没有这个快捷键格式的，此时需要按照以下操作：

  

> WebStorm -> Preferences -> KeyMap -> 右面搜索输入 ESLint -> 确定选择 Fix Eslint Problem 设置你喜欢的快捷键就可以了。

  

还有一个配置文件 vue.config.js，这个文件非常重要，它可以配置怎么跟后端对接，配置资源地址，我们最后再说。到这里项目目录下的配置文件信息都不必去关注了，对于快速开发一个功能足够了。

  

#### 关注点

  

项目的目录在官方也有介绍，下面是这个链接大致浏览一下即可。

  

> [https://panjiachen.github.io/vue-element-admin-site/zh/guide/#%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84](https://panjiachen.github.io/vue-element-admin-site/zh/guide/#%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84)

  

这里我们只按照后端要关注的东西去梳理这些文件。

  

##### **路由**

  

再次回到后端模式，在不使用前后端分离技术的时候，JSP 页面是怎么跳转的呢。假如我们使用 SpringMvc 作为后端框架。后端会注册一个 Controller，然后在方法上添加 @RequestMapping 注解，方法返回一个 view 的名字，也就是文件名。这样就实现了从一个页面跳转到了另外一个页面。这种页面间的跳转在前端开发就叫路由。路由就是跳转的绑定而已，而且前端的路由不需要任何的业务逻辑操作，所以更加的简单。

  

那么为啥前端需要自己维护一套路由呢，那是因为现在前端是跟后端分离了，不再由后端提供这种全局路由的维护，所以页面跳转的逻辑需要自己维护。这个文件在项目里名字是 router/index.js。典型的配置如下：

  

```js

{

    path: '/',

    component: Layout,

    redirect: '/dashboard',

    children: [{

      path: 'dashboard',

      name: 'Dashboard',

      component: () => import('@/views/dashboard/index'),

      meta: { title: 'Dashboard', icon: 'dashboard' }

    }]

  },

```

  

这里主要看 path 和 component，有 children 的只看 children， 这个 path 就相当于 @RequestMapping 里面的 path 和 component 就相当于 Controller 里面 return 的 "xxx" view 名称，

`component: () => import ('@/views/dashboard/index')` 后面的 import 就相当于后端的 JSP 实际存放路径。

  

了解了这个，你就知道了浏览器上面显示的地址究竟是跳转到哪个页面了。有时候我们还需要路由跳转传参，比如从列表页跳转到详情页，在后端我们可以通过 RedirectAttributes 对象进行传递参数，前端路由通过：

  

```js

this.$router.push({ path: '/user/userDetail', query: { id: id }})

```

  

这种方式传递参数，常用的场景在于 Table 列表页跳转到详情页。

  

看下面的图再加深一下印象：

  

![在这里插入图片描述](https://images.gitbook.cn/b1ad53c0-31a6-11ea-a6f1-c115c5f9d55b)

  

##### **store**

  

现在的前端每天都说些什么状态管理是什么东西呢，什么 Redux、Vuex 这些东西是干啥的。我们再次穿越回后端。在 JSP 页面中我们都知道有几个内置对象，比如 Session Application Request，在这些对象里我们通常会管理跟用户或者跟应用生命周期绑定的数据。比如我们要在一个页面中获取我们应用发布的版本通常放在 Application 里，用户的信息我们会放在 Session 里面，用户退出了数据也就没有了。

  

有些需求会让我们记录下用户什么时间登录的，什么时间退出的，实际上这些跟 Session 的是否存在没有任何的关系，只是我们想记录这个状态的变化。

  

这种维护公共状态的方式在前端开发里面起了高大上的名字就叫状态管理。

  

忽然发现我们后端一直都有这些概念啊。只不过前后端分离之后，在前端起了个新的名字而已。关于 store 相关的东西都放在 store 的文件夹下。

  

打开 store 下面的 modules 目录怎么还分模块啊？ 这个不就相当于 JSP 为了区分不同的组件的生命周期，抽象出来 Session、Request、Application 这几个对象然后更加方便区分和管理。这个 Vuex 为了统一管理还把多个模块放到了一个对象下面：

  

```js

Vue.use(Vuex)

  

const store = new Vuex.Store({

  modules: {

    app,

    settings,

    user

  },

  getters

})

  

export default store

  

const getters = {

  sidebar: state => state.app.sidebar,

  device: state => state.app.device,

  token: state => state.user.token,

  avatar: state => state.user.avatar,

  name: state => state.user.name

}

export default getters

```

  

这样在任何页面我们都能获取到这个统一维护的变量了。因为 Vue.use(Vuex)，相当于注入到了全局。这个就是入口。如果想再添加自己新写的 store，放在这个地方就可以了。

  

在任何的页面只要引入 store 对象就能获取存储在 store 里面的任何数据了。store 在前端主要存储全局的变量，比如一些设置信息和 token。典型用法，可以找到 request.js 这个文件，查看里面获取 token 并放在 header 里面的逻辑。

  

```js

import store from '@/store'

service.interceptors.request.use(

  config => {

    // do something before request is sent

if (store.getters.token) {

  // let each request carry token

  // ['X-Token'] is a custom headers key

  // please modify it according to the actual situation

  config.headers['X-Token'] = getToken()

}

return config

  

  },

  error => {

    // do something with request error

    console.log(error) // for debug

    return Promise.reject(error)

  }

)

```

  

理解了 Vuex 是干什么的，再看官网看看语法就能快速地掌握。

> [https://vuex.vuejs.org/zh/](https://vuex.vuejs.org/zh/)

  

##### **请求**

  

前端想要获取数据还得跟后端一起玩。以前异步请求都使用 Ajax。现在的前端在 Ajax 之上又封装了一个请求的包叫 Axios。这个没有什么高级的地方，只是 API 更加的优雅，提供了更多的回调接口而已。对于后端来说就相当于 Apache 的 HttpClient 升级到比较好用的 OkHttp。这块建议直接看官网 [axios](https://github.com/axios/axios)。当做看 OkHttp 就好了，只是一个语法的区别而已。打开 utils 目录下面的 request.js 有两个方法：

  

```js

service.interceptors.request

```

  

这个方法在会发送请求之前进行拦截。像现在流行的 Jwt 鉴权，需要在 header 里面添加 token，逻辑就放在这里，token 可以从我们上面提到的 store 里面获取。store 里面存 token 的逻辑是在登录完成之后。

  

```js

service.interceptors.response

```

  

response 这里面跟后端息息相关，前端需要在这里做一些统一的异常处理。这也是为啥后端现在要统一定义 Response 结构体的关键所在。

  

常用的返回格式像这样：

  

```java

ResponseResult{

    private int code;

    private String message;

    private Object result;

}

```

  

```js

service.interceptors.response.use(

  response => {

    const res = response.data

    if (res.code !== 20000) {

      Message({

        message: res.message || 'Error',

        type: 'error',

        duration: 5 * 1000

      })

      if (res.code === 50008 || res.code === 50012 || res.code === 50014) {

        // to re-login

        MessageBox.confirm('You have been logged out, you can cancel to stay on this page, or log in again', 'Confirm logout', {

          confirmButtonText: 'Re-Login',

          cancelButtonText: 'Cancel',

          type: 'warning'

        }).then(() => {

          store.dispatch('user/resetToken').then(() => {

            location.reload()

          })

        })

      }

      return Promise.reject(new Error(res.message || 'Error'))

    } else {

      return res

    }

  },

  error => {

    console.log('err' + error) // for debug

    Message({

      message: error.message,

      type: 'error',

      duration: 5 * 1000

    })

    return Promise.reject(error)

  }

)

```

  

如果我们 code 成功和失败统一定义为 1 和 0，在这里前端可以很方便的进行错误提示。在正常的返回里面直接获取 result 即可。如果公司有很多的项目，最好也用一个统一返回体，不然前端开发就真的要骂人了。看到这里是不是觉得把前端的问题考虑的更加周全也是一个优秀的后端应该做的事情。

  

上面的部分就是后台管理模块开发的核心内容了。下面的就是基于模板的展示层了。相当于后端的增删改查。

  

#### 上手开发

  

**依赖管理**

  

如果没有接触过 TS 的语法，看到 Js 里面的 require 和 export 还挺头疼的。作为后端可以这样去理解。现在前端已经像后端一样了，采用模块化。这个 require 就相当于后端的 import。想用什么插件直接 require 就可以了。如果在 Linux 系统下，export 这个命令是用于设置环境变量的，export 之后别人就可以直接使用了，在前端这里也是一样的，一个文件里有多个方法，有的可能是封装的私有方法，不想暴露出去，那么直接 export

公用的方法就可以了，别人依赖只能使用暴露的方法。这样就达到了一个封装的目的。这样 require 可以直接引入需要的方法，后面跟上文件的名字，此外这又解决了方法重名的问题。跟 Java 里面 import 包名 + 文件名是一样的道理。

  

**模板**

  

这部分就是 Vue 的语法知识了，这个需要去看官方的文档或者去慕课网找个免费的 Vue 教程，先理解一下基础，如果没有任何的基础，看代码应该就像看天书。去慕课网看视频开双倍语速，两个小时的视频看一个小时就能看完。看一场电影的时间都用不了就能掌握一门技术的基础。

  

这里主要讲解模板的思想。首先理解模板是什么，做后端开发的都知道 Swing。Swing 提供了界面开发的基础组件。比如 Button、Text、Dialog 之类的基础 UI 组件。Vue 支持这种模板定义，相当于定义了一个规则。

  

按照一定的规则去写，Vue 通过自己的工具包能解释成浏览器能识别的语法。Element UI 是一个组件库，它定义了这样的一些基础组件，所以对使用者来讲开箱即用。Element UI 的官网的每一个组件都是现成的代码，直接复制就能使用。相当于 Swing 组件可以通过一些拖拉就能实现功能是一样的。Swing 组件提供了一些属性的配置和方法回调，Element UI 也是一样的。组件之间又可以通过组合。就像后端通过小组件搭建 UI 界面是一样的。

  

**数据绑定**

  

没用过 Vue 也可能经常听前端说什么双向绑定。这个并不是什么魔法，只不过是 在 Object.defineProperty 做的扩展而已。有兴趣的可以去查查这个方法的用法。一些背后的思想值得我们学习。对于后端开发如果不想深入只需要知道在 Vue 开发中，可以将数据和组件进行绑定，组件的行为变化可以直接改变数据，数据的变化是直接反应到组件上。数据我们可以在 Vue 组件的生命周期中进行动态获取和销毁。这个和 Swing 的开发是一样的。

  

**后端对接**

  

这个对于后端开发是最简单的一环了，前端开发在没有后端开发提供数据的时候，需要使用一些第三方模拟库，如 mock.js 进行模拟开发。如果后端自己写接口然后自己开发，只需要将最开始提到的 vue.config.js 中的 proxy 改下 target 就可以了。这样改了之后就可以开发联调了。

  

```js

proxy: {

      // change xxx-api/login => mock/login

      // detail: https://cli.vuejs.org/config/#devserver-proxy

  

[process.env.VUE_APP_BASE_API]: {

  

​        target: `后端服务地址`,

​        changeOrigin: true,

​        pathRewrite: {

  

['^' + process.env.VUE_APP_BASE_API]: ''

  

​        }

​      }

​    },

  

```

  

vue.config.js 还有一个配置需要注意一下：

  

```

configureWebpack: {

    // provide the app's title in webpack's name field, so that

    // it can be accessed in index.html to inject the correct title.

    name: name,

    resolve: {

      alias: {

        '@': resolve('src')

      }

    }

  },

```

  

这个 alias 相当于给 src 定义了一个别名，在使用 require 的时候用 @ 就是相当于定位到了 src 目录。

  

#### 总结回顾

  

网上的 Vue 教程写法一般都是针对前端的，上来就是一大堆概念，什么打包工具，插件之类的，这些让想学习前端的后端来讲，看起来就很头痛，根本不知道如何下手，本文主要是想帮助后端用自己擅长的思想来理解现在的前端开发。实际上后端开发只需要关注上面提到的几点就可以了，想自己开发个管理后台还是很容易的，知道哪些组件控制哪些东西就可以了。

  

最重要的是知道原理。有的思想剩下的就是熟悉语法规则了，这个就需要实际的练习和模拟了，以下是一些不错的案例，方便大家去模拟学习。练习个几天就可以上手了。

  

集成好的项目：

- [https://gitee.com/smallc/Saber](https://gitee.com/smallc/Saber)

- [https://gitee.com/enilu/web-flash](https://gitee.com/enilu/web-flash)

  

对更炫酷有需求的组件库

  

- [https://coreui.io/vue/](https://coreui.io/vue/)

- [https://vuetifyjs.com/zh-Hans/](https://vuetifyjs.com/zh-Hans/)

  

模板项目

  

- [https://github.com/sendya/ant-design-pro-vue](https://github.com/sendya/ant-design-pro-vue)

- [https://panjiachen.github.io/vue-element-admin-site/zh/guide/#%E5%8A%9F%E8%83%BD](https://panjiachen.github.io/vue-element-admin-site/zh/guide/#%E5%8A%9F%E8%83%BD)

  

国内组件库

  

- [https://www.iviewui.com/](https://www.iviewui.com/)

- [https://element.eleme.io/#/](https://element.eleme.io/#/)

- [https://www.antdv.com/docs/vue/introduce/](https://www.antdv.com/docs/vue/introduce/)

  

### 写在最后

  

用已知的东西去强化记忆未知的东西，能有效的降低抵触的情绪，其实前端没有那么难，思想都是相通的。

  

掌握一门技术，最好先从架构上去了解它，可以先看架构图，再从问题出发，因为先有问题才能解决方案，所以学习的思维主动性很重要。还好 Vue 是国内主导的框架，好多问题用中文提问就能得到答案。如果是一个国外的框架，去 Stack Overflow 发现问题并学习是一个非常好的学习习惯。

  

不要为了学习框架而学习框架，首先要明白我们需要什么，框架只是为了满足我们的需求的，学会分析问题，找出问题的需求点，才是我们需要每天学习的东西，不然框架的层出不穷，会让一些人发出一些声音：“别再更新了，学不动了。”

  

仔细想想我们学习新特性的出发点是什么，当框架的一些特性不能满足我们的需求的时候，试着去提一些 issue，到那时我相信你已经会学习了一个新版本的发布，第一时间看 change log，那才是正确的学习方法。从框架作者的角度，为啥要更新，肯定是有新的问题，这些新的问题去驱动它完善它，这样也才会有一个功能强大的框架。

  
  

----------

本文首发于 GitChat，未经授权不得转载，转载需与 GitChat 联系。