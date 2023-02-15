平台：Travis CI (<https://www.travis-ci.com/>)

npm cli 与npm install区别

 npm install 从package.json安装

npm cli 从package-lock.json安装，从package.json来验证是否一致，不一致抛出错误并停止运行，假如有yarn.lock文件，默认使用yarn安装

 ![](resources/EE6FCEC863252E8D369EBFDF9ACD8354.jpg) 

 ![](resources/F82831A9D7B436DB561C1BC42C716688.jpg)