# Vue uploader solutions

Vue上传的解决方案

目前主要处理 `vue-simple-uploader` 的方案，基于 `vue-simple-uploader` 封装了可以分片、秒传及断点续传的上传插件

预览：[https://shady-xia.github.io/vue-uploader-solutions](https://shady-xia.github.io/vue-uploader-solutions)

## 文章

该仓库有对应的文章进行分析：

[基于vue-simple-uploader封装文件分片上传、秒传及断点续传的全局上传插件](https://www.cnblogs.com/xiahj/p/vue-simple-uploader.html)

[vue-simple-uploader 常见问题整理](https://www.cnblogs.com/xiahj/p/15950975.html)

## 本地调试

**前端服务**：
```bash
npm install

npm start
# or
npm run serve
```

前端服务默认打开8080端口

**node服务端**

```bash
cd server
npm install
npm run server
```

node服务会打开3000端口，临时文件存在 tmp 目录下，上传成功的文件存在 uploads 目录下

## 插件的封装及使用

### GlobalUploader

`GlobalUploader.vue` 为基于 `vue-simple-uploader` 二次封装的上传插件，源码路径为 `/vue-simple-uploader/GlobalUploader.vue`

它有两种使用方式：

### 通过bus作为全局组件使用

作为全局上传组件使用时，将组件注册在`App.vue`中，通过 `Event Bus` 的方式调用插件。

使用场景为：上传器为整个网站级别的，切换路由时不打断上传流程，上传窗口始终存在于网站右下角。

**打开上传器**

调用`Bus.$emit('openUploader')`，打开上传器，弹出选择文件窗口，该函数有两个参数：

* params：传给服务端的额外参数
* options：上传选项，目前支持 target、testChunks、mergeFn、accept

```js
Bus.$emit('openUploader', {
  params: {
    page: 'home'
  },
  options: {
    target: 'http://10.0.0.10'
  }
})
```

**Bus Events**

* fileAdded：文件选择后的回调
* fileSuccess：文件上传成功的回调

```js
// 文件选择后的回调
Bus.$on('fileAdded', () => {
  console.log('文件已选择')
})

// 文件上传成功的回调
Bus.$on('fileSuccess', () => {
  console.log('文件上传成功')
})
```

### 作为普通组件在单个页面中使用

使用场景为：在单个页面中使用上传器

props：
* global：请务必设置为 `false`，代表非全局
* params：（同用法一）传给服务端的额外参数
* options：（同用法一）上传选项，目前支持 target、testChunks、mergeFn、accept

events:
* fileAdded：文件选择后的回调
* fileSuccess：文件上传成功的回调

```html
 <global-uploader
    :global="false"
    :params="{page: 'home'}"
    :options="{target: 'http://10.0.0.10'}"
    @fileAdded="fileAdded"
 />
```

## 其他上传器

### vue-webuploader（已不推荐）

见文章：
[Vue2.0结合webuploader实现文件分片上传](https://www.cnblogs.com/xiahj/p/8529545.html)

## License

请捍卫自己作为劳动者的权利

[![LICENSE](https://img.shields.io/badge/license-Anti%20996-blue.svg)](https://github.com/996icu/996.ICU/blob/master/LICENSE)
[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu)
