webpack打包vue速度慢怎么办？

- 确保webpack、npm、node的版本，比如webpack4.x比3.x提升很多
- loader范围缩小到src项目文件，配置include
- eslint代码校验其实是一个很费时间的一个步骤，可以把eslint的范围缩小到src，且只检查\*.js和\*.vue：生产环境不开启lint
- happypack多进程进行
- 动态链接库（DllPlugin），有点类似配置的externals，缺点：不能按需加载，会将配置的第三方库全部打包。





- 使用webpack-bundle-analyzer对项目进行模块分析生成report，查看report后看看哪些模块体积过大，然后针对性优化

- 配置webpack的externals，防止将某些import的包（package）打包到bundle中，而是在运行时（runtime）再去外部获取这些扩展一栏。所以，可以将体积大的库分离出来：

  ```js
  externals: {
  	'element-ui': 'Element',
  	'v-charts': 'VCharts'
  }
  ```

- 然后在main.js中移除相关库的import

- 在index.html模板文件中，添加相关库的cdn引用，如：

  ```js
  <script src="https://unpkg.com/element-ui@2.10.0/lib/index.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/echarts/dist/echarts.min.js"></script>
  ```



- 在处理loader时，配置include，缩小loader检查范围
- 如果require模块时不写后缀名，默认webpack会尝试.js, .json等后缀名匹配，配置extensions，可以让webpack少做一点后缀匹配
- 使用cache-loader启用持久化缓存。使用package.json中的postinstall清除缓存目录