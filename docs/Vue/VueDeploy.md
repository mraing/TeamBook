<!--
 * @Author: your name
 * @Date: 2020-06-29 14:45:37
 * @LastEditTime: 2020-06-29 14:46:47
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: /TeamBook/docs/Vue/Vue部署.md
-->

# Vue 部署

## 根目录部署

```javascript
// config/index.js
module.exports = {
  dev: {
      assetsSubDirectory: 'static',
      // 原 '/' => 新 './'
      assetsPublicPath: './',
  },
  build: {
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),

    // Paths
    assetsRoot: path.resolve(__dirname, '../dist/index.html'),
    assetsSubDirectory: '../dist/static',
    assetsPublicPath: '/',
  }
```

## 非根目录部署

`/dist/` 目录是可以修改的，根据实际的项目需求可以更改这个打包目录

```javascript
// config/index.js
module.exports = {
  dev: {
      assetsSubDirectory: 'static',
      assetsPublicPath: './',
  },
  build: {
    // dist 目录是可以根据实际需求进行更改的
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),

    // Paths
    assetsRoot: path.resolve(__dirname, '../dist/index.html'),
    assetsSubDirectory: '../dist/static',
    assetsPublicPath: './',
  }
```



防止本地的静态资源访问不到

```javascript
// config/utils.js
if (options.extract) {
    return ExtractTextPlugin.extract({
        use: loaders,
        fallback: 'vue-style-loader',
        // 新增或者修改
        publicPath: '../../'
    })
} else {
	return ['vue-style-loader'].concat(loaders)
}
```



```javascript
// router/index.js
export default new Router({
  // 这里与config打包目录的文件夹保持一致
  base: '/dist/',
  routes: []
```