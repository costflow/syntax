# Costflow 语法
![v0.1](https://img.shields.io/badge/Costflow%20Syntax-v0.1-green)

[Costflow](https://www.costflow.io/) 是一个提升复式记账效率的工具，通过连接聊天工具与云存储的方式来解决现有复式记账记录麻烦和移动设备无法记录的问题。更多 Costflow 的介绍可以查看[这篇文章](https://blog.costflow.io/zh/introducing-costflow-zh/)。Costflow 语法则是其中解析文本到记账工具格式的规则。

例如发送下方内容给 Costflow 机器人：

```javascript
Dinner #trip 200 bofa > trip
```

根据语法规则会被解析成下方内容：

```javascript
2019-06-25 * "Dinner" #trip #costflow
  Assets:US:BofA                              -200.00 USD
  Expenses:Trip                               +200.00 USD
```

# 文档
- [中文简体](https://docs.costflow.io/costflow-syntax/zh)
- [English](https://docs.costflow.io/costflow-syntax/en)

# Playground
[https://playground.costflow.io](https://playground.costflow.io)

# 更新日志
## v0.1 (2019-07-09)
语法特点
- 可选输入日期，默认为今天；
- 货币代码可省略；
- 账户名替换，例如用 bofa 代替 Assets:US:BofA:Checking；
- 自动获取当前货币汇率、加密货币和股票的价格；
- 极简交易语法；
- 自定义缩进长度以及每行对齐长度；

# 相关链接
- [Early Access](https://www.costflow.io/)
- [Blog](https://blog.costflow.io/)
- [Docs](https://docs.costflow.io/)
- [Twitter](https://twitter.com/costflow)
- [Telegram Chanel](https://twitter.com/costflow)
