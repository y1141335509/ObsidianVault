
## 1. 比较推荐的方法（Docker）
<span style="background:#fff88f">（假设已经安装了本地的docker。这种方法不需要在本地安装Elasticsearch和Kibana）</span> [原教程](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)
首先，创建docker network
```bash
docker network create elastic
```
（如果你之前已经创建了docker network，terminal会返回相关提示信息。

然后获取 Elasticsearch Docker Image：
```bash
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.13.2
```

接着启动Elasticsearch container：
```bash
docker run --name es01 --net elastic -p 9200:9200 -it -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.13.2
```
启动之后，你会看到一些列的token之类的信息。
<span style="background:#fff88f">注意，你需要在看到该信息之后的30min之内完成setup。否则需要重新生成相关token</span>。用来启动Elasticsearch container的terminal可以关闭，但不要关闭Docker。

接着，打开一个新的terminal，运行Kibana：
```bash
docker pull docker.elastic.co/kibana/kibana:8.13.2
```


然后开一个Kibana container：
```bash
docker run --name kib01 --net elastic -p 5601:5601 docker.elastic.co/kibana/kibana:8.13.2
```


然后你就可以打开localhost:5601端口进行登录了。登录所需要的信息都可以在刚刚打开的Elasticsearch image中找到。

注：
<span style="background:#fff88f">username 通常是 elastic</span>
<span style="background:#fff88f">password 可以在Elasticsearch image的logs信息里找到</span>









---
## 2. 假设说你已经安装好了Elasticsearch和Kibana
### 2.1. 启动Elasticsearch
现在你想要登录Elasticsearch，你需要执行这条命令：
```bash
./elasticsearch-8.12.0/bin/elasticsearch
```
（如果不行就执行·`./bin/elasticsearch`)
启动Elasticsearch之后，在新的terminal中执行：
```bash
curl -k -u USERNAME:PASSWORD https://localhost:9200
```
上面的`USERNAME`通常叫做`elastic`；
`PASSWORD`：
* 如果你是第一次使用Elasticsearch，它会给你一个自动生成的密码
* 如果你是第二次使用（已经安装过Elasticsearch）且在第一次的时候更改了密码，那么这里的`PASSWORD`就是你自己设置的密码

### 2.2. 启动Kibana
打开一个新的terminal，然后执行：
```bash
./kibana-8.12.0/bin/kibana
```
等启动好







































