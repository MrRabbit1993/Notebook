## mySqlConfig.js

```js
module.exports = function(){
const config={
    host:'localhost',
    user:'root',
    password:'',
    database:'website'
  }
  return config
}
```

## index.js
```js
const MySqlConfig = require('./mySqlConfig')

const mysql = require('mysql')

const _mySqlConfig = new MySqlConfig()

const connection = mysql.createConnection(_mySqlConfig)

connection.connect()

const $sql = 'insert into websites(Id,name,url,alexa,country) VALUES(0,?,?,?,?)'

const $sqlParms = ['数据库过来的','https://umeqy.com:17016','23453','CN']

connection.query($sql,$sqlParms,(err,results)=>{
  if(err){
    console.log('Error---',err.message);
    return
  }
  console.log(results)
})
connection.end()
```