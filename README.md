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
