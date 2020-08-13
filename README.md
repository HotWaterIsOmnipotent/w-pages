# w-pages 

> gulp打包工具包

## cli

```shell
# 清除打包产物
$ gulp clean

# 生产打包
$ gulp build

# 本地开发
$ gulp dev
```

## 过程
  1. 首先安装gulp和gulp-cli，使本地可使用gulp，并从gulp中解构出src读取流和dest写入流。
  2. 实现js类型文件的打包，引入gulp-babel、@babel/core、@babel/preset-env三个模块，封装script任务，实现js新特性编译为es5。
  3. 实现scss类型文件打包，引入gulp-sass模块，封装style任务，实现scss类型文件编译成css。
  4. 实现html类型文件按照模板进行渲染，并压缩打包，封装page任务，引入gulp-swig模块使用swig模板对html进行渲染同时注入变量。
  5. 实现图片和字体资源的打包，引入gulp-imagemin模块，分别封装image任务和font任务，通过gulp-imagemin模块对各种图片格式和svg格式的字体进行特殊打包处理，其他类型直接打包。
  6. 实现其他静态资源打包，封装extra任务直接进行打包。
  7. 实现本地启动服务，并进行实时热编译，引入browser-sync模块，封装serve任务，通过browser-sync任务在本地启动服务，并从gulp中解构出watch方法对js，css，html文件进行监听，实时监听这三类文件的变化并重新执行相应任务或重载页面实现热编译。
  8. 每次打包或者启动服务前先删除上一次打包的产物，引入del模块，封装clean任务，使用del模块清除掉指定目录下的打包文件。
  9. 实现将打包后的文件进行生产环境压缩，引入useref模块，封装useref方法，并引入gulp-uglify模块实现js文件的压缩，引入gulp-clean-css模块实现css类型文件的压缩，引入gulp-htmlmin模块对html类型文件进行压缩。
  11. （非必要）引入gulp-load-plugins模块，将所有gulp模块统一使用该模块导出引用，无需一一导出引用。
  10. 从gulp中解构出parallel（并行）, series（串行）对之前封装的任务进行组合，组合成：
    - **build**：生产时打包需要用先串行执行clean，清除掉之前打包产物后，再并行执行script、style、page、image、font、extra任务到dist文件夹下，实现生产环境打包。
    - **compile**：开发时编译js、css、html，并行执行script、style、page任务到.temp目录下实现开发环境临时编译，
    - **dev**: 本地开发时，先串行执行clean，清除掉之前打包产物后，先执行compile任务生成临时编译产物后再执行serve任务启动本地服务。
