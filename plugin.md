# 插件用法
### axios基本用法
#### get
```js
axios.get(url, params)
.then( res => {
}).catch( err => {
})
```
#### post
```js
axios.post( url, params)
.then( res => {
}).catch( err => {
})
```
#### 配置的默认值/defaults
```js
// 默认请求地址
* axios.defaults.baseURL = 'https://api.example.com';
// 默认请求超时时间
* axios.defaults.timeout = 2000;
// header添加自定义信息
* axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
// 
* axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```
#### 拦截器
```js
// 添加请求拦截器
axios.interceptors.request.use(function (config){
    // 在发送请求前添加一些处理
    return config
})

// 添加响应拦截器
axios.interceptors.response.use(function (config){
    // 对响应数据添加处理
    return config
})
```
### vue-bspicker(城市选择插件)

``` js
<template>
    <div class="area-dialog"
    v-show="showArea">
        <vue-picker 
        v-show="showArea"
        :data="linkageData" ref="picker" 
        :selected-index="selectedIndex"
         @select="select"
         @cancel="handleCancel" 
         @change="handleChange"></vue-picker>
    </div>
</template>
<script>
import vuePicker from 'vue-bspicker'
let province = [],
    city = [],
    area = [];
var data = [province, city, area]
const COMPONENT_NAME = 'vue-city-picker'
	export default {
        name: COMPONENT_NAME,
        components: {
			vuePicker
		},
        data () {
            return {
                showArea: false,
                areaDialogShow: false,
                data: [],
                selectedIndex: [0, 0, 0],
                tempIndex: [0, 0, 0],
            }
        },
        computed: {
	      	linkageData: function() {
	      		this.data = this.data.length > 0 ? this.data : [province, city, area]
                let provinceList = this.data[0];
      			let cityList = this.data[1].filter((item) => {
                    return item.parentId === provinceList[this.tempIndex[0]].value;
                })
				let areaList = [];
      			if(cityList.length > 0){
					areaList = this.data[2].filter((item) => {
						return item.parentId === cityList[this.tempIndex[1]].value
					})
                  }
		        return [provinceList, cityList, areaList]
	      	}
	    },
        created() {
             this.$event.$on('AREA_DIALOG_city', (obj) => {
                 this.$refs.picker.show();
                 this.showArea = true;        
            })
            this.$event.$on('INIT_CITY_DATA', _data => {
                this.getProvince(_data)
                this.data = data;
            })
        },
        watch: {
	        linkageData: function() {
	        	this.$refs.picker.refresh()
	        }
	    },
        methods: {
            select(...rest){
                this.showArea = false;
                this.$event.$emit('SHIPPING_ADDRESS_CITY', rest[3]);
            },
            handleChange(index, curIndex){
               if (curIndex !== this.tempIndex[index]) {
		          	for (let j = 2; j > index; j--) {
		            	this.tempIndex.splice(j, 1, 0)
		            	this.$refs.picker.scrollTo(j, 0)
		          	}
                      this.tempIndex.splice(index, 1, curIndex)
		        }
            },
            handleCancel(){
                this.showArea = false;
            },
            getProvince(data){
                for(var i=0; i<data.length; i++){
                    province.push({
                        value: data[i].id,
                        text: data[i].name,
                        parentId: 0})
                        this.getCity(data[i].child, data[i].id)
                }
            },
             getCity(data, _parentId){
                for(var i=0; i<data.length; i++){
                    city.push({
                        value: data[i].id,
                        text: data[i].name,
                        parentId: _parentId})
                       this.getArea(data[i].child, data[i].id)
                }
            },
             getArea(data, _parentId){
                for(var i=0; i<data.length; i++){
                    area.push({
                        value: data[i].id,
                        text: data[i].name,
                        parentId: _parentId})
                }
            }
        },
    }
</script>
<style scoped lang="scss">
    .area-dialog{
        position: fixed;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        z-index: 100;
    }
    .confirm{
        color: $c_ff6;
    }
</style>
```
### LayerDialog(自定义弹窗组件)

``` js
<template>
    <div class="buy-layer" v-show="show || showDialog">
        <transition name="fade">
            <div class="mask-layer" v-show="show"></div>
        </transition>
        <transition name="slide-fade"
        @before-enter="beforeEnterLayer"
        @after-leave="afterLeaveLayer">
            <div class="buy-dialog" v-show="show">
                <h3 class="buy-dialog__title">
                    <span>{{titleDialog}}</span>
                    <span class="buy-dialog__close--dialog" @click="handleCloseDialog"></span>
                </h3>
                <section class="buy-dialog__content">
                    <slot></slot>
                </section>
            </div>
        </transition>
    </div>
  </template>
<script>
export default {
  data () {
    return {
        showDialog: false,
    }
  },
  props: {
    titleDialog: String,
    show: {
      type: Boolean,
      default: false
    },
  },
  created() {
  },
  methods: {
      handleCloseDialog(){
          this.$emit('CLOSE_DIALOG')
      },
      beforeEnterLayer(){
          this.showDialog = true;
      },
      afterLeaveLayer(){
        this.showDialog = false;
      }
  },
}
</script>

<style scoped lang="scss">
    .slide-fade-enter-active {
        transition: all .5s ease;
    }
    .slide-fade-leave-active {
        transition: all .5s ease;
    }
    .slide-fade-enter,.slide-fade-leave-to{
        transform: translateY(100%);
    }
    .fade-enter-active {
        animation: fade-in .5s;
    }
    .fade-leave-active {
        animation: fade-in .5s reverse;
    }
    @keyframes fade-in{
        0% {
            opacity: 0;
        }
        100% {
         opacity: 1;
        }
    }

    .buy-layer{
        width: 100%;
        height: 100%;
        position: fixed;
        left: 0;
        top: 0;
        z-index: 100;
    }
    .mask-layer{
        background-color: rgba(0,0,0,0.6);
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        z-index: 101;
    }
    .buy-dialog{
        width: 100%;
        height: 918px;
        padding-top: 50px;
        box-sizing: border-box;
        position: absolute;
        bottom: 0;
        left: 0;
        z-index: 102;
        background-color: $c_fe;
        border-top-left-radius:15px;
        border-top-right-radius:15px;
        &__title{
            height: 50px;
            font-size: 40px;
            font-weight: normal;
            color: $c_23;
            padding: 0 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            &__close--dialog{
                width: 28px;
                height: 28px;
                display: block;
                float: right;
                @include bg('../../assets/buy/close.png');
            }
        }
        &__content{
            margin-top: 40px;
            height: 675px;
            overflow-y: auto;
            padding: 0 30px;
            box-sizing: border-box;
        }
    }

</style>
```

### 获取图片的颜色 getImageMeanColor

```zh
    https://github.com/liyongleihf2006/getImageMeanColor
```