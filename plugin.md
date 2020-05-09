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
### 根据van-picker改造的(城市选择插件)

``` html
<template>
    <div class="picker-dialog"
    v-show="showPicker">
        <div v-if="showPicker" class="picker-dialog__content">
            <van-picker 
            show-toolbar
            :columns="columns" 
            @change="onChange" 
            @cancel="onCancel"
            @confirm="onConfirm"/>
        </div>
    </div>
</template>
<script>

import jsonp from 'jsonp'
export default {
    name: 'Picker',
    data() {
        return {
            columns: [
                {
                    values: [],
                    className: 'column1',
                    defaultIndex: 0
                },
                {
                    values: [],
                    className: 'column2',
                    defaultIndex: 0
                },
                 {
                    values: [],
                    className: 'column3',
                    defaultIndex: 0
                }
            ],
            cityList: [],
            curProvinceList: [],
            curCityList: [],
            curDistrictList: [],
            curProvinceId: "",
            curCityId: "",
            curDistrictId: "",
            showPicker: false,
        }
    },
    created() {
        this.$event.$on('INIT_CITY_DATA', (data) => {
            this.getInitData(data)
        })
        this.$event.$on('AREA_DIALOG_city', () => {
            this.getInitData(this.cityList)
            this.showPicker = true;
        })
    },
    methods: {
        onChange(picker, values, index){ // 当前列index0, 1, 2
            const curProvince = values[0];
            const curCity = values[1];
            const curDistrict = values[2];
            if(index === 0){
                this.cityList.map( item => {
                    if(item.name === curProvince){
                        this.curCityList = this.getCurData(item.child); 
                    }
                })
                this.cityList.map( item => {
                    if(item.name === curProvince){
                        item.child.map( sub => {
                            if(sub.name === this.curCityList[0].name){
                                this.curDistrictList = this.getCurData(sub.child);
                            }
                        })
                    }
                })
                picker.setColumnValues(1, this.getCurName(this.curCityList));
                picker.setColumnValues(2, this.getCurName(this.curDistrictList));
                
            }else if(index === 1){
                 this.cityList.map( item => {
                    if(item.name === curProvince){
                        item.child.map( sub => {
                            if(sub.name === curCity){
                                this.curDistrictList = this.getCurData(sub.child);
                            }
                        })
                    }
                })
                picker.setColumnValues(2, this.getCurName(this.curDistrictList));
            }
        },
        getInitData(data){
            if(data.length > 0){
                this.curProvinceList = this.getCurData(data);
                this.curCityList = this.getCurData(data[0].child);
                this.curDistrictList = this.getCurData(data[0].child[0].child);
            }
            this.columns[0].values = this.getCurName(this.curProvinceList);
            this.columns[1].values = this.getCurName(this.curCityList);
            this.columns[2].values = this.getCurName(this.curDistrictList);
            this.cityList= data;
        },
        getCurData(data){
            let _data = data.map( item => {
                return {
                    name: item.name,
                    id: item.id
                }
            })
            return _data;
        },
        getCurName(data){
             let _data = data.map( item => {
                return item.name
            })
            return _data;
        },
        getCurId(curName, curData){
            let curId;
            curData.map( item => {
                if(item.name ===curName){
                    curId = item.id;
                }
            })
            return curId
        },
        onConfirm(value, index) {
            this.curProvinceId = this.getCurId(value[0], this.curProvinceList)
            this.curCityId = this.getCurId(value[1], this.curCityList)
            this.curDistrictId = this.getCurId(value[2], this.curDistrictList)
            let cityArr = [];
            cityArr[0] = {
                name: value[0],
                id: this.curProvinceId
            }
            cityArr[1] = {
                name: value[1],
                id: this.curCityId
            }
            cityArr[2] = {
                name: value[2],
                id: this.curDistrictId
            }
            this.$event.$emit('SHIPPING_ADDRESS_CITY', cityArr);
            this.showPicker = false;
        },
        onCancel() {
            this.showPicker = false;
        }
    },
}
</script>
<style lang="scss" scoped>
    .picker-dialog{
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background: rgba(37,38,45,.4);
        z-index: 100;
        &__content{
            width: 100%;
            position: absolute;
            bottom: 0;
        }
    }
</style>
```

### 获取图片的颜色 getImageMeanColor

```js
    https://github.com/liyongleihf2006/getImageMeanColor
```
### 小程序中识别html标签的插件 ```wxParse```

```js
 [wxParse](https://blog.csdn.net/weixin_38423356/article/details/78076312 "这是链接")
```
[wxParse](https://github.com/kgm1818?tab=repositories "识别HTML标签")

### vue-jsonp 用法
```js
    this.$jsonp(url,{}).then((data)=> {
        //
    }).catch(error => {
        //
    })
```