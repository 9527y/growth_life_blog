---
title: uniapp 使用 i18n 实现多语言切换
categories:
  - 开发
tags:
  - uniapp
date: 2020-06-12 15:47:21.572000
---
[TOC]
## 主题思想

使用`vue-i18n`实现国际化。[官方文档](http://kazupon.github.io/vue-i18n/zh/installation.html)

## npm 安装
```shell
npm install vue-i18n --save
```
在 main.js 中引入
```javascript
import Vue from 'vue';
import App from './App';
import VueI18n from 'vue-i18n'  // ★
import messages from './commom/lang.js' // ★
Vue.use(VueI18n)  // ★
Vue.config.productionTip = false;

const i18n = new VueI18n({       // ★
  locale: 'zh-CN',  // 默认选择的语言
  messages 
})  
App.mpType = 'app';
Vue.prototype._i18n = i18n  	// ★
const app = new Vue({
	i18n,			// ★
    ...App
});
app.$mount();
```
其中改变`locale`的取值可以改变语言的类型，`messages`的内容我放到一个独立的`lang.js`文件，便于维护，其中的内容如下：
```javascript
export default {
	'en-US': {
		lang: 'en',
		loading: 'loading...',
		index: {
			navTitle: 'Face TV',
			more: 'more'
		},
		content: {
			derector: 'derector',
			protagonist: 'protagonist'
		},
		mine: {
			login: 'login',
			myCollection: 'My favorite'
		}
	},
	'zh-CN': {
		lang: 'zh',
		loading: '加载中...',
		index: {
			navTitle: '扫扫看电视',
			more: '更多'
		},
		content: {
			derector: '导演',
			protagonist: '主演'
		},
		mine: {
			login: '登录',
			myCollection: '我的收藏'
		}
	}
}
```
## 在需要的页面引用这些变量

- 在`index.vue`页面需要更改导航栏标题
```
uni.setNavigationBarTitle({
	title: this.$t('index.navTitle')
})
```

- 标签中绑定

```
<view class="cartu-more">{{$t('index.more')}}</view>
```
## 检测系统语言并改变 locale 值

在`app.vue` 的`onLaunch` 中使用 `uni.getSystemInfoSync()`

```
var lan = 'zh'
 try {
	const res = uni.getSystemInfoSync();
	lan = res.language
} catch (e) {
	console.log('error='+e)
}
console.log('lan='+lan); 
 if(lan == 'en') {
	 this._i18n.locale = 'en-US'
 }
 if(lan == 'zh-Hans-CN' || lan=='zh') {
	 this._i18n.locale = 'zh-CN'
 }

```