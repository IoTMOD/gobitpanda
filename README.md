<p align="center"><img src="https://gitlab.com/iotmod/gobitpanda/raw/master/logo.svg" width="360"></p>

# Go client for Bitpanda Global Exchange REST API
This is an unofficial Go client for the Bitpanda Global Exchange.
The official Bitpanda GE API documentation can be found [here](https://developers.bitpanda.com/exchange/)

**Warning! Test your software extensively before deploying it. Otherwise, you may lose a lot of money.**

# Coverage

### Account (secured)
* GET    /account/balances
* GET    /account/deposit/crypto/{currency_code}
* GET    /account/deposit/fiat/EUR
* POST   /account/deposit/crypto
* POST   /account/withdraw/crypto
* GET    /account/fees
* GET    /account/orders
* POST   /account/orders
* DELETE /account/orders
* GET    /account/orders/{orderId}
* DELETE /account/orders/{orderId}
* GET    /account/orders/{orderId}/trades
* GET    /account/trades
* GET    /account/trades/{trade_id}
* GET    /account/trading-volume

### Currencies
* GET /currencies

### Candlesticks
* GET /candlesticks/{instrument_code}

### Fees
* GET /fees

### Instruments
* GET /instruments

### Order-book
* GET /order-book/{instrument_code}

### Market-ticker
* GET /market-ticker
* GET /market-ticker/{instrument_code}

### Price-ticks
* GET /price-ticks/{instrument_code}

### Time
* GET /time


# Usage

### New client
API Key is only needed for the secured API calls

```go
import "gitlab.com/iotmod/gobitpanda"

c, err := gobitpanda.NewClient(gobitpanda.APIBase, YourAPIKey)
```

### Get balances of an account (secured)
```go
account, err := c.GetAccountBalances()
```

### Get deposit address for an account by currency code (only crypto currency allowed) (secured)
```go
deposit, err := c.GetAccountDepositAddress(gobitpanda.CurrencyMIOTA)
```

### Create a deposit address for an account by currency code (only crypto currency allowed) (secured)
```go
newDeposit, err := c.NewAccountDepositAddress(&gobitpanda.CurrencyCode{Code: gobitpanda.CurrencyMIOTA}) 
```

### Returns deposit information for sepa payments (secured)
```go
newFiatDeposit, err := c.NewAccountFIATDeposit()
```

### Withdraw from an account (only crypto currency allowed) (secured)
```go
withdraw, err := c.Withdrawl(&gobitpanda.Withdraw{Currency: gobitpanda.CurrencyMIOTA, Amount: "33", Recipient: gobitpanda.Recipient{Address: "999999999...", DestinationTag: ""}})
```

### Get fee details for an account (secured)
```go
fees, err := c.GetAccountFees()
```

### Set account fee mode (pay fees with BEST)
```go 
accountFees, err := c.SetAccountFeeMode(true)
```

### Get orders of an account (secured)
```go
now := time.Now()
orders, err := c.GetAccountOrders(now.AddDate(0, -1, 0), now, gobitpanda.InstrumentMIOTAEUR, true, true, "", "")
```

### Get order of an account by it's ID (secured)
```go
order, err := c.GetAccountOrderByID("e6753f5b-81fa-4b36-8b50-83db34cf9998")
```

### Create a new order (secured)
```go
err := c.NewOrder(&gobitpanda.CreateOrder{InstrumentCode: gobitpanda.InstrumentMIOTAEUR, Side: gobitpanda.OrderSideBuy, Type: gobitpanda.OrderTypeLimit, Amount: "125", Price: "0.08"})
```

### Close all orders or only orders in one market (secured)
All orders:
```go
orderIDs := c.CloseOrders()
```

Orders in one market:
```go
orderIDs := c.CloseOrders(gobitpanda.InstrumentMIOTAEUR)
```

###  Close order by it's ID (secured)
```go
err := c.CloseOrderByID("e6753f5b-81fa-4b36-8b50-83db34cf9998")
```

### Get trades of an account (secured)
```go
now := time.Now()
trades, err := c.GetAccountTrades(now.AddDate(0, -1, 0), now, gobitpanda.InstrumentMIOTAEUR, "", "")
```

### Get trade by it's ID (secured)
```go
trade, err := GetAccountTradeByID("f56e6c14-dfa9-1bcc-98cd-c9ca517c1607")
```

### Get trades by an order ID (secured)
```go
trades, err := GetAccountTradesByOrderID("e6753f5b-81fa-4b36-8b50-83db34cf9998")
```

### Get account's trading volume (secured)
```go
volume, err := c.GetAccountTradingVolume()
```

### Get candlesticks
```go
now := time.Now()
candlesticks, err := c.GetCandlesticks(gobitpanda.InstrumentMIOTAEUR, gobitpanda.UnitMinutes, gobitpanda.PeriodFifteenMinutes, now.AddDate(0, 0, -1), now)
```

### Get available currencies
```go
currencies, err := c.GetCurrencies()
```

### Get fees
```go
fees, err := c.GetFees()
```

### Get instruments
```go
instruments, err := c.GetInstruments()
```

### Get time
```go
time, err := c.GetTime()
```

### Get order book
```go
orderBook, err := c.GetOrderBook(gobitpanda.InstrumentMIOTAEUR, gobitpanda.LevelTwo)
```

### Get market statistics for the last 24 hours
```go
marketTicks, err := c.GetMarketTicker()
```

### Get statistics on a single market
```go
marketTick, err := c.GetMarketTickerByCode(gobitpanda.InstrumentMIOTAEUR)
```

### Get price-ticks information for last 4 hours when no query parameters are specified.

```go 
now := time.Now()
priceTicks, err := c.GetPriceTicksByCode(gobitpanda.InstrumentMIOTAEUR, now.AddDate(0, 0, -1), now)
```

# Bugs and feature requests
Please feel free to open a new issue or clone this repo, add your fixes/changes and create a pull request.
