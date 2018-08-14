##vue_splider
项目基于Vue2+axios+VueRouter的一个应用,招聘信息（前程无忧/智联招聘/boss直聘/拉勾网）、热点新闻展示、歌曲排行展示播放、热播电影详情查看、微信精选、百度美图、搞笑段子,


> 使用技术:
* Vue2.0
* axios+promise 网络请求封装
* VueRouter
* mint-UI
* API接口数据：[TP5_splider项目](https://github.com/jiajun00/TP5_Splider/) 


浏览地址：
[演示](http://splider.qqxio.cn)



### Build Setup 安装部署运行
-----------------------------------------------------
```
#clone
git clone https://github.com/jiajun00/vue_splider.git
# install dependencies
npm install

# serve with hot reload at localhost:800
npm run dev

###
npm run build

```

### 网络请求封装
>这里封装了两个网络请求方法  fetchPost，fetchGet以及请求配置，通过请求

config,js
```javascript
import axios from 'axios'
import qs from 'qs'
// axios 配置
axios.defaults.timeout = 5000;
axios.defaults.baseURL = 'https://api.xxx.ccom/api';

//POST传参序列化
axios.interceptors.request.use((config) => {
    if (config.method === 'post') {
        config.data = qs.stringify(config.data);
    }
    return config;
}, (error) => {
    alert("错误的传参")
    return Promise.reject(error);
});

/**
 * 
 * POST 请求方式
 * @param {string} url     请求URL
 * @param {object} params  请求参数
 * @returns 
 */

export default {
    //fetchPost  请求方式
    fetchPost: function(url, params) {
        return new Promise((resolve, reject) => {
            axios.post(url, params)
                .then(response => {
                    resolve(response.data);
                }, err => {
                    reject(err);
                })
                .catch((error) => {
                    reject(error)
                })
        })
    },


    //GET 请求方式
    fetchGet: function(url, params) {
        console.log(params)
        return new Promise((resolve, reject) => {
            axios.get(url, {
                    params: params
                })
                .then(response => {
                    resolve(response.data);
                }, err => {
                    reject(err);
                })
                .catch((error) => {
                    reject(error)
                });
        })

    }
}

```
>api.js 为所有接口封装,如下面的相关接口

```javascript
import fetch from './config'
export default {
    /**
     * 新闻轮播图
     * @param {object} params 
     * @returns 
     */
    banner(params) {
        return fetch.fetchPost('News/banner', params);
    },
    /**
     * 文章分类列表
     * @param {object} params {page：分页，type:文章类型}
     * @returns 
     */
    new_list(params) {
        return fetch.fetchGet('News/new_list', params)
    }
    }
```

> 使用请求接口数据

```javascript
 // 获取首页新闻列表
    [types.FECTH_INDEX_NEWS]({commit}) {
        var data={
            page:20,
            type:0
        }
        api.new_list(data)
            .then(res => {
                commit(types.TOGGLE_INDEX_NEWS, res.data)
            }).catch(err => console.log(err))
    }
```


