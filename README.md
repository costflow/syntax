# Costflow Syntax

Syntax for transforming plain text message into Beancount/Ledger/hledger format by [Costflow](https://www.costflow.io/).

## Beancount

V0.0.1

General Syntax:

[YYYY-MM-DD] [Command] [Strings] [#Tags] [^Links] [Accounts] [Amount] [Commodity]

### Date
Date is optional, the default value is the day we receive the message.

### Command
Command is optional, it can be a symbol or directive. If no command is specified, the message will treated as 'Completed transaction'.

Supported Commands | Usage
------------ | -------------
\*        | Completed transaction.
!         | Incomplete transaction.
//        | Comment. The full message will be saved to Costflow, but will not be appended to your ledger file.
;         | Comment. The full message will be saved to Costflow and appended to your ledger file.
open      | Open an account.
close     | Close an account.
commodity | Commodity directive. [Beancount document](https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.a3si01ejc035).
price     | Price directive. [Beancount document](https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.f78ym1dxtemh).
balance   | Balance directive. [Beancount document](https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.l0pvgeniwvq8).
note      | A Note directive is simply used to attach a dated comment to the journal of a particular account. [Beancount document](https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.c4cyaa6o6rqm).
event     | Event directive.[Beancount document](https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.tm5fxddlik5x).
pad       | Pad directive. [Beancount document](https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.aw8ic3d8k8rq).


### Strings
The String is the words between Command and next attribute. If there is only one String you want to use, the double-quotes is optional.

### Tags
Transaction tags. Optional. [Beancount document](https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.oivvp5olom2v).

### Links
Transaction links. Optional. [Beancount  document](https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.k4v5vkjukel7).

### Accounts
You can use the abbreviations which created on Costflow. 

### Amount


### Commodity


---

## Ledger
// to do

## hledger
// to do
