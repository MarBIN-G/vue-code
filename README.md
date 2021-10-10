# vue-cnode

> 学习vue之后不清楚如何在实际项目中使用，恰好最近有在学习node.js，发现cnode社区开放了API，便有了这个cnode社区的demo

## 项目截图

- 主页

![主页](https://github.com/mogu396/myImgStorage/blob/main/%E4%B8%BB%E9%A1%B5.png?raw=true)

---

- 文章详情

![文章详情](https://github.com/mogu396/myImgStorage/blob/main/%E6%96%87%E7%AB%A0%E8%AF%A6%E6%83%85.png?raw=true)

## 技术栈

- vue
- vue Router
- Axios
- ElementUI

ElementUI使用的不多，此外还使用了day.js用作日期处理。

## 项目运行设置

- 克隆到本地

```
git clone https://github.com/mogu396/vue-code.git
```

- 安装依赖

```
npm install
```

- 启动项目

```
npm run serve
```

## 遇到的问题

#### 1.引用ElementUI后，如果想要修改它的默认样式，首先在开发者工具中找到它对应的类名，在style中修改，但这样是全局修改了，如果添加上scoped又不起作用。

- 解决方法：这里我外层用一个div包裹要修改的Element组件，使用>>>修改它的样式（也可以用/deep/）

#### 2.首页文章标题的左侧有一个文章分类，需要根据后台返回的数据来给它添加样式。于是我在v-for中对组件的data属性进行修改来判断需要添加什么class。vue控制台输出：可能引起一个无限循环警告。

![无限循环警告](https://github.com/mogu396/myImgStorage/blob/main/%E6%97%A0%E9%99%90%E5%BE%AA%E7%8E%AF%E8%AD%A6%E5%91%8A.png?raw=true)

- 解决方法：不要这样做。。

#### 3.父组件通过props传递数据给子组件，页面能渲染出来，但是console.log显示不出来，也无法在生命周期钩子或methods之类的地方读取数据。

![props传数据](https://github.com/mogu396/myImgStorage/blob/main/%E5%AD%90%E7%BB%84%E4%BB%B6%E8%AF%BB%E5%8F%96%E4%B8%8D%E4%BA%86%E7%88%B6%E7%BB%84%E4%BB%B6%E9%80%9A%E8%BF%87props%E4%BC%A0%E8%BF%87%E6%9D%A5%E7%9A%84%E6%95%B0%E6%8D%AE.png?raw=true)

- 解决方法：
  - 注意父子组件生命周期的挂载顺序。
  - 请看问题5

#### 4.在用户页面，用户头像已经渲染出来了，但是Vue还是警告这个头像的地址是undefined。

![页面已渲染但仍警告undefined.png](https://github.com/mogu396/myImgStorage/blob/main/%E9%A1%B5%E9%9D%A2%E5%B7%B2%E6%B8%B2%E6%9F%93%E4%BD%86%E4%BB%8D%E8%AD%A6%E5%91%8Aundefined.png?raw=true)

- 原因：初始化用户对象时，在data中创建了一个user空对象，但是在页面渲染前没有检查user是否为空，由于数据是通过ajax从后端获取而来，获取到数据时，生命周期早已走完。
- 解决方法：v-if判断user是否为空，由于空对象转换为布尔值仍为true，所以在给user初始化的时候，可以给它初始化为空串而不是空对象。
- 参考文章：[vue.js数据可以在页面上渲染成功却总是警告提示某个字段“undefined”未定义 - 豫见陈公子 - 博客园 (cnblogs.com)](https://www.cnblogs.com/tnnyang/p/10239889.html)

#### 5.Topic组件有两个子组件TopicReplies和AuthorInfo，在Topic中通过ajax获取文章信息并通过props向它的组件传递'topic'数据。在三个组件中的mounted生命周期尝试打印topic数据。结果Topic、AuthorInfo的topic为undefined，但是TopicReplies的topic有值。

- 原因：
  - 这里应该还是异步调取接口获取数据的问题。mounted已经走完，页面挂载完毕之后数据才返回，控制台输出当然为undefined。
  - 但是为什么父组件Topic和它的子组件AuthorInfo没法获取到数据，但是AuthorInfo的兄弟组件TopicReplies却能获取到？这个问题我仍没找到解决方法。

- 解决方法：
  - 使用v-if判断数据有没有拿到，但是页面会有轻微闪动的感觉，如果ajax长时间拿不到数据，组件会无法显示
  - 用watch监视父组件传过来的数据，有更新再做其他操作

## 结束语

这是我的第一个vue项目，9月底建立仓库，10月份国庆节回家，断断续续做了5天左右才完成。

其实并不难，这个项目甚至都还没有用到vuex，不过对我来说算是个很好的练手项目。

作为第一个vue项目，仍有许多不足之处。

比如：组件通用性不足，没有考虑到复用，只是代码层面的拆分

> 项目部分代码参考了[shuiRong/VueCnodeJS: ⚽️🎉Vue初/中级项目，CnodeJS社区重构。](https://github.com/shuiRong/VueCnodeJS)

**感谢[CNode社区](https://cnodejs.org/)提供APi接口**

**感谢[林水溶](https://github.com/shuiRong)开源了项目**

