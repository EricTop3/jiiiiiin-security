# jiiiiiin-security

一个前后端分离的内管基础项目

+ [MyBatis-Plus优秀案例之一](https://mp.baomidou.com/guide/#%E4%BC%98%E7%A7%80%E6%A1%88%E4%BE%8B)

# 原则

+ 以最少的表结构字段完成一个基础应用，以便在以此完成实际项目时有更多的扩充自由

# 快速开始

+ [导入数据【sql-mysql.sql】](https://github.com/Jiiiiiin/jiiiiiin-security/blob/master/db/sql-mysql.sql)
+ [启动redis，配置查看【application.yml#redis】](https://github.com/Jiiiiiin/jiiiiiin-security/blob/master/jiiiiiin-server-manager/src/main/resources/application.yml#L28)
+ 启动后端内管应用
+ 进入前端内管应用：jiiiiiin-client-manager
    - 安装项目依赖：`npm i`
    - 配置服务端uri：`.env.development`文件下：`VUE_APP_SEVER_URL=http://192.168.1.123:9000`
    - 启动前端内管应用：`npm run serve`

# 计划
| 功能 | 完成状态 | 简介 |
| ------ | ------ | ------ |
| 代码自动生成 | 100% | [服务端3层代码自动生成](https://github.com/Jiiiiiin/jiiiiiin-security/blob/master/jiiiiiin-module-common/src/main/java/cn/jiiiiiin/module/common/generator/CodeGenerator.java) |
| RBAC后端权限控制 | 100% | [基于Spring Security的后端RBAC权限控制](https://github.com/Jiiiiiin/jiiiiiin-security/tree/master/jiiiiiin-security-authorize) |
| RBAC前端权限控制 | 90% | [1.基于vue-viewplus，实现了一个自定义模块](http://jiiiiiin.cn/vue-viewplus/#/global_api?id=mixin-) <br> [2.实现前端页面可访问性控制，通过路由拦截，判断用户待访问页面是否已经授权](https://github.com/Jiiiiiin/jiiiiiin-security/blob/master/jiiiiiin-client-manager/src/plugin/vue-viewplus/rbac.js#L250) <br> [3.实现可见页面的局部UI组件的**可使用性或可见性**控制，基于自定义`v-access`指令，对比声明的接口或资源别是否已经授权](https://github.com/Jiiiiiin/jiiiiiin-security/blob/master/jiiiiiin-client-manager/src/plugin/vue-viewplus/rbac.js#L124)|
| 全面集成vue-viewplus | 70% | [vue-viewplus一个简化Vue应用开发的工具库](https://github.com/Jiiiiiin/vue-viewplus) |
| 会话并发控制 | 100% | [使用SpringSecurity#concurrency-control实现应用中同一用户在同时只能有一个是终端（渠道）成功登录应用，后登录终端会导致前一个会话失效](https://github.com/Jiiiiiin/jiiiiiin-security/blob/master/jiiiiiin-security-browser/src/main/java/cn/jiiiiiin/security/browser/config/BrowserSpringSecurityBaseConfig.java#L123) |
| 会话集群共享 | 100% | [使用Spring Session与Redis实现会话的共享存储和集群部署](https://github.com/Jiiiiiin/jiiiiiin-security/blob/master/jiiiiiin-server-manager/src/main/resources/application.yml#L20) |
| d2-mng-page | 100% | [自定义管理页面组件（统一管理：分页、检索、table、编辑）,为了统一审美](https://github.com/Jiiiiiin/jiiiiiin-security/blob/master/jiiiiiin-client-manager/src/components/d2-mng-page/index.vue) |
| 角色管理 | 95% | 用来管理系统定义的角色 |
| 资源管理 | 95% | 用来管理系统定义的资源 |
| 用户管理 | 95% | 用来管理系统存在的用户 |
| 接口管理 | 95% | 用来管理后台对应的接口集合 |

# 功能截图

![image-20181106162706851](https://ws3.sinaimg.cn/large/006tNbRwgy1fwyf81a19lj31kw0w0awb.jpg)

![](https://ws4.sinaimg.cn/large/006tNbRwgy1fxy9om3ct3j31hc0u0dpu.jpg)

用户管理页面截图

![](https://ws3.sinaimg.cn/large/006tNbRwgy1fxw90anl1yj31c00u0taw.jpg)

角色管理页面截图

![](https://ws3.sinaimg.cn/large/006tNbRwgy1fyazwdryopj31hc0u0qa5.jpg)

资源管理页面截图

![](https://ws2.sinaimg.cn/large/006tNbRwgy1fy2tx2neg1j31hc0u0q9b.jpg)

![](https://ws4.sinaimg.cn/large/006tNbRwgy1fy7ui2yu4cj31c00u0438.jpg)

接口管理页面截图


# 表结构和权限说明

| 表名称 | 简介 | 
| ------ | ------ |
| mng_admin |【用户表】，使用`channel`字段可以区分不同业务系统的用户，如这里`0`标识内管 |
| mng_role |【角色表】，使用`channel`字段可以区分不同业务系统的角色，如这里`0`标识内管 |
| mng_role_admin |【角色用户关联表】，这套系统中，角色和用户是可以多对多配置的 |
| mng_resource |【权限资源表】，使用`channel`字段可以区分不同业务系统的资源，如这里`0`标识内管，另外`type`用来标识资源的类型，在这里只有`类型: 1:菜单(默认) 0:按钮`两种类型，并且没有直接定义一个类`url`字段取标识资源记录对应的后端某一个接口，是因为如一个菜单点击之后到达的页面可能要发多个后台交易，故一个字段来标识这多个交易容易导致混乱，所以在传统的RBAC表结构之下新增了`mng_interface`【系统接口表】来定义，而资源则是给业务人员配置角色或菜单时使用；这张表还有一个特点是将前端Vue Router的页面路径以`path`字段标识，以方便前后端的权限管理 |
| mng_role_resource |【角色资源关联表】，用户、角色、资源都可以由业务人员进行关联操作 |
| mng_interface |【系统接口表】，使用`channel`字段可以区分不同业务系统的接口，如这里`0`标识内管，使用`url+method`来区分后台的某一个接口 |
| mng_resource_interface | 【资源接口关联表】，`mng_resource`和`mng_interface`是多对多关系，因为某一个接口，比如`查询资源树`接口在角色管理列表和资源管理列表两个页面都会被调用，且存在一个资源记录会调用多个接口，故我觉得这样来设计表机构，多加这一张表，才能更清晰的将意思表达到位，且方便维护 |
| persistent_logins | [spring security 记住用户所涉及表](https://docs.spring.io/spring-security/site/docs/3.0.x/reference/remember-me.html)|
| springsocial_UserConnection | [spring social 第三方授权信息关联表](https://docs.spring.io/spring-social/docs/2.0.0.M4/reference/htmlsingle/#section_jdbcConnectionFactory)|

+ 关于前端`rbac`权限控制

    - [Vue 前端应用实现RBAC权限控制的一种方式](https://juejin.im/post/5c19a282f265da61137f372c)

    - 配置：
    
    ```js
      import router from './router'
              import ViewPlus from 'vue-viewplus'
              import rbacModule from '@/plugin/vue-viewplus/rbac.js'
            
              ViewPlus.mixin(Vue, rbacModule, {
                // 是否为调试模式
                debug: true,
                errorHandler(err) {
                  // 当`vue-viewplus`捕获到插件中抛出异常会回调这个全局错误处理函数
                  console.error(err)
                },
                // 当前`rbacModule`作为`vue-viewplus`的自定义混合模块，这里取一个名字，以便观察注入插件情况
                moduleName: '自定义RBAC',
                router,
                // [*] 系统公共路由path路径集合，即可以让任何人访问的页面路径
                publicPaths: ['/login'],
                // 权限检查失败时被回调
                onLoginStateCheckFail(to, from, next) {
                  this.dialog(`您无权访问【${to.path}】页面`)
                    .then(() => {
                      // 防止用户被踢出之后，被权限拦截导致访问不了任何页面，故这里进行登录状态监测
                      if (this.isLogin()) {
                        next(false);
                      } else {
                        next('/login');
                      }
                    })
                }
              })
    ```
    
    详细配置和解释可以点击：[vue-viewplus-自定义RBAC权限控制模块](http://jiiiiiin.cn/vue-viewplus/#/rbac)

# 所用技术栈

### 后台
    
+ [springboot](https://github.com/spring-projects/spring-boot)

+ [spring security](https://github.com/spring-projects/spring-security)

+ [mybatis-plus](https://github.com/baomidou/mybatis-plus)

### 前端    
    
+ [vue](https://github.com/vuejs/vue)

+ [ElemeFE/element](https://github.com/ElemeFE/element)

+ [vue-viewplus](https://github.com/Jiiiiiin/vue-viewplus)

+ [d2-admin](https://gi]thub.com/d2-projects/d2-admin)

<a href="https://github.com/d2-projects/d2-admin" target="_blank"><img src="https://raw.githubusercontent.com/FairyEver/d2-admin/master/doc/image/d2-admin@2x.png" width="200"></a>

  
  
