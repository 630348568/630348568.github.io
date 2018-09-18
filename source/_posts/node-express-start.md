---
title: node-express的搭建
date: 2018-09-17 11:02:07
author: T.stt
tags:
- nodejs
categories:
- tools---node
---
>node express搭建项目框架初始化搭建，查看文档学习-依据学习记录基础的操作（不断的积累学习）

## 学习地址
```bash
    http://www.expressjs.com.cn/
```
---

## 创建项目骨架
### 下载express-generator
```bash
    $ npm install express-generator -g
```

### 项目初始化
```bash
    $ express FirstApp -e （在当下目录下建立文件夹FirstApp）
    $ express -e (则在当下目录建立)
    或者是（只建package.json）
    $ mkdir myapp
    $ cd myapp
    $ npm init
```

### 进入项目安装依赖
```bash
    npm install
```
### 启动项目
```bash
    node ./bin/www // 没启动app.js是因为www文件中包含了
```
---

## 连接数据库
### 下载包
```bash
    $ cnpm install mysql
```

### 连接数据库
```bash
    var mysql      = require('mysql');
    var connection = mysql.createConnection({
    host     : 'localhost',
    user     : 'root',
    password : '123456',
    database : 'test'
    });
    
    connection.connect();
    
    connection.query('SELECT 1 + 1 AS solution', function (error, results, fields) {
    if (error) throw error;
    console.log('The solution is: ', results[0].solution);
    });
```
执行以上命令输出结果为：
$ node test.js
The solution is: 2

---

## 数据库操作
### 查询数据
```bash
var mysql  = require('mysql');  
var connection = mysql.createConnection({     
  host     : 'localhost',       
  user     : 'root',              
  password : '123456',       
  port: '3306',                   
  database: 'test', 
});
connection.connect();
var  sql = 'SELECT * FROM websites';
//查
connection.query(sql,function (err, result) {
    if(err){
        console.log('[SELECT ERROR] - ',err.message);
        return;
    }
    console.log('--------------------------SELECT----------------------------');
    console.log(result);
    console.log('------------------------------------------------------------\n\n');  
});
connection.end();
```

### 插入数据
```bash
var mysql  = require('mysql');
var connection = mysql.createConnection({     
  host     : 'localhost',       
  user     : 'root',              
  password : '123456',       
  port: '3306',                   
  database: 'test', 
}); 
connection.connect();
var  addSql = 'INSERT INTO websites(Id,name,url,alexa,country) VALUES(0,?,?,?,?)';
var  addSqlParams = ['菜鸟工具', 'https://c.runoob.com','23453', 'CN'];
//增
connection.query(addSql,addSqlParams,function (err, result) {
        if(err){
         console.log('[INSERT ERROR] - ',err.message);
         return;
        }        
 
       console.log('--------------------------INSERT----------------------------');
       //console.log('INSERT ID:',result.insertId);        
       console.log('INSERT ID:',result);        
       console.log('-----------------------------------------------------------------\n\n');  
});
connection.end();
```

### 更新数据
```bash
var mysql  = require('mysql');  
 
var connection = mysql.createConnection({     
  host     : 'localhost',       
  user     : 'root',              
  password : '123456',       
  port: '3306',                   
  database: 'test', 
}); 
connection.connect();
var modSql = 'UPDATE websites SET name = ?,url = ? WHERE Id = ?';
var modSqlParams = ['菜鸟移动站', 'https://m.runoob.com',6];
//改
connection.query(modSql,modSqlParams,function (err, result) {
   if(err){
         console.log('[UPDATE ERROR] - ',err.message);
         return;
   }        
  console.log('--------------------------UPDATE----------------------------');
  console.log('UPDATE affectedRows',result.affectedRows);
  console.log('-----------------------------------------------------------------\n\n');
});
connection.end();
```

### 删除数据
```bash
var mysql  = require('mysql');  
 
var connection = mysql.createConnection({     
  host     : 'localhost',       
  user     : 'root',              
  password : '123456',       
  port: '3306',                   
  database: 'test', 
}); 
 
connection.connect();
 
var delSql = 'DELETE FROM websites where id=6';
//删
connection.query(delSql,function (err, result) {
        if(err){
          console.log('[DELETE ERROR] - ',err.message);
          return;
        }        
 
       console.log('--------------------------DELETE----------------------------');
       console.log('DELETE affectedRows',result.affectedRows);
       console.log('-----------------------------------------------------------------\n\n');  
});
 
connection.end();
```