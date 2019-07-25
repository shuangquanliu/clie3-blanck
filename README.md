
## 移动端适配
## 适配的目的是什么？？？
  1. 让同一个页面在不同的机型上显示的效果一样
  2. 不同的机型： 屏幕大小不一样
  3. 适配的最终目的： 让一个页面等比显示
## 常用的适配方案呢
  1. viewport
  2. rem
## viewport 视口
  1. 视口分类： 布局视口，视觉视口，理想视口
  2. 视觉视口： 所见即所得
  3. 布局视口： 页面的宽度，默认的布局视口的宽度是： 980
  4. <meta name="viewport" content="width=device-width,initial-scale=1.0">
  5. width=device-width ---> 布局视口 = 视觉视口
  6. initial-scale=1.0 页面和视口的比例
## rem
  1. 定义： root em 根节点字体的大小
  2. px em rem
    1. px： 就是css像素单位
    2. em： 父节点字体大小
    3. rem: 根节点字体大小
## 适配方案
  1. 淘宝方案
    1. 将屏幕等分10份作为rem的值
  2. 网易方案
    1. iphone6： 1rem = 100px
  

# 适配方案

## 1. 安装依赖
    npm install px2rem-loader  lib-flexible --save 
## 2. 设置viewport 
    设置: <meta name="viewport" content="width=device-width,initial-scale=1.0">
      1. 说明: 因为默认的布局视口大于视觉视口，如果不设置将导致页面的内容显示非常小
      2. 原理: 将980的页面在375的屏幕上完全显示只能缩小980页面中的内容
## 3. 直接设置px问题
    1. px为css像素单位等同于独立设备像素单位
    2. 设计师给的设计稿的单位是物理像素单位
    3. 直很多设备上接设置px像素单位会比设计师的要求大
     
## 4. 使用px2rem， lib-flexible   

  1. 在项目入口文件 main.js 里 引入 lib-flexible， flexible会自动根据设备情况动态设置rem的值的大小
    1. import 'lib-flexible/flexible'
  2. 在build文件夹下的util.js中添加
  ```
      const px2remLoader = {
          loader: 'px2rem-loader',
          options: {
          remUnit: 37.5  // remUnit为转换rem的基础 设计稿单位/等分数 = remUnit
          }
      }



      
      function generateLoaders (loader, loaderOptions) {
          // 添加使用 px2remLoader
          const loaders = options.usePostCSS ? [cssLoader, postcssLoader,px2remLoader] : [cssLoader, px2remLoader]
      
          if (loader) {
            loaders.push({
              loader: loader + '-loader',
              options: Object.assign({}, loaderOptions, {
                sourceMap: options.sourceMap
              })
            })
          }
      
          // Extract CSS when that option is specified
          // (which is the case during production build)
          if (options.extract) {
            return ExtractTextPlugin.extract({
              use: loaders,
              fallback: 'vue-style-loader'
            })
          } else {
            return ['vue-style-loader'].concat(loaders)
          }
        }


## 6. 使用px2rem， lib-flexible    脚手架3配置 
        module.exports = {
          css: {
              loaderOptions: {
                css: {},
                postcss: {
                  plugins: [
                    require('postcss-px2rem')({
                      remUnit: 37.5
                    })
                  ]
                }
              }
          },
      }
  ```
 ## 7. 原生实现
 ```
      let timeoutId;
      window.addEventListener('pageshow', function () {
        refreshRem()
      })
      window.addEventListener('resize', function () {

        if(timeoutId){
          clearTimeout(timeoutId);
        }
        timeoutId = setTimeout(function () {
          refreshRem()
        }, 300)
      })

      function refreshRem() {
        // 获取屏幕的宽度
        let clientWidth = document.documentElement.clientWidth;
        // 将屏幕宽度10等分
        let fontValue = clientWidth / 10;
        // 获取屏幕的设备像素比dpr的值
        let dpr = window.devicePixelRatio;
        // 确定dpr的值
        if(dpr >= 3){
          dpr = 3;
        }else if(dpr < 3 && dpr >=2){
          dpr = 2;
        }else {
          dpr = 1;
        }
        // 设定单位rem值的大小
        let rem = fontValue;

        // px转换rem的基础, 
        let remUnit = fontValue * dpr;
        // 设定body标签字体大小
        document.body.style.fontSize = '12px';
        // 设定html上fontsize的大小
        document.documentElement.style.fontSize = rem + 'px';
      }
```


