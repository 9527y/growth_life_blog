---
title: Uni app 页面&组件生命周期
categories:
  - 开发
tags:
  - uniapp
date: 2020-09-18 16:42:12.625000
---
# 页面的生命周期

- onLoad 监听页面加载，其参数为上个页面传递的数据，参数类型为object（用于页面传参），参考示例
- onShow 监听页面显示
- onReady 监听页面初次渲染完成
- onHide 监听页面隐藏
- onUnload 监听页面卸载
- onPullDownRefresh 监听用户下拉动作
- onReachBottom 页面上拉触底事件的处理函数
- onShareAppMessage 用户点击右上角分享 微信小程序
- onPageScroll 监听页面滚动
- onTabItemTap 当前是 tab 页时，点击 tab 时触发。

# 组件的生命周期
- beforeCreate：组件初始化，但数据原生观测、自定义观测(event\watcher)没生成之前。 未完全创建阶段
- created：组件创建后，但还未挂载 完全创建阶段
- beforeMount：组件渲染后，挂载前。 渲染后待挂载
- mounted： 组件挂载到页面 可用 vm.$el 访问 挂载OK
- beforeUpdate： 虚拟 DOM 重新渲染和打补丁之前 再次渲染前
- updated ： 组件 DOM 已经更新 再次渲染后
- activated： keep-alive 组件激活时调用。 当前组件被激活：显示
- deactivated： keep-alive 组件停用时调用。 当前组件隐藏
- beforeDestroy： 实例销毁之前调用。实例仍然完全可用。 销毁前
- destroy： Vue 实例销毁后调用
## 补充：
挂载阶段，先渲染组件，然后挂载组件。
