```typescript
const mysql = require('mysql') 

const connection = mysql.createConnection({ 
  host:'localhost',
  user:'root',
  password:'',
  database:'website'
})

connection.connect() 

const $sql= 'update websites set name = ?,url=? where Id=?' 

const $sqlParmes =['系统管理','umeqy.com:17005',6]

connection.query($sql,(err,results) => { //查询
  if(err){
    console.log('select Error --',err.message)
    return
  }
  console.log(results)
  console.log(results.affecteRows)
})

connection.end()
```