```typescript
const mysql = require('mysql') //调取服务

const connection = mysql.createConnection({ //创建数据连接
  host:'localhost',
  user:'root',
  password:'',
  database:'website'
})

connection.connect() //连接数据

const $sql= 'select * from websites' //sql语句

connection.query($sql,(err,results) => { //查询
  if(err){
    console.log('select Error --',err.message)
    return
  }
  console.log(results)
})
```