# Costflow Syntax

[中文简体](./zh)

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
  Equity:Trip                                 +200.00 USD
```



Costflow Syntax and Costflow Parser will open source on [https://github.com/costflow/syntax](https://github.com/costflow/syntax) and [https://github.com/costflow/parser](https://github.com/costflow/parser) . Your ideas and contributions are welcome.



# Costflow Syntax v0.1

Costflow Syntax v0.1 is the latest version and Beancount is the only output format supported. All the following content based on this version.



## Features

- Date is optional, the default value is ‘today’ in your timezone;
- Currency/Commodity code is optional;
- Account name replacements. E.g. bofa in your message will be replaced with Assets:US:BofA:Checking;
- Get real time price for exchanging rate or stock, even cryptocurrency;
- Simple transaction syntax;
- Custom indent and line length;



## Commands

The 'command' is the first word/symbol of the texts (exclude date). Some commands are same with 'directives' in Beancount.

| Command                 | Description                                                  |
| ----------------------- | ------------------------------------------------------------ |
| [*](#transaction)       | Completed transaction. Optional.                             |
| [!](#transaction)       | Incomplete transaction.                                      |
| [;](#comment)           | Comment. It will be saved to your ledger file.               |
| [//](#comment)          | Comment. It will only be saved to Costflow, **not** your ledger file. |
| [open](#open)           | Open directive.                                              |
| [close](#close)         | Close directive.                                             |
| [commodity](#commodity) | Commodity directive.                                         |
| [option](#option)       | Option directive.                                            |
| [note](#note)           | Note directive.                                              |
| [balance](#balance)     | Balance directive.                                           |
| [pad](#pad)             | Pad directive.                                               |
| [price](#price)         | Price directive.                                             |
| [$](#snap)              | Get the latest exchange rate between currencies or price of cryptocurrencies or stocks. It will not be saved to your ledger file, just return the result. |
| [event](#event)         | Event directive.                                             |
|                         | If no command found and<br/>If message contains numbers, then the command will be *;<br/>Otherwise, the command will fall back to //。 |



## General Rules

To make the syntax easier, users will be asked to add the following fileds to config:

- Output format, this version only support Beancount;
- Users' timezone;
- Default currency code;
- Indent length and line length;
- Text replacements for account names;



Costflow Syntax v0.1 Rules:

- The date is optional. If no YYYY-MM-DD format date found, the date will be 'today' in your timezone. Use '2019-07-01' in the following examples;

- Commodity/Currency code is optional, too, the default value is what you set before. Use 'USD' in following examples;

- Metadata is not supported in v0.1;

- Inline comment is not supported in v0.1;

- Commodity/Currency should be a word all in capital letters;

- The abbreviation of an account name should **not** be a word all in capital letters or numbers; These  replacements might be used in the following examples.

  ```javascript
  eob: Equity:Opening-Balances
  bofa: Assets:US:BofA:Checking
  rx: Assets:Receivables:X
  ry: Assets:Receivables:Y
  boc: Assets:CN:BOC
  cmb: Liabilities:CreditCard:CMB
  food: Expenses:Food
  phone: Expenses:Home:Phone
  rent: Expenses:Home:Rent
  ```

  

##  Transaction

Transactions are the most common type of directives that occur in a ledger. Costflow Syntax support two methods for transactions.

### First: >

The first one uses greater-than sign, it means the flow direction from its left to its right. The general format is:

```javascript
[YYYY-MM-DD] [Flag] ["Payee"] ["Narration"] [#tag] [^link] AMOUNT [COMMODITY] FROM_ACCOUNT > [AMOUNT] [COMMODITY] TO_ACCOUNT
```

The elements in square brackets are optional. Rules details:

- The date is optional. If no YYYY-MM-DD format date found, the date will be 'today' in your timezone;

- The flag is used to indicate the status of a transaction, * means completed transaction, ! means incomplete transaction. * can be omitted;

- Payess and Narration are easy to use, but the syntax is a little bit complex:
  - Compatible with Beancount double quotes format: if there is only one double-quotes string, it is a narration; If there are two double-quotes strings, the first one is a payee and the second one is narration. Not support the legacy pipe symbol format;
  - Double-quotes can be omitted in these cases:
    - Input narration directly without double-quotes, but the narration **cannot** contain numbers. E.g., 'Xbox 360 Game' is not a valid narration;
    - Payee can be used in @Payee format. In this rule, payee **cannot** contain blank spaces, but payee can be connected with - or _. E.g., @KFC and @Burger_King are all valid payees;
    - If you only want to record payee, just input @Payee, no need to concern narration;
  - Tag: strings start with # are treated as tags;
  - Link: strings start with ^ are treated as links;
  - In the first transaction syntax, the order of AMOUNT / COMMODITY / ACCOUNT should be:
    1. AMOUNT. Because > means the flow direction, so the default amounts in left side are negative, the minus sign can be omitted;
    2. COMMODITY. Optional, the default value is what you set before;
    3. ACCOUNT. Accounts can be the full name in your ledger file, or they can be the abbreviations you set before.
  - The right side rules are the same with the left side, except the default amounts are positive;
  - [Costs and prices](https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.mtqrwt24wnzs) are same with Beancount;
  - If there are multiple accounts in either side, use + to connect, Be aware, only one > allowed in a message;
  - The amounts in the right side can be omitted, they are equal to **sum*(left side amounts)/(right side accounts number)**. Useful when splitting expenses with friends.
  
  
  
  Examples, finally!
  
  ```javascript
  // #1 Traditional format with >
  2017-01-05 "RiverBank Properties" "Paying the rent" 2400 Assets:US:BofA:Checking > 2400  Expenses:Home:Rent
  // #1 Output
  2017-01-05 * "RiverBank Properties" "Paying the rent"
    Assets:US:BofA:Checking                        -2400.00 USD
    Expenses:Home:Rent                             +2400.00 USD
  
  
  // #2 No date, no commodity symbol, use @Payee
  @Verizon 59.61 Assets:US:BofA:Checking > Expenses:Home:Phone
  // #2 Output
  2019-07-01 * "Verizon" ""
    Assets:US:BofA:Checking                          -59.61 USD
    Expenses:Home:Phone                              +59.61 USD
  
  
  // #3 Use text replacements
  @Verizon 59.61 bofa > phone
  // #3 Output is exactly same with #2
  
  
  // #4 Multiple accounts flow to one
  Rent 750 cmb + 750 boc > rent
  // #4 Output
  2019-07-01 * "Rent"
    Liabilities:CreditCard:CMB                     -750.00 USD
    Assets:CN:BOC                                  -750.00 USD
    Expenses:Home:Rent                            +1500.00 USD
  
  
  // #5 One account flows to multiple accounts. The scene in this example is you have dinner with your friends, you pay the bill for your friends(x and y), then your friends will pay you back.
  Dinner 180 CNY bofa > rx + ry + food
  // #5 Output
  2019-07-01 * "Dinner"
    Assets:Receivables:X                            +60.00 CNY
    Assets:Receivables:Y                            +60.00 CNY
    Expenses:Food                                   +60.00 CNY
    Liabilities:CreditCard:CMB                     -180.00 CNY
  
  
  // #6 Transaction with cost
  Transfer to account in US 5000 CNY @@ 726.81 USD boc > 726.81 bofa
  // #6 Output
  2019-07-01 * "Transfer to account in US"
    Assets:CN:BOC                                 -5000.00 CNY @@ 726.81 USD
    Assets:US:BofA:Checking                        +726.81 USD
  ```
  
  

### Second: |

The second method uses the pipe sign (U+007C) instead of >, it means "start a new line", the timing is the same with Beancount. The syntax detail:

```javascript
[YYYY-MM-DD] [Flag] ["Payee"] ["Narration"] [#tag] [^link] | ACCOUNT_A [COMMODITY] AMOUNT | ACCOUNT_B [COMMODITY] AMOUNT
```

As we can see, the usage of Date, Flag, Payee, Narration, Tag, Link are all same with the first one. The differences:

- Should insert a | before each account;
- The order of ACCOUNT / COMMODITY / AMOUNT is different from the first one but same with Beancount;
- The second transaction syntax has no flow direction, so the minus sign and amounts cannot be omitted;

We can rewrite the examples with |:

```javascript
// #1 Traditional format with |
2017-01-05 "RiverBank Properties" "Paying the rent" | Assets:US:BofA:Checking -2400 | Expenses:Home:Rent 2400
// #1 Output
2017-01-05 * "RiverBank Properties" "Paying the rent"
  Assets:US:BofA:Checking                        -2400.00 USD
  Expenses:Home:Rent                             +2400.00 USD


// #2 No date, no commodity symbol, use @Payee
@Verizon | Assets:US:BofA:Checking -59.61 | Expenses:Home:Phone 59.61
// #2 Output
2019-07-01 * "Verizon" ""
  Assets:US:BofA:Checking                          -59.61 USD
  Expenses:Home:Phone                              +59.61 USD


// #3 Use text replacements
@Verizon | bofa -59.61 | phone 59.61
// #3 Output is exactly same with #2


// #4 Multiple accounts flow to one
Rent | cmb -750 | boc -750 | rent 1500
// #4 Output
2019-07-01 * "Rent"
  Liabilities:CreditCard:CMB                     -750.00 USD
  Assets:CN:BOC                                  -750.00 USD
  Expenses:Home:Rent                            +1500.00 USD


// #5 One account flows to multiple accounts. The scean in this example is you have dinner with your friends, you pay the bill for your friends(x and y), then your friends will pay you back.
Dinner | bofa 180 CNY | rx -60 | ry -60 | food -60
// #5 Output
2019-07-01 * "Dinner"
  Assets:Receivables:X                            +60.00 CNY
  Assets:Receivables:Y                            +60.00 CNY
  Expenses:Food                                   +60.00 CNY
  Liabilities:CreditCard:CMB                     -180.00 CNY


// 示例六：记录 Cost/Price
Transfer to account in US | boc -5000 CNY @@ 726.81 USD  | bofa +726.81
// #6 Output
2019-07-01 * "Transfer to account in US"
  Assets:CN:BOC                                 -5000.00 CNY @@ 726.81 USD
  Assets:US:BofA:Checking                        +726.81 USD


```



## <a name="comment"></a>Comment

; and // are easy to use as comments, just type anything after them. 

Examples:

```javascript
; I paid and left the taxi, forgot to take change, it was cold.
// to do: cancel Netflix subscription
```

The difference is that comments with ; will be saved to Costflow first, then appended to the ledger file, comments with // will only be saved to Costflow. // is useful for reminding yourself to do some financial actions.



## <a name="open"></a>Open

Same with Beancount. Do not support metadata in this version.

```javascript
[YYYY-MM-DD] open ACCOUNT
```



Examples:

```javascript
// Input
open Assets:US:BofA
// Output
2019-07-01 open Assets:US:BofA
```



## <a name="close"></a>Close

Same with Beancount.

```javascript
// Input
close Assets:US:BofA
// Output
2019-07-01 close Assets:US:BofA
```



## <a name="commodity"></a>Commodity

Same with Beancount. Do not support metadata in this version.

```javascript
[YYYY-MM-DD] commodity SYMBOL
```



Examples:

```javascript
// Input
commodity BTC
// Output
2019-07-01 commodity BTC
```



## <a name="option"></a>Option

Syntax:

```javascript
option [NAME] VALUE
```



Explanation:

NAME and VALUE usually are double-quotes strings, but in the following cases double-quotes can be omitted:

- If NAME is "title", just input option VALUE, VALUE does not need double-quotes;
- If VALUE is one of currency codes in [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217), the NAME can be omitted (will be parsed to operating_currency), VALUE does not need double-quotes;



Examples:

```javascript
// Input
option Example Costflow file
// Output
option "title" "Example Costflow file"

// Input
option CNY
// Output
option "operating_currency" "CNY"

// Input
option "conversion_currency" "NOTHING"
// Output
option "conversion_currency" "NOTHING"

```



## <a name="note"></a>Note

Syntax:

```javascript
[YYYY-MM-DD] note ACCOUNT DESCRIPTION
```



Explanation:

- ACCOUNT can be the full name in your ledger file, or they can be the abbreviations you set before;
- DESCRIPTION can be any strings, no need double-quotes.



Examples

```javascript
// Input
note bofa Called about fraudulent card.
// Output
2019-07-01 note Assets:US:BofA:Checking "Called about fraudulent card."
```



## <a name="balance"></a>Balance

Syntax:

```javascript
[YYYY-MM-DD] balance ACCOUNT AMOUNT [COMMODITY]
```



Explanation:

- ACCOUNT can be the full name in your ledger file, or they can be the abbreviations you set before;
- COMMODITY  is optional, the default value is what you set before;



Examples:

```javascript
// Input
balance bofa 360
// Output
2019-07-01 balance Assets:US:BofA:Checking 360 USD
```



## <a name="pad"></a>Pad

Syntax:

```javascript
[YYYY-MM-DD] pad ACCOUNT ACCOUNT_PAD
```



Explanation:

- ACCOUNT and ACCOUNT_PAD can be the full name in your ledger file, or they can be the abbreviations you set before;
- Use pad carefully. Check [Beancount docs](https://docs.google.com/document/d/1wAMVrKIA2qtRGmoVDSUBJGmYZSygUaR0uOMW1GV3YE0/edit#heading=h.aw8ic3d8k8rq).



Examples:

```javascript
// Input
pad bofa eob
// Output
2019-07-01 pad Assets:US:BofA:Checking Equity:Opening-Balances
```



## <a name="price"></a>Price

Syntax:

```javascript
[YYYY-MM-DD] price COMMODITY_A [PRICE] [COMMODITY_B]
```



Explanation:

- COMMODITY_B  is optional, the default value is what you set before;
- Set the PRICE manually: just use a number as PRICE like Beancount;
- If there is no PRICE found, Costflow will automatically get the latest exchange rates of currencies, or the latest price of cryptocurrencies and stock (by Alpha Vantage](https://www.alphavantage.co/). The priority:
  1. Firstly, Costflow will check whether COMMODITY_A is one of [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)  currency codes. If it is, get the exchange rate;
  2. Secondly, Costflow will check whether COMMODITY_A is one of cryptocurrency codes that Alpha Vantage support ([support list](https://www.alphavantage.co/digital_currency_list/)). If it is, get the cryptocurrency price;
  3. Finally, Costflow will treat COMMODITY_A as a stock code and get its price. Only stocks listed on NASDAQ and NYSE are supported in this version. Note, COMMODITY_B **should not** be set, if COMMODITY_A is a stock code, COMMODITY_B is inherited from the stock market. 



Examples:

```javascript
// #1 Input
2017-01-17 price USD 1.08 CAD
// #1 Output
2017-01-17 price USD 1.08 CAD

// #2 Input, get the exchange rate automatically. The word "to" is only for understanding, can be omitted.
price CAD to USD
// #2 Output
2019-07-01 price 

// #3 Input, get Bitcoin price
price BTC
// #3 Output
2019-07-01 price BTC 11946.64 USD

// #4 Input, get Apple stock price
price AAPL
// #4 Output
2019-07-01 price AAPL 199.8 USD

```



## <a name="calculate"></a>$

$ is for getting the latest exchange rate of currencies or the latest price of cryptocurrencies and stocks. The result will be returned directly, **will not** be saved to the ledger file. The syntax:

```javascript
$ [AMOUNT] COMMODITY_A [to] COMMODITY_B
```



Explanation:

- COMMODITY_B  is optional, the default value is what you set before;
- AMOUNT is optional, the default amount is 1;
- The word "to" is only for understanding, can be omitted;
- The priority is the same with Price:
  1. Firstly, Costflow will check whether COMMODITY_A is one of [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217)  currency codes. If it is, get the exchange rate;
  2. Secondly, Costflow will check whether COMMODITY_A is one of cryptocurrency codes that Alpha Vantage support ([support list](https://www.alphavantage.co/digital_currency_list/)). If it is, get the cryptocurrency price;
  3. Finally, Costflow will treat COMMODITY_A as a stock code and get its price. Only stocks listed on NASDAQ and NYSE are supported in this version. Note, COMMODITY_B **should not** be set, if COMMODITY_A is a stock code, COMMODITY_B is inherited from the stock market. 



Examples:

```javascript
// Input, get the exchange rate automatically. The word "to" is only for understanding, can be omitted.
$ CAD to USD
// Return
1 CAD = 0.7637 USD

// Input, get 10 Bitcoin price
$ 10 BTC
// Return
10 BTC = 119466.4 USD

// Input, get Apple stock price
$ AAPL
// Return
AAPL 199.8 USD (-0.030%)

```



## <a name="event"></a>Event

Syntax:

```javascript
[YYYY-MM-DD] event NAME VALUE
```



Explanation:

- NAME and VALUE can use double-quotes like Beancount;
- If no double-quotes found, the first word (exclude date) will be saved as NAME, the rest part will be saved as VALUE;



Examples:

```javascript
// Input, traditional Beancount format
2017-01-02 event "location" "Paris, France"
// Output
2017-01-02 event "location" "Paris, France"

// Input, without double-quotes
event location Paris, France
// Output
2017-01-02 event "location" "Paris, France"
```



# Links

- [Costflow]([https://costflow.io](https://www.costflow.io/))
- [Costflow Blog](https://blog.costflow.io/)
- [Costflow Parser](https://github.com/costflow/parser)
- [Syntax Issues](https://github.com/costflow/syntax/issues)
- [Twitter](https://twitter.com/costflow)
- [Telegram Chanel](https://twitter.com/costflow)


