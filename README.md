<p>
    <a href="https://github.com/achannarasappa/ticker/releases"><img src="https://img.shields.io/github/v/release/achannarasappa/ticker" alt="Latest Release"></a>
    <a href="https://github.com/achannarasappa/ticker/actions"><img src="https://github.com/achannarasappa/ticker/workflows/test/badge.svg" alt="Build Status"></a>
    <a href='https://coveralls.io/github/achannarasappa/ticker?branch=master'><img src='https://coveralls.io/repos/github/achannarasappa/ticker/badge.svg?branch=master' alt='Coverage Status' /></a>
    <a href='https://goreportcard.com/badge/github.com/achannarasappa/ticker'><img src='https://goreportcard.com/badge/github.com/achannarasappa/ticker' alt='Report Card' /></a>
</p>

<h1 align="center">Ticker</h2>
<p align="center">
终端股票观察和股票仓位跟踪
</p>
<p align="center">
<img align="center" src="./docs/ticker.gif" />
</p>

## Features

* 实时股票报价
* 追踪您的股票仓位价值
* 支持多个成本基础批段cost basis lots
* 支持开市前和开市后的报价

## Install

Download the pre-compiled binaries from the [releases page](https://github.com/achannarasappa/ticker/releases) and copy to a location in `PATH` or see quick installs below

**mac**
```
brew install achannarasappa/tap/ticker
```

**linux**
```sh
curl -Ls https://api.github.com/repos/achannarasappa/ticker/releases/latest \
| grep -wo "https.*linux-amd64*.tar.gz" \
| wget -qi - \
&& tar -xf ticker*.tar.gz \
&& chmod +x ./ticker \
&& sudo mv ticker /usr/local/bin/
```

## Quick Start

```sh
ticker -w NET,AAPL,TSLA
```

## Usage
|Alias|Flag|Default|Description|
|-|-|-|-|
|  |--config|`~/.ticker.yaml`|配置监视列表和仓位|
|-i|--interval|`5`|刷新间隔（以秒为单位）|
|-w|--watchlist||逗号分隔的股票代码列表|
|  |--show-tags||显示每个报价的货币，交易所名称和报价延迟 |
|  |--show-fundamentals||显示开盘价、前收盘价和日波动范围 |
|  |--show-separator||每个报价之间有分隔符的布局|
|  |--show-summary||显示总日变化、总值和总值变化|
|  |--proxy||proxy URL for requests (default is none)|

## Configuration

Configuration is not required to watch stock price but is helpful when always watching the same stocks. Configuration can also be used to set cost basis lots which will in turn be used to show daily gain or loss on any position.
观察股票价格，配置并不必需，但总是有助于观察相同的股票。配置也可以用来设置成本基础批段，这可将用于显示每日盈利或损失的任何仓位。

```yaml
# ~/.ticker.yaml
show-tags: true
show-fundamentals: true
show-separator: true
interval: 10
proxy: http://localhost:3128
watchlist:
  - NET
  - TEAM
  - ESTC
  - BTC-USD
lots:
  - symbol: "ABNB"
    quantity: 35.0
    unit_cost: 146.00
  - symbol: "ARKW"
    quantity: 20.0
    unit_cost: 152.25
  - symbol: "ARKW"
    quantity: 20.0
    unit_cost: 145.35
```

* Symbols not on the watchlist that exists in `lots` will automatically be watched
* All properties in `.ticker.yaml` are optional
* `.ticker.yaml` can be set in user home directory, the current directory, or [XDG config home](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html)

### Display Options

With  `--show-summary`, `--show-tags`, `--show-fundamentals`, and `--show-separator` options set, the layout and information displayed expands:

<img src="./docs/ticker-all-options.png" />

## Notes

* **Real-time quotes** - Quotes are pulled from Yahoo finance which may provide delayed stock quotes depending on the exchange. The major US exchanges (NYSE, NASDAQ) have real-time quotes however other exchanges may not. Consult the [help article](https://help.yahoo.com/kb/SLN2310.html) on exchange delays to determine which exchanges you can expect delays for or use the `--show-tags` flag to include timeliness of data alongside quotes in `ticker`.
* **Cryptocurrencies**  - `ticker` supports any cryptocurrency Yahoo / CoinMarketCap supports. A full list can be found [here](https://finance.yahoo.com/cryptocurrencies?offset=0&count=100)
* **Non-US Symbols, Forex, ETFs** - The names for there may differ from their common name/symbols. Try searching the native name in [Yahoo finance](https://finance.yahoo.com/) to determine the symbol to use in `ticker`
* **Terminal fonts** - Font with support for the [`HORIZONTAL LINE SEPARATOR` unicode character](https://www.fileformat.info/info/unicode/char/23af/fontsupport.htm) is required to properly render separators (`--show-separator` option)

## Related Tools

* [tickrs](https://github.com/tarkah/tickrs) - real-time terminal stock ticker with support for graphing, options, and other analysis information
* [cointop](https://github.com/miguelmota/cointop) - terminal UI tracking cryptocurrencies
