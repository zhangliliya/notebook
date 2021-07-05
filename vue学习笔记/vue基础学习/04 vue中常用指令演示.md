# 常用指令演示

## 1、==v-text==和==v-html==

## 2、条件渲染指令==v-if==和    ==v-else-if== 和==v-else==还有==v-show==

当我们使用v-if、v-else-if、v-else时，假使v-if为false那么此元素就不生成，相当于不生成元素。

当我们使用v-show的时候，v-show的值为false的时候，元素已经生成，只是style中的display属性变成none。

两者之间的区别：一个直接让元素消失，另一个直接改变元素的display属性值。

当元素被频繁切换显示/隐藏的时候使用v-show；当元素显示/隐藏不频繁的时候使用v-if。

## 3、指令之==v-bind==

v-bind的是单向数据绑定，可以简写为：

## 4、指令之==v-on==事件绑定

v-on是对事件进行监听的绑定，可以简写为@，所有的js点击事件都可以使用v-on进行绑定。

1、v-on提供了一个事件修饰符，触发事件后我们不仅是要得到一个纯粹的数据处理逻辑，同时还需要做一些常见的DOM处理细节。

用的最多的事件修饰符：.stop，.prevent，.once（只执行一次）。

2、还提供了按键修饰符

3、为什么要在html中监听事件呢？

- 扫一眼html模板便能轻松定位在javascript代码里面对应的方法
- 因为你无须在javascript里动手绑定事件，你的viewModel代码是可以是非常纯粹的逻辑，和DOM完全解耦，更易于测试。
- 当一个viewModel被销毁时，所有的事件处理器都会自动被删除。你无须担心如何清理它们

## 5、指令之== v-for ==列表渲染

v-for不仅能遍历数组，而且还能遍历对象。这个时候需要对应使用key进行就地更新，这样当数组或者对象中有某一项数据进行改变的时候就只会对当前改变的数组进行视图更新，否则就会所有数据进行视图更新。

## 6、指令之 ==v-model==双项数据绑定

- 在input表单控件中的双向数据绑定， `<input type="text" v-model="msg">`
- 在textarea表单空间中的双向数据绑定，

## 7、监听器==watch==

对单个变量的监视，比较简单：

```js
 'msg':function(newV,oldV){
                        // console.log(newV,oldV);
                        // key是属于data对象的属性名 value：监视后的行为 newV:新值 oldV是旧值
                        if(newV==='100'){
                            console.log('hello');
                        }
                    },
```

对复杂对象的监视，需要进行深度监视：

```js
// 对于复杂的对象，需要进行深度的监视
                    'stus':{
                        deep:true,
                        handler:function(newV,oldV){
                            console.log(newV[0].name);
                        }
                    }
                },
```

## 9、计算属性==computed==

考虑到在大型项目中，在模板中需要对数据进行复杂的处理，这个时候就需要计算属性computed，这样会更容易维护。

计算属性非常强大，当计算属性中的某个元素发生变化的时候，就会重新去取数据，当计算属性中没有元素发生变化的话，就不会重新取数据。

###### 计算属性==computed==最大的优点：产生缓存，如果数据没有发生变化 直接从缓存中取

```html
<html lang="en">
    <body>
        <div id="app">          
            {{reverseMsg}}
            <h3>{{fullName}}</h3>
            <button @click="handleClick">改变</button>
        </div>
        <script src="./vue.js"></script>
        <script>
            new Vue({
                el:"#app",
                data:{
                   msg:'hello world',
                   firstName:'小马',
                   lastName:'哥'
                },
                computed:{
                    //computed默认只有getter方法
                    reverseMsg:function(){
                        return this.msg.split('').reverse().join('');
                    },
                    fullName:function(){
                        return this.firstName+this.lastName;
                    }
                },
                methods:{
                    handleClick(){
                        this.msg="计算属性computed";
                        this.lastName='妹';
                    }
                }
            })
        </script>
    </body>
</html>
```

## 10、计算属性computed的==setter==方法

stter方法不是必须的，

```html
<body>
        <div id="app"> 
            {{content}}         
            <input type="text" v-model="content" @input="handleInput">
            <button @click='handleClick'>获取</button>
        </div>
        <script src="./vue.js"></script>
        <script>
           
            new Vue({
                el:"#app",
                data:{
                   msg:''
                },
                computed:{
                   content:{
                       set:function(newV){
                        this.msg=newV;
                        },
                        get:function(){
                            return this.msg;
                        }
                   }
                },
                methods:{
                   handleInput(event){
                    console.log(event);
                    const {value}=event.target;//这个就可以将长长的嵌套分解出我们要的值
                    this.content=value;
                   },
                   handleClick(){
                    if(this.content){
                        console.log(this.content);
                    }else{
                        console.log('没有获取到值');
                    }
                    
                   }
                }
            })
        </script>
    </body>
```

## 11、过滤器==filters==

为数据添油加醋，

```html
 <body>
        <div id="app"> 
           <h3>{{price|myPrice('$')}}</h3>
           <h3>{{msg|myReverse}}</h3>
        </div>
        <script src="./vue.js"></script>
        <script>
        // 为数据添油加醋
            // 创建全局的过滤器
            Vue.filter('myReverse',(val)=>{
                return val.split('').reverse().join('');
            })
            new Vue({
                el:"#app",
                data:{
                   price:10,
                   msg:'hello 过滤器'
                },
                // 局部的过滤器
                filters:{
                    myPrice:function(price,a){
                        // console.log(price);
                        return a+price;
                    }
                }
            })
        </script>
    </body>
```

