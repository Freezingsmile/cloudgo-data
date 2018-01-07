# cloudgo-data

借鉴了老师的很多代码
"github.com/pmlpml/golang-learning/web/cloudgo-data-template/service"
-
>似乎我能力有限，很难在有的时间内作出比老师的方案更好的更改，因此厚颜谈下我对老师代码的理解吧。

 
<code>func NewServer() *negroni.Negroni {
	formatter := render.New(render.Options{		
  IndentJSON: true,	
  })
  
	n := negroni.Classic()	
  mx := mux.NewRouter()
  
	initRoutes(mx, formatter)
	n.UseHandler(mx)	
  return n
  }</code>
  
  在服务器端返回JSON的值，用以进行下一步的数据调用和处理，我们对这个返回的JSON值可以用test.js 中使用Jquery的Ajax方法请求这个地址，在本地的console记录下得到的数据打开firefox的开发者工具，可以看到console记录下了返回的数据。
 
