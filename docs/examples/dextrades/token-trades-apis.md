---
sidebar_position: 5
---

# Token Trades API

## Historical Token Trades & Price API

DEXTrades API can give you historical trades. Let's see an example where we get trades of [BLUR Token](https://explorer.bitquery.io/ethereum/token/0x5283d291dbcf85356a21ba090e6db59121208b44) in the past. As you can see, we are using Block -> Time filter, which includes the time. If you want to filter by date, then use Block -> Date. You can also use Block -> Number if you want to filter based on block height. We are setting the `seller` and `buyer` to 1inch router [0x1111111254eeb25477b68fb85ed929f73a960582] to get both buys and sells of the BLUR token. 


```graphql
{
  EVM(dataset: combined, network: eth) {
    buyside: DEXTrades(
      limit: {count: 10}
      orderBy: {descending: Block_Time}
      where: {Trade: {Buy: {Currency: {SmartContract: {is: "0x5283d291dbcf85356a21ba090e6db59121208b44"}}, Seller: {is: "0x1111111254eeb25477b68fb85ed929f73a960582"}}}, Block: {Time: {since: "2023-03-03T01:00:00Z", till: "2023-03-05T05:15:23Z"}}}
    ) {
      Block {
        Number
        Time
      }
      Transaction {
        From
        To
        Hash
      }
      Trade {
        Buy {
          Amount
          Buyer
          Currency {
            Name
            Symbol
            SmartContract
          }
          Seller
          Price
        }
        Sell {
          Amount
          Buyer
          Currency {
            Name
            SmartContract
            Symbol
          }
          Seller
          Price
        }
      }
    }
    sellside: DEXTrades(
      limit: {count: 10}
      orderBy: {descending: Block_Time}
      where: {Trade: {Sell: {Currency: {SmartContract: {is: "0x5283d291dbcf85356a21ba090e6db59121208b44"}}, Buyer: {is: "0x1111111254eeb25477b68fb85ed929f73a960582"}}}, Block: {Time: {since: "2023-03-03T01:00:00Z", till: "2023-03-05T05:15:23Z"}}}
    ) {
      Block {
        Number
        Time
      }
      Transaction {
        From
        To
        Hash
      }
      Trade {
        Buy {
          Amount
          Buyer
          Currency {
            Name
            Symbol
            SmartContract
          }
          Seller
          Price
        }
        Sell {
          Amount
          Buyer
          Currency {
            Name
            SmartContract
            Symbol
          }
          Seller
          Price
        }
      }
    }
  }
}


```

Open the above query on GraphQL IDE using this [link](https://ide.bitquery.io/token-trades-both-buy-sell-1-inch).



## Latest Token Trades

To get the latest token trades you just need to sort by Block -> Time.

```
{
  EVM(dataset: combined, network: eth) {
    buyside: DEXTrades(
      limit: {count: 10}
      orderBy: {descending: Block_Time}
      where: {Trade: {Buy: {Currency: {SmartContract: {is: "0x5283d291dbcf85356a21ba090e6db59121208b44"}}}}}
    ) {
      Block {
        Number
        Time
      }
      Transaction {
        From
        To
        Hash
      }
      Trade {
        Buy {
          Amount
          Buyer
          Currency {
            Name
            Symbol
            SmartContract
          }
          Seller
          Price
        }
        Sell {
          Amount
          Buyer
          Currency {
            Name
            SmartContract
            Symbol
          }
          Seller
          Price
        }
      }
    }
    sellside: DEXTrades(
      limit: {count: 10}
      orderBy: {descending: Block_Time}
      where: {Trade: {Buy: {Currency: {SmartContract: {is: "0x5283d291dbcf85356a21ba090e6db59121208b44"}}}}}
    ) {
      Block {
        Number
        Time
      }
      Transaction {
        From
        To
        Hash
      }
      Trade {
        Buy {
          Amount
          Buyer
          Currency {
            Name
            Symbol
            SmartContract
          }
          Seller
          Price
        }
        Sell {
          Amount
          Buyer
          Currency {
            Name
            SmartContract
            Symbol
          }
          Seller
          Price
        }
      }
    }
  }
}

```

Open the above query on GraphQL IDE using this [link](https://graphql.bitquery.io/ide/latest-trades-for-a-token---both-buy-and-sell)



## Token trade from a specific DEX

If you are looking for token trades on a specific dex, use the following API as an example. Here we are getting [WETH Token](https://explorer.bitquery.io/ethereum/token/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2) trades from the Uniswap V3 DEX. You can also use the factory contract of Uniswap-like protocols in the DEX ->  OwnerAddress filter to get trades for that DEX.


```graphql
{
  EVM(dataset: combined, network: eth) {
    buyside: DEXTrades(
      limit: {count: 5}
      orderBy: {descending: Block_Time}
      where: {Trade: {Buy: {Currency: {SmartContract: {is: "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"}}}, Dex: {ProtocolName: {is: "uniswap_v3"}}}}
    ) {
      Block {
        Number
        Time
      }
      Transaction {
        From
        To
        Hash
      }
      Trade {
        Buy {
          Amount
          Buyer
          Currency {
            Name
            Symbol
            SmartContract
          }
          Seller
          Price
        }
        Sell {
          Amount
          Buyer
          Currency {
            Name
            SmartContract
            Symbol
          }
          Seller
          Price
        }
        Dex {
          ProtocolFamily
          ProtocolName
          SmartContract
          Pair {
            SmartContract
          }
        }
      }
    }
  }
}

```

Open the above query on GraphQL IDE using this [link](https://graphql.bitquery.io/ide/token-trades-for-a-specific-DEX_1).




## Subscribe to new token trades (Webhook)

You can use GraphQL subscription (Webhook) to subscribe to latest trades. In the following example we are subscribing to latest trades for [WETH Token](https://explorer.bitquery.io/ethereum/token/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2).



```graphql
subscription {
  EVM(network: eth, trigger_on: head) {
    buyside: DEXTrades(
      orderBy: {descending: Block_Time}
      where: {Trade: {Buy: {Currency: {SmartContract: {is: "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"}}}}}
    ) {
      Block {
        Number
        Time
      }
      Transaction {
        From
        To
        Hash
      }
      Trade {
        Buy {
          Amount
          Buyer
          Currency {
            Name
            Symbol
            SmartContract
          }
          Seller
          Price
        }
        Sell {
          Amount
          Buyer
          Currency {
            Name
            SmartContract
            Symbol
          }
          Seller
          Price
        }
      }
    }
    sellside: DEXTrades(
      orderBy: {descending: Block_Time}
      where: {Trade: {Buy: {Currency: {SmartContract: {is: "0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2"}}}}}
    ) {
      Block {
        Number
        Time
      }
      Transaction {
        From
        To
        Hash
      }
      Trade {
        Buy {
          Amount
          Buyer
          Currency {
            Name
            Symbol
            SmartContract
          }
          Seller
          Price
        }
        Sell {
          Amount
          Buyer
          Currency {
            Name
            SmartContract
            Symbol
          }
          Seller
          Price
        }
      }
    }
  }
}

```

Open the above query on GraphQL IDE using this [link](https://graphql.bitquery.io/ide/latest-token-trades-subscription)


## Getting OHLC and Distinct Buys/Sells 

The below query retrieve OHLC (Open, High, Low, Close) data and distinct buy/sell information on the Binance Smart Chain (BSC) on a daily basis. Adjust the parameters within the `where` and `Block` sections to customize the query for your specific needs, such as changing the token smart contract addresses or modifying the date range. 
In this query we have set trade currency pair to `0xfb6115445bff7b52feb98650c87f44907e58f802`( AAVE ) and `0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c`(WBNB), i.e AAVE/WBNB.

```
{
  EVM(dataset: combined, network: bsc) {
    buyside: DEXTradeByTokens(
      limit: {count: 30}
      orderBy: {descending: Block_Time}
      where: {Trade: {Side: {Currency: {SmartContract: {is: "0xfb6115445bff7b52feb98650c87f44907e58f802"}}, Amount: {ge: "0"}}, Currency: {SmartContract: {is: "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c"}}}, Block: {Date: {since: "2023-07-01", till: "2023-08-01"}}}
    ) {
      Block {
        Time(interval: {in: days, count: 1})
      }
      volume: sum(of: Trade_Amount)
      distinctBuyer: count(distinct: Trade_Buyer)
      distinctSeller: count(distinct: Trade_Seller)
      distinctSender: count(distinct: Trade_Sender)
      distinctTransactions: count(distinct: Transaction_Hash)
      total_sales: count(
        if: {Trade: {Side: {Currency: {SmartContract: {is: "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c"}}}}}
      )
      total_buys: count(
        if: {Trade: {Currency: {SmartContract: {is: "0xbb4cdb9cbd36b01bd1cbaebf2de08d9173bc095c"}}}}
      )
      total_count: count
      Trade {
        Currency {
          Name
        }
        Side {
          Currency {
            Name
          }
        }
        high: Price(maximum: Trade_Price)
        low: Price(minimum: Trade_Price)
        open: Price(minimum: Block_Number)
        close: Price(maximum: Block_Number)
      }
    }
  }
}


```

## Get Least Traded Token

The below query gets least traded tokens within a specified time frame on the Ethereum network. By querying the DEX trades and sorting them based on the least number of trades, it provides insights into tokens with minimal trading activity during the designated period.

```
query MyQuery {
  EVM(dataset: combined, network: eth) {
    DEXTradeByTokens(
      limit: {count: 10}
      where: {Block: {Time: {after: "2023-11-20T00:00:00Z", before: "2023-11-27T00:00:00Z"}}}
      orderBy: {ascendingByField: "count"}
    ) {
      Trade {
        Currency {
          Name
          SmartContract
        }
      }
      count
    }
  }
}

```