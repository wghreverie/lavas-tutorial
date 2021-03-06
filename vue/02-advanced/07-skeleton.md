# App Skeleton 介绍

在页面渲染过程中，首先让用户看到页面的大致框架，感知到页面正在加载，然后看到页面元素被逐渐渲染出来，体验明显优于面对长时间的白屏。这里页面中的这个 “大致框架” 就是 skeleton，起到占位作用，随后会被前端最终渲染完毕的页面元素替换掉。

[flipkart 就采用了这样的做法](http://tech-blog.flipkart.net/2016/03/)，虽然此时用户还无法正常与顶部导航栏进行交互，但不至于面对白屏迅速失去耐心。

![flipkart 中的 skeleton](./images/flipkart-skeleton.png)

受 [Ele.me 升级至 PWA](https://medium.com/elemefe/upgrading-ele-me-to-progressive-web-app-2a446832e509) 一文启发，我们在构建时使用 Vue 的服务端渲染功能，将一个固定的 skeleton 路由组件的渲染结果插入 html 模版中。这样 service worker 对 html 进行缓存后，skeleton 展示速度很快。

## 开发调试

由于使用了 [Webpack 插件](https://github.com/lavas-project/vue-skeleton-webpack-plugin)，在开发环境中，skeleton 对应的路由组件位于 `pages/Skeleton.vue`，通过 `/skeleton` 路由即可访问。只有在开发环境中才会向 `src/router.js` 文件中插入这个路由规则，所以不用担心在生产环境中被用户访问到。

![模版项目展示的 skeleton](./images/lavas-skeleton.png)

有两点需要注意：

1. 由于 skeleton 中包含的代码和样式都会内联在最终的 html 中，所以应该尽可能保持 skeleton 结构和样式的精简。如无必要，尽量不要在 skeleton 中使用 vuetify 的组件，这会导致输出大量样式规则。

2. Skeleton.vue 中已经引入了 [normalize.css](https://github.com/necolas/normalize.css/)，这就意味着在全局样式，或者其他组件中都不需要重复引入了。

## 后续改进

在 [Lavas Basic 模版](https://github.com/lavas-project/lavas-template-vue-basic)和 [Lavas App Shell 模版](https://github.com/lavas-project/lavas-template-vue-appshell)中，构建时生成的 skeleton 只有一个。
对于一个简单应用尚可接受，但是对于一个复杂应用，这样做的缺点是很明显的，当多个页面差异很大，例如搜索页可能并不需要顶部的导航条，而 skeleton 却统一渲染了出来，又或者页面的占位图差别巨大，用户会明显感知到这种差异。

使用多页应用架构能够解决这个问题，因为每个页面有各自对应的 skeleton。我们的 [Lavas MPA 模版](https://github.com/lavas-project/lavas-template-vue-mpa)就采用了这种方式。

另外，在使用服务端渲染时，由于服务端返回的 html 已经包含了首屏的页面结构，并不需要客户端渲染首屏内容，此时 skeleton 就没有存在的必要了，可参考我们的 [Lavas SSR 模版](https://github.com/lavas-project/lavas-template-vue-ssr)。

