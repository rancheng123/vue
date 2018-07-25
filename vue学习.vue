<template>

    <!--必须有一个外壳-->
    <div
        <!--指令   abc是变量-->



                    <!-- 指令    参数          修饰符            值      -->
                        v-bind   :a          .prevent            =   "abc"


            <!--bind-->
            1. 完整语法
                v-bind:a="abc"
            2. 缩写
                :a="abc"

            2.
                :a="abc"


            <!--开始渲染元素，  条件成立，才显示              渲染开销小  适用频繁地切换-->
            v-show   （ 不支持 <template> 元素，也不支持 v-else）

            <!--开始不渲染元素，条件成立，才渲染元素           渲染开销大  适用不频繁地切换 -->
            v-if="seen"
            v-else-if
            v-else

                {{#if ok}}
                <h1>Yes</h1>
                {{/if}}


            <!-- 数组和对象 都适用 -->        <!--根react 中的key一样-->
            v-for="(item,i) in list"        v-bind:key="item.id"
            v-for="(item,i) of list"
            v-for="(item,i) of even(list)"


            <!--事件-->
            1. 完整语法
                v-on:click="fn"
                v-on:click="fn('hi',$event)"
            2. 缩写
                @click="clickFn.stop"

                        修饰符
                            .stop
                            .prevent
                            .capture


                            .self
                            .once
                            .passive

                            按键修饰符
                                .enter
                                .tab
                                .delete (捕获“删除”和“退格”键)
                                .esc
                                .space
                                .up
                                .down
                                .left
                                .right


            <!--数据双向绑定-->
            v-model="name"

                    <!--这个也是数据双向绑定-->
                    <input
                        v-bind:value="name"
                        v-on:input="name = $event.target.value"
                    >

            <!--只更新一次-->
            v-once

            <!--输出真正的 HTML(有XSS 攻击风险)-->
            v-html

            <!--样式-->
            v-bind:class="{active:isActive}"

            v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"
                多重值
                 v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"





        <!--非指令  abc是字符串-->
            a="abc"
    >
        文本插值
        {{ message }}

        {{reversedMessage()}}


        <!--错误的   vue不支持js语句    start-->
        {{ var a = 1 }}
        {{ if (ok) { return message } }}
        <!--错误的   vue不支持js语句    end-->


        <!--同一节点，v-for 的优先级比 v-if 更高-->
        <li v-for="todo in todos" v-if="!todo.isComplete">
            {{ todo }}
        </li>



        <!--组件实例（小型、独立，可复用）-->

            <!--静态组件-->
            <title-bar>


                <!--分发内容 start-->

                    <!--组装插槽-->
                    <div slot='abc'>
                        <div>1111111111</div>
                    </div>

                    <!--渲染插槽-->

                        <!--具名插槽-->
                        <slot name='abc'></slot>

                        <!--默认插槽-->
                        <slot></slot>


                        <slot>
                            <!--插槽默认内容-->
                            <div style="background: gold">
                                Submit
                            </div>
                        </slot>

                <!--分发内容 end-->

            </title-bar>

            <!--动态组件-->        <!--currentComponent 是组件名字，如titleBar,或组件对象-->
            <component v-bind:is="currentComponent"></component>

            <!-- 失活的组件将会被缓存！-->
            <keep-alive>
                <component v-bind:is="currentTabComponent"></component>
            </keep-alive>


    </div>

    <!--表单-->
    <input v-model.number="age" type="number">

        修饰符
            .number
            .trim



    <!--template 用来管理一批元素-->
    <template>
        <div>1111</div>
        <div>钱夹</div>
    </template>

    <!--key 管理可复用的元素

     Vue 为了高效渲染元素，复用已有元素而不是重新渲染
     如果不想复用已有元素，使用key (给其赋唯一值)

     -->
    <template v-if="loginType === 'username'">
        <input placeholder="Enter your username">
    </template>
    <template v-else>
        <input placeholder="Enter your email address">
    </template>

</template>

<script>

    /*问题：

            jsx

            使用全局组件报错

    */

   /*
        自底向上逐层应用
        只关注视图层


        虚拟 DOM
            Vue 将模板编译成虚拟 DOM 渲染函数


        axios    相当于ajax
        Lodash   相当于underscore

    */

   //ViewModel(视图)
   var vm = new Vue({
       el: '#app',
       data: {
           message: 'Hello Vue!'
       }
   });



   //自定义属性

       //vue实例 变化，视图也变化
       vm.message = 2;

       //data 变化，视图也变化
       data.seen = false;


   //vue自带的实例属性

       //vue自带的实例属性（ 前缀$，与定义属性区分）
       vm.$data
       vm.$el

       //vue自带的实例方法（ 前缀$，与定义属性区分）
       vm.$watch('a', function (newValue, oldValue) {
           // 这个回调将在 `vm.a` 改变后调用
       })



    //组件注册
        //  全局注册组件
        Vue.component('button-counter'/* W3C 规范(字母全小写且必须包含一个连字符)  */, {
            data: function () {
                return {
                    count: 0
                }
            },
            template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
        })



        //index.js 中自动全局注册基础组件   start
        const requireComponent = require.context(
            // 其组件目录的相对路径
            './baseComponents',
            // 是否查询其子目录
            false,
            // 匹配基础组件文件名的正则表达式
            /\w+\.(vue|js)$/
        )
        requireComponent.keys().forEach(fileName => {

            // 获取组件配置
            const componentConfig = requireComponent(fileName)

            // 全局注册组件
            Vue.component(
                componentConfig.default.name,
                // 如果这个组件选项是通过 `export default` 导出的，
                // 那么就会优先使用 `.default`，
                // 否则回退到使用模块的根。
                componentConfig.default || componentConfig
            )
        })
        //index.js 中自动全局注册基础组件   end






    export default {
            //组件命名
            name: 'titleBar',

            //组件没有el选项

            //data 选项必须是一个函数（为了  每个组件实例可以维护一份独立的data拷贝）
            data(){
                return {
                    name: '主页',
                    costProductList: [],
                    logo: require('../gift.jpg')
                }
            },

            //计算属性（有缓存），因为vue模板功能不够强大
            computed: {
                computedData1: function () {
                    if(1){
                        return 5;
                    }
                },
                computedData2: {
                    // getter
                    get: function () {
                        return this.firstName + ' ' + this.lastName
                    },
                    // setter
                    set: function (newValue) {
                        var names = newValue.split(' ')
                        this.firstName = names[0]
                        this.lastName = names[names.length - 1]
                    }
                } ,
            },

            //侦听器
            watch: {
                // 如果 `question` 发生改变，这个函数就会运行
                question: function (newQuestion, oldQuestion) {
                    this.answer = 'Waiting for you to stop typing...'
                    this.debouncedGetAnswer()
                }
            },

            //局部注册组件
            components: {
                titleBar,

                'component-a': ComponentA
            },

            //禁用特性继承
            inheritAttrs: false,

            //注册属性（通过属性接受父作用域的数据）
            props: ['title'],

            //属性值类型验证，错误的话，浏览器报错，提示用户
            props: {
                //1.
                    title: String,
                            Number,
                            Boolean,
                            Array,
                            Object,
                            Function,
                            Date,
                            Symbol,
                //2.
                    propB: [String, Number],
                //3.
                    love: {
                        type: Boolean,
                        required: true,
                        default: false
                    },

                 //4. 自定义验证
                    trueLove: {
                        validator: function (value) {
                            // 这个值必须匹配下列字符串中的一个
                            return 'yes' == value
                        }
                    }
            },


            methods:{
                toLogin(){

                    //切换路由
                    this.$router.push({
                        name: 'login'
                    });
                },
                clickFn(){
                    console.log(111)
                }
            },

            mounted :()=> {
                console.log(this)
            },

            //被创建之后
            created(){

            },

            //更新之后
            updated (){

            },

            //更新之后
            destroyed (){

            },




        //每天都在瞎忙碌，空闲之余，想起了你，远方的你在做什么？



        }


    /*组件通信*/

        /*组件内部  发射事件*/
        <button v-on:click="$emit('enlarge-text', 1)">
            Enlarge text
        </button>

        /*组件实例  监听事件*/
        <Caisujia

                                                 /*$event 是传入的值*/
            v-on:enlarge-text="postFontSize +=    $event"
        >
        </Caisujia>

</script>
