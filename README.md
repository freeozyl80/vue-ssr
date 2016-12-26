# vue-ssr
vue 同构直出小测试

## vue同构直出个人理解；(主流程整理)

生成模式下：
node server.js;

1. 准备工作：


> a.vue-server-renderer;
  ps: 这里会产生个render的实例； 将vue打包成string到prd环境，将vue打包成stream到dev环境。

  :renderToString(vm, (err. html) = > { console.log(html)}); //Render a Vue instance to string;
  :renderToStream() 返回stream.on('data', chunk => {res.write(chunk)}) // Render a vue instance to a Node.js readable stream;

  :createBundleRenderer(code)  => code相当于 webpack的产出文件（个人理解）
  :bundleRenderer.renderToString([context], cb)  
  :bundleRenderer.renderToStream([context])
	
>
  webpack  {
  	 1 => client的webpack // ?
  	 2 => server的webpack //需要定义一个context; 这个context保留着基本store数据。
  }  

>

2  流程工作

> a.express的启动；

> b.html文件坑处理  =》 生成为 HEAD , TAIL ,中间空出的是{{APP}}

> c.创建bundleRenderer 将server-bundle.js（就是webpack对server端的输出）打入
	ps: 这个时候其实已经生成了完整的html，不过（server-rendered="true"）

> d.get(/)的时候将bundleRenderer的renderToStream输出。

> e.执行 webpack打包的client的文件，并挂载节点，主要是replaceState; 

  ps: 这里就要说下vue ssr的流程了， vue ssr(看做个浏览器)其实只是实现一个virture dom，执行到vue的生命周期的beforemonut. vue在这个过程中的store其实会发生更改的（vue 里面一般使用fetchData），那么我们可以理解其实ssr就是vue实例到mount前面那一部分。

