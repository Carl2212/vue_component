# d1_pull_to_refresh

> A demo for pulling to refresh

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).

#解读iscroll-probe
iscroll-probe 最重要的几个架构函数 ：
1）HandleEvent ：  操作触发事件行为（行为分化，归类行为并且触发，this._execEvent('beforeScrollStart')，this._execEvent('scrollStart') ，this._execEvent('scrollEnd')....）
2）this.on  :  绑定事件（对应行为 绑定对应事件 也就是我们看到的效果的实现）
3)_execEvent : 执行事件 ，查找用户绑定到对应事件的事件 进行执行操作

# 解读下拉时  眼睛看到的与实际发生的
下拉时 两个样式发生变化
1）由iscroll-probe控制的下拉移动
2）自己设置的margin-top
所以 当实际下拉 this.scroll.y = 10 时 其实pull-down-box 是相对它本身原始移动20
下拉距离 + margin-top = pull-down-box实际下移距离
所以 this.scroll.y = 50 时 实际上 pull-down-box 已经完全显示出来 此时释放要触发 pull-down-box停住loading 刷数据操作

#解读上拉时 pull-up-box 可以停住在页面底部的实现
上拉时 pull-up-box 由隐藏变为显示 但是此时的iscroll-probe 的滚动高度是没有变化的 也就是说释放时 pull-up-box依然会被滚动到页面下面，即看不到，停不住
所以 必须在 pull-up-box 状态变为显示 时 使用 this.scroll.refresh()方法 手动刷新页面滚动高度 此时释放 pull-up-box会在页面底部 因为释放时iscroll-probe会控制页面下滑到此时的滚动高度最底部

#使用滚动条时 scrollbars: 'custom',自定义滚动条样式 假设为true 默认系统样式

