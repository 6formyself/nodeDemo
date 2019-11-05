# Node.js

## 1.原生Http模块

1. 加载核心模块

   ```javascript
   var http = require('http')
   ```

2. 获取服务器实例

   ```javascript
   var server = http.createServer()
   ```

3. 注册请求事件

   ```javascript
   server.on('request', function(request,response){
       console.log('the server has received request, the request url is :' + request.url)
       // 设置响应头 http://tool.oschina.net/commons 可查看对应的响应头
       response.setHerder('Content-Type', 'text/plain; charset=utf-8')
       // 响应信息,要记得结束响应,响应方法只能写字符串或者二进制数据
       response.write('life is short, you need python')
       response.end('the end of response')
   })
   ```

4. 监听端口

   ```JavaScript
   server.listen(8080,function(){
       console.log('the server has listened 8080 port!')
   })
   ```

## 2.Node中的模块系统

### 2.1常用模块

- 文件系统模块:`fs`

  ```JavaScript
  // 获取文件系统对象
  var fileSystem = require('fs')
  console.log(fileSystem)  // exports对象的内容
  // 读取文件,回调函数,读取的数据和错误
  fileSystem.readFile('./data/date.txt',function(error,data){
      // 可以直接用return的方式终止代码运行
      if (error) {
          return console.log("read file failed!")
      }
      console.log(data.toString())
  })
  // 写入文件
  fileSystem.writeFile('./data/data.txt',"life is short, you need python!",function(error){
      if (error) {
              console.log("write file failed!")
          } else{
              console.log("write file success!")
          }
  })
  ```

- 操作系统模块:`os`

  ```JavaScript
  var os = require('os')
  os.cpus() // 获取当前机器的CPU信息
  os.totalmem() // 获取当前机器的内存
  ```

- 路径模块:`path`

  ```javascript
  // 操作路径
  var path = require('path')
  path.extname('01_read_file.js') // 获取文件的拓展名->.js
  ```

### 2.2模块加载规则

- 三种模块:

  1. 具名的核心模块
  2. 用户自己编写的文件模块
  3. 第三方模块

- 模块加载顺序:

  1. 优先从缓存中加载,如果加载过一个模块,则第二次加载的时候直接从缓存中获取(就不会二次执行模块中的代码).

  2. 加载第三方模块时,会先找到当前文件所属目录中的node_modules, 再从node_modules中找到模块的文件夹,然后找到该文件夹下的package.json文件,最后找package.json中的main属性,从而找到模块的入口,如果没有package,json,则会默认加载index.js.

     ```txt
     require('art-template')==>当前目录的node_modules下的art-template文件夹==>package.json==>main属性中的标识==>xxx.js
     ```

  3. 如果当前目录未找到则会一直往上一级目录找,直到磁盘根目录,否则 => can not find module.

## 3.npm

- 淘宝npm镜像:

  ```shell
  npm config set registry=https://registry.npm.taobao.org
  ```

## 4.Express

- 加载模块

  ```JavaScript
  var express = require('express')
  ```

- 创建应用程序

  ```JavaScript
  var app = express()
  ```

- 绑定请求

  ```JavaScript
  app.get('/',function(request,response){
      // 获取get请求的参数,转化成json
      var requestParameter = request.query()
      response.send('hello express')
  })
  ```

- 监听端口

  ```JavaScript
  app.listen(3000,function(){
      console.log('app is running')
  })
  ```

- 公开静态资源:第一个参数其实是需要公开的文件夹的别名

  ```JavaScript
  // http://127.0.0.1:3000/a.js
  app.use(express.static('./public/'))
  
  // http://127.0.0.1:3000/public/a.js
  app.use('/public/',express.static('./public/'))
  
  // http://127.0.0.1:3000/a/a.js
  app.use('/a/',express.static('./public/'))
  ```

  