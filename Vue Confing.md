# vue.config.js設置

## 建立`vue.config.js`

>   在根目錄自行建立`vue.config.js`

```JS
module.expors={
    
}
```

## 路徑修改

```js
module.expors={
    publicPath:'./', // 檔案路徑修改
    productionSourceMap:false, // 不產生map檔案
}

```

## proxy

```json
// 在vue.config.js建立
deServer:{
    proxy:{
        '/api':{
            target:"",
            changeOrigin:true,
            pathRewrite:{
            	"^/api":""
            }
        }
    }
}
```

## 修复 HMR(热更新)失效

```json
module.exports = {
    chainWebpack: config => {
        // 修复HMR
        config.resolve.symlinks(true);
    }
}
```

## 綜合vue.config.js設置

```json
// vue.config.js
module.exports = {
    // baseUrl從 Vue CLI 3.3 起已棄用，請使用publicPath。
    // baseUrl:'./', 
    // 配置sub-path後訪問路徑為https://xxx-path/sub-path/#/
    publicPath: process.env.NODE_ENV === 'production' ? '/sub-path/' : '/',
    // 輸出文件路徑，默認為dist
    outputDir: 'dist',  
    // 放置生成的靜態資源 (js、css、img、fonts) 的 (相對於 outputDir 的) 目錄。
    assetsDir: '', 
    // 指定生成的 index.html 的輸出路徑 (相對於 outputDir)。也可以是一個絕對路徑
    indexPath: '',
    // 配置多頁應用
    pages: {
        index: {
            // page 的入口
            entry: 'src/index/main.js',
            // 模板來源
            template: 'public/index.html',
            // 在 dist/index.html 的輸出
            filename: 'index.html',
            // 當使用 title 選項時，
            // template 中的 title 標籤需要是 <title><%= htmlWebpackPlugin.options.title %></title>
            title: 'Index Page',
            // 在這個頁面中包含的塊，默認情況下會包含
            // 提取出來的通用 chunk 和 vendor chunk。
            chunks: ['chunk-vendors', 'chunk-common', 'index']
        },
        // 當使用只有入口的字符串格式時，
        // 模板會被推導為 `public/subpage.html`
        // 並且如果找不到的話，就回退到 `public/index.html`。
        // 輸出文件名會被推導為 `subpage.html`。
        subpage: 'src/subpage/main.js',
    },
    lintOnSave: true,  // 保存時 lint 代碼
    // css相關配置
    css: {
        // 是否使用css分離插件 ExtractTextPlugin
        extract: true,
        // 開啟 CSS source maps?
        sourceMap: false,
        // css預設器配置項
        loaderOptions: {
            // pass options to sass-loader
            sass: {
                // 自動注入全局變量樣式
                data: `
                    @import "src/你的全局scss文件路徑";
                `
            }
        },
        // 啟用 CSS modules for all css / pre-processor files.
        modules: false,
    },
    // 生產環境是否生成 sourceMap 文件
    productionSourceMap: false,
    //是否為 Babel 或 TypeScript 使用 thread-loader。該選項在系統的 CPU 有多於一個內核時自動啟用，僅作用於生產構建。
    parallel: require('os').cpus().length > 1,
    // 所有 webpack-dev-server 的選項都支持
    devServer: {
        port: 8080, // 配置端口
        open: true, // 自動開啟瀏覽器
        compress: true, // 開啟壓縮
        // 設置讓瀏覽器 overlay 同時顯示警告和錯誤
        overlay: {
            warnings: true,
            errors: true
        },
        // 設置請求代理
        proxy: {
            '/api': {
                target: '<url>',
                ws: true,
                changeOrigin: true
            },
            '/foo': {
                target: '<other_url>'
            }
        }
    },
}
```

## 關於打包後請求數的優化點Preload and Prefetch

```json
// vue.config.js
module.exports = {
  chainWebpack: config => {
    // 移除 prefetch 插件
    config.plugins.delete('preload');
    config.plugins.delete('prefetch');
}
```
