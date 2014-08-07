nodejs---
=========

1. 基于Nodejs内建的调试器

  Nodejs提供了一个内建调试器来帮助开发者调试应用程序。想要开启调试器我们需要在代码中加入debugger标签，当Nodejs执行到debugger   标签时会自动暂停（debugger标签相当于在代码中开启一个断点）。代码如下：
  var path = url.parse(req.url).pathname;
  debugger
  ;

  res.writeHead(200, {'Content-Type': 'text/plain'});
  执行命令：node debug example.js 就可以进入调试模式。

2. 基于V8插件的调试器

  Nodejs是基于google V8的引擎上构建的，Google为Eclipse提供了一个对应的调试插件。关于如何在Eclipse中安装和调试Nodejs程序就不再  重复描述了， 网上已经有很多的文章了（具体可以参考这篇文章http://cnodejs.org/blog/?p=911）。唯一要注意的是在默认情况下V8引   擎  支 持的调试模式是本地模式。如果想要开启远程调试的话，我们需要修改Nodejs中的V8源文件：/deps/v8/src/platform- posix.cc
    addr.sin_family = AF_INET;
    addr.sin_addr.s_addr = htonl(INADDR_LOOPBACK
  ); --> INADDR_ANY
    
  addr.sin_port = htons(port);
  然后重新编译Nodejs。
  提示：
  用插件来调试nodejs程序，你有时候会遇到什么connect refuse, get version failed等等错误。那么请注意你使用的ip的地址，          一般下127.0.0.1的回环地址是都工作的。如果你使用真实的ip地址，请检查防火墙设置。
  
3. 基于Chrome浏览器的调试器(推荐使用)

  既然我们可以通过V8的调试插件来调试，那是否也可以借用Chrome浏览器的JavaScript调试器来调试呢？node-inspector模块提供了这样一   种可能。我们需要先通过npm来安装node-inspector
  npm install -g node-inspector  // -g 导入安装路径到环境变量 node-inspector是通过websocket方式来转向debug输入输出的。
  node-inspector &   //我们在调试前要先启动node-inspector来监听Nodejs的debug调试端口。
  node --debug-brk app.js //运行app.js
  127.0.0.1:8080/debug?port=5858//页面
