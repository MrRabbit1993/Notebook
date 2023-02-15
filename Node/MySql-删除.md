```typescript
const mysql = require('mysql')

const connection = mysql.createConnection({
  host:'localhost',
  user:'root',
  password:'',
  database:'website'
})

connection.connect()

const $sql = 'delete from websites where id=6'

connection.query($sql,(err,resultes)=>{
  if(err){
    console.log(err)
    return
  }
  console.log(resultes)
})

connection.end()
```