### 原生JS开发ToDoList组件

https://www.w3cschool.cn/fetch_api/fetch_api-zilr2ng2.html    目前就只有Chrome支持



#### webcomponent之m-radio组件

- customElements属性：该 Window 接口的 customElements 只读属性用于返回对该 CustomElementRegistry 对象的引用，该对象可用于注册新的自定义元素并获取有关以前注册的自定义元素的信息

```html
<m-radio></m-radio>
<script type="text/javascript">
    //页面所有元素都继承自HTMLElement,可以根据需求继承自div等其他元素
    class MRadio extends HTMLElement {
        //如果子类重写了constructor就必须调用父级的构造函数 super()
        constructor(){      //页面使用了m-radio元素就会调用构造函数
            super();
            console.log("重写构造函数")  
        }
    }

    customElements.define("m-radio",MRadio);   //第三个参数是继承的对象，比如 {extends: 'p'}
</script>
```



#### 组件结构、样式、数据

- 先调用attachShadow创建元素的“内容容器”，再将子元素插入到shadow
- 先创建标签style，用textContent方法设置样式，再将style插入到shadow
- 设置元素属性，作为传入的数据，用getAttribute获取内容，然后使用JSON.parse方法转为JSON格式，使用map和join将数据整理为元素插入

```html
<m-radio data="['上海'，'北京']" index="0"></m-radio>
<m-radio data="['奶茶','蛋糕']"></m-radio>
<script type="text/javascript">
//页面所有元素都继承自HTMLElement,可以根据需求继承自div等其他元素
class MRadio extends HTMLElement {
    //如果子类重写了constructor就必须调用父级的构造函数 super()
   constructor(){      //页面使用了m-radio元素就会调用构造函数
        super();
        console.log("重写构造函数")  
        
        //给m-radio添加内部容器，让子元素都在m-radio内部
        let shadow = this.attachShadow({
            mode:"open"
        });
        
        let data = [];  //处理传入数据为空时JSON.parse报错的情况
        let index = 0;
        try{
            index = this.getAttribute('index')||0;
            data = JSON.parse(this.getAttribute('data'));  //将字符串转为JSON格式
            console.log(data)
        }catch(e){
            throw new Error("报错信息")
        }
        
        let str = data.map( (item,n) => {
            if(n == index ){   //判断选中的index是否和下标n相等以此判断是否选中
                return `<li class="selected" data-n="${n}">${item}</li>`
            }else{
                return `<li data-n="${n}">${item}</li>`
            }
        }).join('')  //map返回数组需要转为字符串
        
        //冒泡   点击子元素会触发事件，给子元素设置data-n属性判断点击的元素是哪一个
        shadow.addEventListener('click',function(e){
            if(e.target.tagName.toUpperCase() == 'LI'){
                index = e.target.dataset.n; //更改选中元素，可以使用dataset获取data-中存储的数据

                //设置选中元素后要重新渲染
                let str = data.map( (item,n) => {
                if(n == index ){   //判断选中的index是否和下标n相等以此判断是否选中
                    return `<li class="selected" data-n="${n}">${item}</li>`
                }else{
                    return `<li data-n="${n}">${item}</li>`
                }
            }).join('')  //map返回数组需要转为字符串
                div.innerHTML = `<ul>${str}</ul>`;   //注意使用的是反引号
            }
        })
           
        let style = document.createElement('style');   //这里创建的样式不会对外面的元素造成影响
        style.textContent = `
        .selected{
            color:red;
        }
        ul{
            list-style:none;
        }`;
        
        let div = document.createElement('div');
        //或者直接在shadow中添加
        div.innerHTML = `<ul>${str}</ul>`;   //注意使用的是反引号
        shadow.appendChild(style);
        shadow.appendChild(div);
    }
}

customElements.define("m-radio",MRadio); 
</script>
```



#### 组件抽象—Component类

- 将上述的代码划分为数据、事件、样式、结构

```html
<m-radio data='["上海","北京"]' index=0></m-radio>
<m-radio data='["奶茶","蛋糕"]' index=1></m-radio>
    <script type="text/javascript">

    //页面所有元素都继承自HTMLElement,可以根据需求继承自div等其他元素
    class MRadio extends HTMLElement {
         //想要动态监听数据需要在组件内部添加生命周期
        static get observedAttributes(){return ['data','index']}   

        //构造函数的内容···

        //如果子类重写了constructor就必须调用父级的构造函数 super()
       constructor(){      //页面使用了m-radio元素就会调用构造函数
            super();
            console.log("重写构造函数")  
            
            //给m-radio添加内部容器，让子元素都在m-radio内部
            let shadow = this.attachShadow({
                mode:"open"
            });

            this.data();
            this.event();
            this.style();
            this.elment();

            //使用this.shadowRoot获取到shadow
        }

        //组件——数据
        data(){
            this.state = {};   //存储数据
            try{
                this.state.index = this.getAttribute('index')||0;
                this.state.data = JSON.parse(this.getAttribute('data'));  //将字符串转为JSON格式
            }catch(e){
                this.state.index = 0;
                this.state.data = [];
            }
        }
        
        //组件——事件  :注意是给shadow添加事件，而不是组件
        event(){
            //冒泡   点击子元素会触发事件，给子元素设置data-n属性判断点击的元素是哪一个
            this.shadowRoot.addEventListener('click',e=>{
                if(e.target.tagName.toUpperCase() == 'LI'){
                    this.state.index = e.target.dataset.n; //更改选中元素，可以使用dataset获取data-中存储的数据
                    //触发事件  改变外部元素
                    this.dispatchEvent(new CustomEvent('change',{
                        detail:{
                            index:this.state.index   //传递数据
                        }
                    }))

                    //设置选中元素后要重新渲染
                    this.render();
                }
            })
        }

        //组件——样式
        style(){
            let style = document.createElement('style');   //这里创建的样式不会对外面的元素造成影响
            style.textContent = `
            .selected{
                color:red;
            }
            ul{
                list-style:none;
            }`;

            //this指向的是当前组件，需要用this.shadowRoot代替shadow
            this.shadowRoot.appendChild(style);    
        }

        //组件——结构
        elment(){
            let div = document.createElement('div');
            this.shadowRoot.appendChild(div);
            this.render();
        }

        //结构渲染
        render(){
            let div = this.shadowRoot.querySelectorAll('div')[0];
            let str = this.dataToElement();
            //或者直接在shadow中添加
            div.innerHTML = `<ul>${str}</ul>`;   //注意使用的是反引号
        }

        dataToElement(){
            let str = this.state.data.map( (item,n) => {
                if(n == this.state.index ){   //判断选中的index是否和下标n相等以此判断是否选中
                    return `<li class="selected" data-n="${n}">${item}</li>`
                }else{
                    return `<li data-n="${n}">${item}</li>`
                }
            }).join('')  //map返回数组需要转为字符串
            return str;
        }

        
        //监听数据data,index变化，变化时将数据渲染
        attributeChangedCallback(){
            this.data();
            this.render();
            console.log("change")
        }

    }

    customElements.define("m-radio",MRadio); 
    let mRadio = document.querySelectorAll('m-radio')[0];
    mRadio.addEventListener('change',function(e){
        console.log(e.detail.index);   //监听change
    });
    mRadio.setAttribute('data',JSON.stringify(["上海","北京"]));  //动态设置html结构中的数据
 </script>
```



#### 组件复用与生命周期

- 使用多个组件，可以将相同的处部分抽取到父级class中，让让组件都继承自相同的父级
- 将组件事件与外部元素变化联系起来  dispatchEvent  然后自定义事件CustomEvent（完整代码如上代码块所示）

```js
event(){
    //冒泡   点击子元素会触发事件，给子元素设置data-n属性判断点击的元素是哪一个
    this.shadowRoot.addEventListener('click',e=>{
        console.log(e.target.tagName.toUpperCase())
        if(e.target.tagName.toUpperCase() == 'LI'){
            console.log('li')
            this.state.index = e.target.dataset.n; 
            
            //触发事件  改变外部元素
            this.dispatchEvent(new CustomEvent('change',{
                detail:{
                    index:this.state.index   //传递数据
                }
            }))
            this.render();
        }
    })
}

//在组件外部对事件进行监听
customElements.define("m-radio",MRadio); 
let mRadio = document.querySelectorAll('m-radio')[0];

mRadio.addEventListener('change',function(e){
    console.log(mRadio.index);
});
```

```js
class MRadio extends HTMLElement {
     //想要动态监听数据需要在组件内部添加生命周期
    static get observeAttrbutes(){return ['data','index']}   

    //构造函数的内容···

    //监听数据data,index变化，变化时将数据渲染
    attributeChangedCallback(){
        this.data();
        this.render();  //渲染
    }
}

```

