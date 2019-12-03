---
title: Costflow Syntax
lang: en-US
---

# Costflow Syntax
![v0.3](https://img.shields.io/badge/Costflow%20Syntax-v0.3-green)

[Costflow](https://www.costflow.io/) is a productivity tool for plain text accounting. By connecting messaging apps with cloud storage services, it can be much easier to create new directives/entries, especially on mobile devices. You can get more details in [this post](https://blog.costflow.io/introducing-costflow/).

Costflow Syntax is the rule for parsing plain text to the accounting tools formats, such as Beancount, Ledger and hledger, etc.

For example, send the message below to Costflow bots:

```javascript
Dinner #trip 200 bofa > trip
````

Costflow will parse it to the content below (in Beancount format):

```javascript
2019-06-25 * "Dinner" #trip #costflow
  Assets:US:BofA                              -200.00 USD
  Expenses:Trip                               +200.00 USD
```

# Docs
- [English](https://docs.costflow.io/syntax/v0.3)
- [中文简体](https://docs.costflow.io/zh/syntax/)

# Playground
[https://playground.costflow.io](https://playground.costflow.io)

# Roadmap
[https://github.com/orgs/costflow/projects](https://github.com/orgs/costflow/projects)

# Changelog
[Costflow Syntax changelog](https://docs.costflow.io/syntax/changelog)
