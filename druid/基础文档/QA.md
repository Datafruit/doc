### TASK insert后马上被remove

排除步骤：
1. 通过看overlord的Complete Tasks看有没failed的task
2. 如果有就看失败task具体日志

可能原因:
1. task目录（对应spec的basePersistDirectory）没创建，或没有权限

### df和du结果不一致
可能原因
部分文件被删除，但文件句柄没释放

排查步骤:
1. losf |grep deleted
2. 是否有进程外的文件删除操作


### task长时间不落地处理办法
原因：kafka client阻塞 overlord线程

1. 让task落地
找到taskId、task的ip和端口
先发如下请求，这个post会返回'{"0":61968201}'，把这个值作为下一个请求的数据
curl -X POST -H 'Content-Type:application/json' http://192.168.0.212:8100/druid/worker/v1/chat/lucene_index_kafka_wuxianjiRT_7e656d469ae7f8b_albdigkj/pause

curl -X POST -H 'Content-Type:application/json' -d '{"0":61968201}' http://192.168.0.212:8100/druid/worker/v1/chat/lucene_index_kafka_wuxianjiRT_7e656d469ae7f8b_albdigkj/offsets/end?resume=true

脚本批量处理：
```
import urllib2,json
url_temple='http://{0}/druid/worker/v1/chat/{1}/pause'
url_temple2='http://{0}/druid/worker/v1/chat/{1}/offsets/end?resume=true'
request = urllib2.Request('http://dev220.sugo.net:8090/druid/indexer/v1/runningTasks')
response = urllib2.urlopen(request)
tasks=json.loads(response.read())
for task in tasks:
    url = url_temple.format(task['location']['host'] + ":" + str(task['location']['port']), task['id'])
    print url
    request = urllib2.Request(url)
    response = urllib2.urlopen(request, '')
    offset = response.read();
    print offset
    url = url_temple2.format(task['location']['host'] + ":" + str(task['location']['port']), task['id'])
    print url
    request = urllib2.Request(url)
    request.add_header('Content-Type', 'application/json')
    response = urllib2.urlopen(request, offset)
    print response.read()

```

2. 处理完后更换kafka扩展包
