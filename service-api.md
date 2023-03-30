#媒资推荐可被集成方案接口使用手册

## 概述
- 本文档描述媒资推荐可被集成方案的接口的使用方法，包括API 请求参数说明、响应参数说明及相应示例。
- 此版本，共包含4个服务接口(数据同步是一个服务，但是对于物料/用户/行为三类数据，接口请求相差较大，因此当成三个接口说明）：
  - 物料同步接口
  - 用户同步接口
  - 行为同步接口
  - 个性化服务请求接口
  - 监控metric采集接口


## 名词解释
- 物料库：物料存储，包括用户、物料等，用于同步外部输入的物料集合，并分发给下游各个推荐组件使用.
- 正排库：包含用户正排、物料正排库， 用于在线推荐实时请求调用

## 接口说明
### 物料同步接口
#### 接口说明
媒资物料内容同步接口, 用于批量同步物料库数据。
#### 接入URL(POST)
http://{{ip}}:{{port}}/oms/api/v3/omni/ms/sync/batch
#### http header
```
{"Content-Type": "application/json", "code": "cms", "id": "def_cms"}
```
#### 请求参数说明
```table
参数名 | 说明 | 类型 | 是否必填 |  说明
nid | 物料唯一标识id |String | 是
title | 物料标题|String | 是
display_run_time | 时长 |String | 否
country | 国家|String | 否
year | 年份 |String | 否
desc | 简介|String | 否
actors | 演员 |List string | 否
directors | 导演|List String | 否
view_point | 看点 |string | 否
score | 评分|string | 否
status | 状态，0下架，1上架 |int | 是
data_source | 所属运营商，CMCC移动、CUCC联通、CTC电信|String | 否
epi_type | 物料类型，program单剧集、series连续剧/系列剧、episode子集、channel频道、schedule节目单、album专栏、subject专题|String | 否
content_type | 内容类目，少儿或科技等 |string | 否
tags | 基础标签|list String | 否
op_tags | 运营标签 |list string | 否
show_flag | 展示标识|String | 否
valid_time | 生效时间 | int64 | 否 | 13位时间戳
expire_time | 过期时间 | int64 | 否 | 13位时间戳
publish_time | 发布时间 |int64 | 否 | 13位时间戳
operator_id | 运营商ID | string | 否
operator_name | 运营商名 | string | 否
provider_id | 供应商ID |String | 否
provider_name | 供应商名 |String | 否
is_free | 是否付费 | int | 否 | 1为免费、0为付费
channels | 所属频道 | list string | 否 | 对应频道，可用于推荐时按频道进行筛选
ts | 数据发送时的时间戳 |Long |是
```
#### 响应参数说明
```table
参数名 | 类型 | 是否必填 |  说明
success | boolean  | 是 | 成功标识
code | string  | 是| 响应码：0正常，其他异常
message | string  |  否 | 响应描述
result | dict  | 是| 响应数据
```

#### 请求示例
```json
[{
        "nid": "02000003000000012021110599002693",
        "title": "小行星： 新的太空黄金国",
        "display_run_time": 52,
        "country": "法国",
        "year": "2021",
        "desc": "\"小行星采矿\" 曾经被看作是科幻小说中的情节， 现在即将变为现实， 它或将成为稀有金属的解决答案， 而其是数字时代和能源转换非常必要的物质， 需求量在不断增大。 是什么还在阻止我们迎来全新的采矿时代？ 这个新黄金国的梦又对我们开发环境有着怎样的启示？",
        "actors": [
            "刘德华"
        ],
        "directors": [
            "张艺谋"
        ],
        "view_point": "是什么还在阻止我们迎来全新的采矿时代？",
        "score": "8.1",
        "epi_type": "vod",
        "content_type": "体育",
        "tags": [
            "刘德华",
            "姚明",
            "科比"
        ],
        "op_tags": [
            "爱情",
            "科技"
        ],
        "show_flag": "hd",
        "valid_time": 1609768813182,
        "expire_time": 1609768813182,
        "publish_time": 1609768013182,
        "operator_id": "CMCC",
        "operator_name": "移动",
        "provider_id": "iqiyi",
        "provider_name": "爱奇艺",
        "is_free": 1,
        "data_source": "BYD",
        "channels": ["科技"],
        "status": 1,
        "ts": 1609768813182
    }]
```

#### 响应示例
```json
{
    "success": true,
    "code": "0",
    "message": null,
    "result": null
}
```

### 用户同步接口
#### 接口说明
用户信息同步接口,可以实现用户库、正排库批量导入
#### 接入URL(POST)
http://{{ip}}:{{port}}/oms/api/v3/omni/ms/sync/batch
#### http header
```
{"Content-Type": "application/json", "code": "ums", "id": "def_ums"}
```
#### 请求参数说明
```table
参数名 | 类型 | 是否必填 |  说明
uid | string  | 是| 用户唯一标志
area | string  | 否| 北京、上海
gender | int | 否 | 性别
age | int | 否| 年龄
profession | string| 否| 职业
register_time | long | 否 | 注册时间
birth_year | long | 否 | 用户出生年份
avatar | string | 否 | 用户头像
labels | strings | 否 | 用户标签列表
ts | long | 否 | 数据发送时的时间戳 
```
#### 响应参数说明
```table
参数名 | 类型 | 是否必填 |  说明
success | boolean  | 是 | 成功标识
code | string  | 是| 响应码：0正常，其他异常
message | string  |  否 | 响应描述
result | dict  | 是| 响应数据
```
#### 请求示例
```json
[{
    "uid": "123",
    "city": "北京",
    "age": 18,
    "job": "student"
}]
```
#### 响应示例
```json
{
    "success": true,
    "code": "0",
    "message": null,
    "result": null
}
```
### 行为同步接口
#### 接口说明
推荐交互行为数据批量同步接口，适用于在线实时调用，回传用户行为
#### 接入URL(POST)
http://{{ip}}:{{port}}/oms/api/v3/omni/ms/sync/batch
#### http header
```
{"Content-Type": "application/json", "code": "ims", "id": "def_ims"}
```
#### 请求参数说明
```table
参数名 | 类型 | 是否必填 |  说明
nid | string  | 是| 物料标识nid
uid | string  | 否 | 用户标识uid
event | string  | 是 | 用户行为，行为类型：show(展现)、click(点击)、collect(收藏)、follow(关注)、like(点赞)、tv_playing(频道直播)、timeshift(频道时移)、vod_playing(VOD播放)、tvod_playing(回看播放)等， 行为类型填对应的英文即可， 括号为对应的中文描述，其他行为类型，也支持动态添加
traceid | string | 是 | 链路追踪id，traceid一般在调用推荐接口后，由推荐接口返回
duration | int | 否 | 播放时长 
ts | long  | 是 | 行为发生时间
```
#### 响应参数说明
```table
参数名 | 类型 | 是否必填 |  说明
success | boolean  | 是 | 成功标识
code | string  | 是| 响应码：0正常，其他异常
message | string  |  否 | 响应描述
result | dict  | 是| 响应数据
```
#### 请求示例
```json
[{
    "nid": "123",
    "uid": "u89",
    "event": "click",
    "ts": 1609768813182,
    "traceid": "demo"
}]
```
#### 响应示例
```json
{
    "success": true,
    "code": "0",
    "message": null,
    "result": null
}
```

### 个性化推荐接口
#### 接口说明
个性化推荐接口，结合用户历史行为，推荐当前用户可能感兴趣的物料集合
#### 接入URL(POST)
http://{{ip}}:{{port}}/airec/api/rec/p_rec
#### 请求参数说明
```table
参数名 | 类型 | 是否必填 |  说明
|字段 | 必选 | 类型 | 说明 |
|uid | 是 | string | 用户id，若未登录传传设备id |
|page_id | 否 | int | 默认一页 |
|req_cnt| 否 | int | 返回数量，默认为10条 |
|is_personalized | 否 | int | 是否使用个性化推荐，使用为1，不使用为0|
|channels | 否 | string | 频道推荐, 只返回这个频道下的物料,多个值逗号分割,如vod,体育 |
```
#### 响应参数说明
```table
|字段 | 必选 | 类型 | 说明 | 
|参数名 | 类型 | 是否必填 |  说明
|code | string  | 是| 响应码：0正常，其他异常
|message | string  |  否 | 响应描述
|trace_id | string  |  是 | 推荐一次请求的唯一标识
|result.item_list | list  |  是 | 返回的物料 nid列表
|result.size | int  | 是| 返回的物料数量
```
#### 请求示例
#### 请求示例
```json
{
    "uid": "xxxx-uid",
    "page_id": 0,     
    "req_cnt": 10
}

```
#### 响应示例
```json
{
"code": 0,
"message": "",
"trace_id": "215679725749670809788154736687724683293",
"result": {
	"size": 3,
	"item_list": ["13453611549650700466", "231359537527085656", "12235609175035395475"]
	}
}
```

### 监控接口
监控通过 Prometheus metric 标准协议对外暴露。用户只需要配置两组 url，就能实现采集。

1. 在线推荐接口 metric url —— gr-personal-rec:9090
2. 业务指标 metric url —— exporter:9090

配置示例：
在 prometheus 的 yaml 文件中：
```yaml
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'
    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.
    static_configs:
    - targets: ['exporter:9090']
    - targets: ['gr-personal-rec:9090']
```
