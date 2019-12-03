---
title: 语法规则中文版
lang: zh-CN
---
# Costflow 语法规则

![v0.3](https://img.shields.io/badge/Costflow%20Syntax-v0.3-green)

[Costflow](https://www.costflow.io/) 是一个提升复式记账效率的工具，通过连接聊天工具与云存储的方式来解决现有复式记账记录麻烦和移动设备无法记录的问题。更多 Costflow 的介绍可以查看[这篇文章](https://blog.costflow.io/zh/introducing-costflow-zh/)。Costflow 语法则是其中解析文本到记账工具格式的规则。

例如发送下方内容给 Costflow 机器人：

```sh
Dinner #trip 200 bofa > trip
```

根据语法规则会被解析成下方内容：

```sh
2019-06-25 * "Dinner" #trip #costflow
  Assets:US:BofA                              -200.00 USD
  Expenses:Trip                               +200.00 USD
```

# 文档
- [中文简体](/zh/syntax/v0.3)
- [English](/syntax/)

# Playground
[https://playground.costflow.io](https://playground.costflow.io)

# Roadmap
[https://github.com/orgs/costflow/projects](https://github.com/orgs/costflow/projects)

# 更新日志
[更新日志](/zh/syntax/changelog)

