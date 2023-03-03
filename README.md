# Vue Web Loader 2

## 具体流程如下：
通过`VueWebLoader`对象加载入口文件，开始Web应用的远程加载。
1. 解析传入的version选项，在后续远程加载时会添加为v参数
2. 解析得到入口文件路径entry，使用`fetch`远程加载入口文件`requestGet`
3. 获取入口文件的文本内容，并开启解析回调
4. 解析网页的根路径为`base_url`
5. 解析`parseImport`入口文件文本，修改所有的导入语句：
   1. 删除搜有注释
   2. 替换代码（`import xx from yy.js/vue`语句注释，并添加`var xx=windows.VueWebLoader.registered_component[.js/vue的http链接];`）
   3. `import xx`语句注释，并添加`var xx=windows.xx`
   4. 返回被修改的源代码，所有的`.js/vue`的http链接
6. 加载`importComponents`所有的`.js/vue`的http链接
   1. 加载完成所有的http链接，`.js`使用`requestJs`解析脚本内容，`.vue`使用`requestVue`解析脚本内容
   2. 执行回调：执行`5`返回的源码内容
7. `requestVue`解析Vue文件流程如下：
   1. 解析出script、template、style文本内容，template和style通过`document.createElement`方式获取
   2. 使用步骤`5`解析script内容；
   3. `importComponents`加载加载完所有的`.js/vue`的http链接——递归加载
   4. 使用`registerVueSFC`注册vue组件
      1. style添加到head中；
      2. template添加为vue options的template字段
      3. 通过`Vue.component`注册组件并返回
   5. 检查所属父组件的全部子组件是否已加载完成
   6. 执行回调（一般都没有）
8. `requestJs`解析js文件流程如下：
   1. 使用步骤`5`解析脚本内容内容；
   2. 使用`importComponents`加载完所有的`.js/vue`的http链接，在回调中执行脚本内容
   3. 检查所属父组件的全部子组件是否已加载完成
9.  
10. 
