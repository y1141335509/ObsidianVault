
## 1. 假设说你已经安装好了Elasticsearch和Kibana
### 1.1. 启动Elasticsearch
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

### 1.2. 启动Kibana
打开一个新的terminal，然后执行：
```bash
./kibana-8.12.0/bin/kibana
```
等启动好







































