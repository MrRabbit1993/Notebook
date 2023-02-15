## 作用：
用于创建软连接，方便开发组件库时，拿其他项目来测试

1、在组件库目录下，执行 sudo npm link

例如：sudo npm link 

![IMAGE](resources/2873357D14B10441165636EA99CDC917.jpg =580x83)

在系统的node_module创建了一个组件（如1），映射到了组件库开发的文件目录（如2），形成了一个软连接

2、在测试的项目目录下执行 npm link CFileUpload
![IMAGE](resources/5751D05FF6CCC26BE01910962C2E7D0A.jpg =1442x273)

在测试项目的node_module下安装了一个组件（如1），映射到了系统的node_module中对应的组件（2），在根据软连接地址，映射到开发中组件实际的地址（3）、
解除link
解除项目和模块link，项目目录下，npm unlink 模块名称
解除模块全局link，模块目录下，npm unlink 模块名称
例如：npm unlink CFileUpload