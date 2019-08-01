# Costflow Syntax
![v0.1](https://img.shields.io/badge/Costflow%20Syntax-v0.1-green)

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
- [English](https://docs.costflow.io/costflow-syntax/en)
- [中文简体](https://docs.costflow.io/costflow-syntax/zh)

# Playground
[https://playground.costflow.io](https://playground.costflow.io)

# Roadmap
[https://github.com/orgs/costflow/projects](https://github.com/orgs/costflow/projects)

# Changelog
## v0.1 (2019-07-09)
Features
- Date is optional, the default value is ‘today’ in your timezone;
- Currency/Commodity code is optional
- Account name replacements. E.g. bofa in your message will be replaced with Assets:US:BofA:Checking
- Get real time price for exchanging rate or stock, even cryptocurrency
- Simple transaction syntax
- Custom indent and line length


# Links
- [Early Access](https://www.costflow.io/)
- [Blog](https://blog.costflow.io/)
- [Docs](https://docs.costflow.io/)
- [Twitter](https://twitter.com/costflow)
- [Telegram Chanel](https://twitter.com/costflow)

