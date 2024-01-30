---
title: 使用Cloudflare Wrangler部署调试Cloudflare Workers与Pages Functions
date: 2024-01-29 18:00:00
categories:
  - Category 1
tags:
  - Tag 1
---

# 安装Wrangler

1.`npm install -g @cloudflare/wrangler`
2.`wrangler login` 登录Cloudflare

# 创建项目

1. `wrangler init` 注意"这个方法将会在未来弃用

# 调试Workers

1. `wrangler dev`会在本地运行一个服务器来处理workers请求

## 使用本地的KV

### 添加值
`wrangler kv:key put <key> <value> --binding=<KV_NAMESPACE> --local`设置KV
例如`wrangler kv:key put "a" "b" --binding=AXOLOTL --local`
这需要你在`wrangler.toml`中设置`KV_NAMESPACE`  
例如添加:
```toml
[[kv_namespaces]]
binding = "AXOLOTL"
id = "b9054efdd25d46cfbbbc8b805f9816c0"
```
若`<value>`内含有空格,可以使用引号`"`!

## 调试定时任务(Workers Cron Triggers)

这需要你在`wrangler.toml`中设置`triggers`  
例如添加:  
```toml
[triggers]
crons = ["*/2 * * * *"]
```

`wrangler dev --test-scheduled`
然后访问`http://localhost:8787/__scheduled?cron=*+*+*+*+*`来运行这个定时任务  
`*+*+*+*+*`需要替换为你的定时任务的cron表达式 例如`*/2+*+*+*+*`  

# 调试Pages Functions
`wrangler pages dev ./ --kv STATUS_INFO=b9054efdd25d46cfbbbc8b805f9816c0`  
`./`表示目录地址
`--kv STATUS_INFO=b9054efdd25d46cfbbbc8b805f9816c0`表示绑定KV空间
