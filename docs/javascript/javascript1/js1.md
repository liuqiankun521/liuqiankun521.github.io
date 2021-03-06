# 信息整理
[[toc]]

## 控制台打印VUE实例

```js
console.log($0.__vue__)
```

## 控制台打印徽章
```js
console.log("%c vue-press %c v".concat("1.0.0-beta.1", " ").concat("dd10c50", " %c"), 'background: #35495e; padding: 1px; border-radius: 3px 0 0 3px; color: #fff', 'background: #41b883; padding: 1px; border-radius: 0 3px 3px 0; color: #fff', 'background: transparent');
```

## 控制台打印code参数值
```js
console.log(window.decodeURIComponent(window.location.search.substr(1).match(new RegExp("(^|&)code=([^&]*)(&|$)", "i"))?.[2]));
```
### 测试标签

## 立即调用函数表达式(IIFE)
[IIFE](https://developer.mozilla.org/zh-CN/docs/Glossary/%E7%AB%8B%E5%8D%B3%E6%89%A7%E8%A1%8C%E5%87%BD%E6%95%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F) <Badge text="IIFE"/>
```js
(function () {
    statements
})();
```

## 控制台引入js
```js
(function() {
    const script = document.createElement('script');
    script.src = "https://cdn.bootcdn.net/ajax/libs/moment.js/2.29.1/moment.min.js";
    document.getElementsByTagName('head')[0].appendChild(script);
})()
```

## 网站置灰
```js
(function() {
    const html = document.getElementsByTagName('html')[0];
    html.style = html.style.cssText + "-webkit-filter: grayscale(100%);-moz-filter: grayscale(100%); -ms-filter: grayscale(100%); -o-filter: grayscale(100%); filter: progid:DXImageTransform.Microsoft.BasicImage(grayscale=1); _filter: none; filter: grayscale(100%)"
})()
```

## 浏览器跳转
打开新窗口跳转
```js
window.open('http://www.baidu.com', '_blank')
```
自身窗口跳转
```js
window.open('http://www.baidu.com', '_self')
```


## Element-UI时间段取值

```js
 export default {

        data() {

            return {
                payStartTime: '',
                payEndTime: '',
            }

        },

        computed: {

            /**
             * 支付时间 时间段
             */
            payTimePeriod: {
                set(val) {
                    this.payStartTime = (val === null) ? '' : val[0]
                    this.payEndTime = (val === null) ? '' : val[1]
                }
                ,
                get() {
                    return (this.payStartTime && this.payEndTime)
                        ? [this.payStartTime, this.payEndTime]: []
                }
            },

        }
    }

```

## 功能权限判断

@/directive目录下创建permission.js
```js

import Vue from 'vue'


/**
 * VUE自定义权限指令
 * @author tjh
 * @desc 功能权限判断-可以移除和禁用 目前只启用移除
 * @use v-permission = "'action:add'"
 * @see https://cn.vuejs.org/v2/guide/custom-directive.html
 */
Vue.directive('permission', {
  inserted(el, binding) {
    const perms = JSON.parse(window.sessionStorage.getItem('perms') || '[]')
    if (perms && !perms.contains(binding.value)) {
      el.parentNode.removeChild(el)
      // 禁用
      // el.disabled = true
      // el.classList.add('is-disabled')
    } else {
      throw new Error(`need perm like v-permission="'action:add'"`)
    }
  }
})

// eslint-disable-next-line no-extend-native
Array.prototype.contains = function (val) {
  return this.indexOf(val) !== -1
}

```
main.js 导入
```js
import '@/directive/permission.js'
```


## 限制element组件只可以选今天昨天
```js

         pickerOptions: {
           shortcuts: [{
             text: '今天',
             onClick(picker) {
               picker.$emit('pick', new Date());
             }
           }],
           disabledDate (time) {
             let _now = Date.now();
             let num = 2;
             let timeT = num * 24 * 60 * 60 * 1000;
             let days = _now - timeT;
             return time.getTime() > _now || time.getTime() < days;
           }
         }
```
## input输入框校验
```html
<!--只允许输入数字(整数：小数点不能输入)-->
<input type="number" name="chinese" maxlength="5" οnkeyup="value=value.replace(/[^\d]/g,'')" >
<!--允许输入小数(两位小数)-->
<input type="number"  name="chinese" maxlength="5" οnkeyup="value=value.replace(/^\D*(\d*(?:\.\d{0,2})?).*$/g, '$1')" >
<!--允许输入小数(一位小数)-->
<input type="number" name="chinese" maxlength="5" οnkeyup="value=value.replace(/^\D*(\d*(?:\.\d{0,1})?).*$/g, '$1')" >
<!--开头不能为0，且不能输入小数-->
<input type="number" name="chinese" maxlength="5" οnkeyup="value=value.replace(/[^\d]/g,'').replace(/^0{1,}/g,'')" >
<!--vue element 的输入框input限制只可以输入整数 不能输入拼音符号小数点-->
<el-input onKeypress="return(/[\d]/.test(String.fromCharCode(event.keyCode)))"></el-input>
```

## 解决表格数据分页问题
```HTML
        <el-pagination v-if="pageshow" @size-change="handleSizeChange" @current-change="handleCurrentChange"
                       :current-page="current" :page-sizes="[10, 15, 30, 50]" :page-size="size"
                       :total="total" layout="total, sizes, prev, pager, next, jumper">
        </el-pagination>
```
```js 
  export default { 
    data() {
        return {
           pageshow:true,  
        }
    },
    methods: {
              search() { //search要绑定查询按钮上
                this.current = 1
                this.pageshow = false
                this.getlist() //获取接口数据
                this.$nextTick(() => {
                this.pageshow = true
                })
              },
    }
}
```
## JSON.stringify()的妙用判断数组是否包含某对象，或者判断对象是否相等
```js 
//判断数组是否包含某对象
let data = [
    {name:'echo'},
    {name:'听风是风'},
    {name:'天子笑'},
    ],
    val = {name:'天子笑'};
JSON.stringify(data).indexOf(JSON.stringify(val)) !== -1;//true

//判断两数组/对象是否相等
let a = [1,2,3],
    b = [1,2,3];
JSON.stringify(a) === JSON.stringify(b);//true




//深拷贝
function deepClone(data) {
    let _data = JSON.stringify(data),
        dataClone = JSON.parse(_data);
    return dataClone;
};
//测试
let arr = [1,2,3],
    _arr = deepClone(arr);
arr[0] = 2;
console.log(arr,_arr)//[2,2,3]  [1,2,3]

```
## 解决跨域问题
``` text
浏览器新建快捷浏览浏览器  D:\chrome   --args --disable-web-security --user-data-dir=  这个加浏览器属性 目标里边最后面  先打个空格
--args --disable-web-security --user-data-dir=D:\chrome
```

## 返回上一页
```js 
  export default {

        mounted() {
        },

        destroyed() {
            if (window.history && window.history.pushState) {
                history.pushState(null, null, document.URL);
                window.addEventListener('popstate', this.$router.replace({name: 'home'}), false);
            } else {
                window.removeEventListener('popstate', this.$router.replace({name: 'home'}), false);
            }
        },

    }
```
