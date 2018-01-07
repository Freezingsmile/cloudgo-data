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
	mx := mux.NewRouter(
	initRoutes(mx, formatter)
	n.UseHandler(mx)	
	return n
  }</code>
  
  在服务器端返回JSON的值，用以进行下一步的数据调用和处理，我们对这个返回的JSON值可以用test.js 中使用Jquery的Ajax方法请求这个地址，在本地的console记录下得到的数据打开firefox的开发者工具，可以看到console记录下了返回的数据。
 
<code>func (dao *userInfoDao) Save(u *UserInfo) error {	
stmt, err := dao.Prepare(userInfoInsertStmt)	
checkErr(err)	
defer stmt.Close()
	res, err := stmt.Exec(u.UserName, u.DepartName, u.CreateAt)	
  checkErr(err)	
  if err != nil {		
  return err	
  }	
  id, err := res.LastInsertId()	
  if err != nil {		
  return err	
  }	
  u.UID = int(id)	
  return nil
  }</code>
  
  在dao文件里面明确了UserInfoDao应该处理的是db还是tx,这样就可以在外部修复其在容器性质上的差异。无论是哪一种形式，都可以对DaoSource进行初始化。如果err存在，就对其进行处理（处理方式与LastInsertId()有关系），否则就不进行有效的操作。
  
  再来讨论一下orm改写的程序与原程序的区别。其已经没有了自己编写dao的过程，而是直接在service完成对数据库的操作。通过xorm传入一个结构指针，xorm就能通过结构本身的信息来生成与结构相对应的table，或者查询结果通过这个结构来呈现。但是在大数据处理量和高负荷下，orm方式操作数据库会导致较大的性能损耗，对多线程的支持也不完美。观察一下两者的服务请求的接受百分比就可以发现：
  >1. non-orm
 <code>Percentage of the requests served within a certain time (ms)
  50%     28
  66%     39
  75%     47
  80%     53
  90%     67
  95%     81
  98%     97
  99%    104
 100%    126 (longest request)</code>
 > 2.orm
 <code>Percentage of the requests served within a certain time (ms)
  50%     27
  66%     41
  75%     48
  80%     54
  90%     71
  95%     86
  98%    113
  99%    124
 100%    144 (longest request)</code>
