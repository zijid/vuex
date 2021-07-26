Vuex
=
格式
-
```javascript
let store=new Vuex.store({
    state:{
        //保存变量 状态的地方
    },
    mutations:{
        //方法 改变state中的变量用的
    },
    getters:{
        //格式化数据和计算属性差不多
    },
    actions:{
        //异步操作
    },
    modules:{
        //模块
    }
})

//Vue中引用
//使用模块方式引用
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)//需要安装一下

new Vue({
  el: '#app',
  store: store,
})
//使用Vuex
store.state["变量"] //读取状态
store.commit("方法名","可选的传入的参数") //执行mutations里的方法
store.getters["方法名"] //读取格式化好的数据
store.dispatch("异步方法名") //执行异步方法 可以返回Promise执行异步操作可以在执行的地方操作也可以在方法里面操作

//读取模块的状态和普通的读取方式一样,如果模块定义了作用域,使用["模块名/方法或变量名"]
```

state
-
作用是存储变量和Vue中的data很像

```javascript
state:{
    "变量名":"存储的数据"
}
//使用方式
this.$store.state["变量名"]
this.$store.state.变量名
```
getters
-
可以认为是 store 的计算属性,当值变化会重新计算并缓存  
也可以通过getters返回一个函数,使用时不会缓存,每次使用都会重新执行

```javascript
getters:{
    方法名(state){
        return "值"
    },
    方法名(state){
        return (参数){
            //函数体
        }
    }
}
//使用
this.$store.getter["方法名"]
this.$store.getter.方法名

```
mutations
-
改变状态的方法

```javascript
mutations:{
    方法名(state){
        state.变量名
    },
    方法名(state,参数){
        state.变量名=参数
    }
}
//使用
this.$store.commit("方法名")
this.$store.commit("方法名",参数)


//使用下面这种形式会把整个对象都传给参数
this.$store.commit({
    type:"方法名",
    参数属性:"值"
})
```
action
-
在里面执行异步方法

```javascript
action:{
    方法名(context) {//参数是一个context对象
        context.commit('increment')
    },
    方法名({ commit,state }) {//参数是方法和状态也可以获取完整的对象
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                commit('increment')
                resolve()
            }, 1000)
        })
        //返回一个Promise对象可以使用then执行异步操作
        //then可以选择在执行的地方使用或者在方法内使用 
    }
}
//使用
this.$store.dispatch("方法名")
```
module
-
模块

```javascript
let module={
    namespaced: true,
    state:{
        A:"A"
}//1.先定义一个模块对象

//2.在store对象上引用
{
    module:{
        "使用模块名":"定义的模块名这里是module",
        "使用的模块名":{
            namespaced: true,            
            state:{
                A:"A"
            }
        }

        //namespaced: true 命名空间
        //不使用命名空间使用方式就和在store上定义的是一样的,使用命名空间使用方式如下
        this.$store.commit("模块名/方法名")
        this.$store.getters["模块名/方法名"]
}
```
生成器map
-
快速生成Vue对应的方法、计算属性和变量

```javascript
    let mapState = Vuex.mapState;
    let mapGetters = Vuex.mapGetters;
    let mapActions = Vuex.mapActions;
    let mapMutations = Vuex.mapMutations;
    computed: {//设置mapState的两种方式
        ...mapState({//对象的三种设置方式
            使用时名字:state => state.count,//1.返回 state的变量
            名字: 'state变量',//2.使用字符串名字直接使用

            名字(state) {//3.使用方法的方式
                return state.count + 1
            }
        }),
        ...mapState({["state变量"]),//使用数组方式,直接传入state变量，使用时名字和state变量名字一样
        // ...mapState('A', {带命名空间的绑定
        // 	a: state => state.a,
        // 	b: state => state.b
        // }),
    }

    //赋值给Vue的方式也可以是
    computed:mapState({})//这种方式就无法再定义本身的属性了
    
    //各种使用方式都是一样的就不复制了
```