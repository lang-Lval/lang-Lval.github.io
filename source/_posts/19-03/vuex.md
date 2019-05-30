---
title: vuex的相关使用
categories: vue
date: 2019-03-01 16:13:25
---

![avatar](https://raw.githubusercontent.com/langhuonan/wechat/master/mdImages/day1/1.jpg)
### 引入
单独建一个store文件夹，用来管理store的状态
```
import Vue from 'vue'
import Vuex from 'vuex'
import 'es6-promise/auto'
import axios from 'axios'
Vue.use(Vuex)
```
### 核心概念
<!--more-->
1. store（仓库）：“store”基本上就是一个容器，它包含着你的应用中大部分的状态 (state)
store 中的状态不能直接修改，改变 store 中的状态的唯一途径就是显式地提交 (commit) 
```
const state = {
  uuid: '',
  user: {},
  qrCode:'',
  qrCodeUser:{},
  prizeQrCode:'',
  prizeQrCodeUser:{},
  errorMsg:'',
}
$store.commit('setQrCode','e1dddec9')//setQrCode是要修改的state内的状态，e1dddec9是修改为的数据，可以是一个对象
```
2.getter（可以认为是 store 的计算属性）：就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算
Getter 接受 state 作为其第一个参数
```
const getters = {
  // 将数据从cache中取出
  getCache: function (state) {
    return function (key) {
      var cache_data = localStorage.getItem(key)
      if (!cache_data) {
        return null
      }
      // var data = JSON.parse(Base64.decode(cache_data))
      var data = JSON.parse(cache_data)
      if (data.timeout === 0 || data.timeout > (new Date()).getTime()) {
        return data.data
      }
      localStorage.removeItem(key)
      return null
    }
  },
  getUUID: function () {
    return state.uuid
  },
  getAdminUserInfo: function () {
    return state.user
  },
  getQrCode:function () {
    return state.qrCode
  },
  getQrCodeUserInfo:function () {
	  return state.qrCodeUser
  },
	getPrizeQrCode:function () {
		return state.prizeQrCode
	},
	getPrizeQrCodeUserInfo:function () {
		return state.prizeQrCodeUser
	},
  getErrMsg:function () {
	  return state.errorMsg
  }
}
```
在任意组件的计算属性中可以获取getters内函数的返回值
如：获取上面计算属性中的UID
```
computed:{
			uid:function () {
				return this.$store.getters['getUUID'];
			},
```

3.Mutation
更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数
```
//第一个参数为state状态对象，第二个参数Payload载荷（也就是要修改为的数据）
const mutations = {
  // 将数据存储到cache中
  setCache: function (state, data) {
    if (data.timeout === undefined) {
      data.timeout = 30 * 24 * 3600 * 1000
    }
    var obj = {
      data: data.value,
      timeout: data.timeout + (new Date()).getTime()
    }
    // var cache = Base64.encode(JSON.stringify(obj));
    var cache = JSON.stringify(obj)
    localStorage.setItem(data.key, cache)
  },
  clearCache: function (state, key) {
    localStorage.removeItem(key)
  },
  setAdminUserInfo (state, data) {
    state.user = data
  },
  setQrCodeUserInfo(state,data){
  	state.qrCodeUser = data;
  },
  setQrCode(state,data){
  	state.qrCode = data;
  },
	setPrizeQrCodeUserInfo(state,data){
		state.prizeQrCodeUser = data;
	},
	setPrizeQrCode(state,data){
		state.prizeQrCode = data;
	},
  setErrMsg(state,data){
  	state.errorMsg = data
  }
}
```
在组件中提交 Mutation（提交载荷）
在组件中使用 this.$store.commit('xxx') 提交 mutation
如：
```
  getQrcodeInfo:function (code) {
            	let self = this;
				this.$http({
					methods:'get',
					url:'/mfw/xiaoliwu/backend/Login/showCodeInfo',
					params:{
						uid:this.uid,
						code:code
					}
				}).then(res=>{
					console.log(res)
					if(res.data.code === 0){
						self.$store.commit('setQrCodeUserInfo',res.data.data)//此处就是提交上面mutation内的setQrCodeUserInfo函数，这个函数的第二个参数就是上面的data
						this.$router.push({
							path:'/convert'
						})
					}else {
						self.$vux.alert.show({
							title: '',
							content: res.data.msg,
							buttonText:'知道了'
						})
					}
				})
			},
```
4. Action
Action 类似于 mutation，不同在于：

Action 提交的是 mutation，而不是直接变更状态。
Action 可以包含任意异步操作。

5. Module
由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿

为了解决以上问题，Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：
```
const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```
模块的局部状态
对于模块内部的 mutation 和 getter，接收的第一个参数是模块的局部状态对象。
```
const moduleA = {
  state: { count: 0 },
  mutations: {
    increment (state) {
      // 这里的 `state` 对象是模块的局部状态
      state.count++
    }
  },

  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```
同样，对于模块内部的 action，局部状态通过 context.state 暴露出来，根节点状态则为 context.rootState：
```
const moduleA = {
  // ...
  actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```
对于模块内部的 getter，根节点状态会作为第三个参数暴露出来：
```
const moduleA = {
  // ...
  getters: {
    sumWithRootCount (state, getters, rootState) {
      return state.count + rootState.count
    }
  }
}
```
### 导出
将上面的以常量形式声明的各个核心导出，就可以在任意组件中调用了
```
export default new Vuex.Store({
  state, getters, mutations, actions
})

```