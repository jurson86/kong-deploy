## 功能列表

### 接口注册
>网关对外开放的接口，通过restful接口添加

### 接口自动更新【注册中心】
>后端容器启停，ip地址变更后统一写入注册中心；网关需要不定期从注册中心拉取已经注册到本网关的服务最新地址；

### 反向代理
>网关基础功能：内部服务通过代理服务向外提供服务。

### 负载均衡
>代理网关通过负载均衡算法，来分配对多个目标服务访问的概率；  
>算法： 目标地址轮询，访问权重，最快响应优先，目标地址哈希；

### 健康检测 与 熔断
>健康检测方案：  检测地址轮询检测 、 无效访问概率检测     
>检测地址轮询检测： 通过健康检测地址，轮询检测服务健康状况，超过阀值会自动熔断，间歇检测重新激活；  
>无效访问率检测： 通过对无效访问率的统计，达到阀值后，自动熔断；

### 安全协议【https / http】
>SSL安全协议配置，对外提供HTTPS安全链接；

### 客户端限流 【流控中心】
>请求内容限流： 限制请求内容大小；  
>防刷限流： 单个用户单位时间内容请求量限制；

### 服务端限流【流控中心】
>可用性保障限流： 单个接口单位时间内允许最大访问量限制，如令牌桶机制；可通过流控中心进行令牌消耗量，监控>实时流量，并通过更改令牌数量来动态配置各接口最大流量；

### 权限认证【认证服务】
>权限认证： 用户密码认证，TOKEN认证等

### 服务跟踪【监控服务】
>调用链跟踪： 通过向调用链头部嵌入跟踪ID，调用服务落盘日志，统计分析从而跟踪整个调用链；


## 接口文档 【可直接参考官网】

### admin api  

##### 端口号 PORTS :  

8001 默认的管理端监听端口  
8001 is the default port on which the Admin API listens.  

8444 默认的https 管理端监听端口  
8444 is the default port for HTTPS traffic to the Admin API.

##### 支撑请求类型  Supported Content Types  

1、 application/x-www-form-urlencoded  
.e.g
<pre>
config.limit=10&config.period=seconds
</pre>

2、 application/json  
.e.g  
<pre>
{
    "config": {
        "limit": 10,
        "period": "seconds"
    }
}
</pre>


### 接口列表
<p>
  注：    所有接口示例 均采用 json 格式演示
</p>


1、kong节点信息查询  
GET     http://127.0.0.1:8001/ 
<pre>
响应结果：
{
    "hostname": "",
    "node_id": "6a72192c-a3a1-4c8d-95c6-efabae9fb969",
    "lua_version": "LuaJIT 2.1.0-alpha",
    "plugins": {
        "available_on_server": [
            ...
        ],
        "enabled_in_cluster": [
            ...
        ]
    },
    "configuration" : {
        ...
    },
    "tagline": "Welcome to Kong",
    "version": "0.11.0"
}
</pre>



2、kong节点状态查询  
GET     http://127.0.0.1:8001/status 
<pre>
响应结果：
{
  "database": {
    "reachable": true
  },
  "server": {
    "connections_writing": 1,
    "total_requests": 6,
    "connections_handled": 3,
    "connections_accepted": 3,
    "connections_reading": 0,
    "connections_active": 1,
    "connections_waiting": 0
  }
}
</pre>



3、注册接口   【查询 、删除接口 从官网查看】  
GET     http://127.0.0.1:8001/apis 
<pre>
请求体：
{
  "hosts": [
    "jgq-target",
    "tcc.order"
  ],
  "name": "jgqApiTarget",
  "upstream_url": "http://tcc.order",
  "retries": 3
}

响应结果：
{
  "created_at": 1521641586296,
  "strip_uri": true,
  "id": "ea1ff795-ce42-4062-b281-7283b7ab9f8b",
  "hosts": [
    "jgq-target",
    "tcc.order"
  ],
  "name": "jgqApiTarget",
  "http_if_terminated": false,
  "preserve_host": false,
  "upstream_url": "http://tcc.order",
  "upstream_connect_timeout": 60000,
  "upstream_send_timeout": 60000,
  "upstream_read_timeout": 60000,
  "retries": 3,
  "https_only": false
}
</pre>



3、更新接口  
GET     http://127.0.0.1:8001/apis/[name  or id]  
   .e.g:  http://127.0.0.1:8001/apis/jgqApiTarget    

<pre>
请求体：
{
  "hosts": [
    "tcc.order"
  ],
  "name": "jgqApiTarget",
  "upstream_url": "http://tcc.order",
  "retries": 3
}

响应结果：
{
  "created_at": 1521641586296,
  "strip_uri": true,
  "id": "ea1ff795-ce42-4062-b281-7283b7ab9f8b",
  "hosts": [
    "tcc.order"
  ],
  "name": "jgqApiTarget",
  "http_if_terminated": false,
  "preserve_host": false,
  "upstream_url": "http://tcc.order",
  "upstream_connect_timeout": 60000,
  "upstream_send_timeout": 60000,
  "upstream_read_timeout": 60000,
  "retries": 3,
  "https_only": false
}
</pre>









