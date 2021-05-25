
---
title:   API 文档

language_tabs: # must be one of https://git.io/vQNgJ
  - json

toc_footers:
  - <a href='https://www.hbg.com/zh-cn/apikey/'>创建 API Key </a>
includes:

search: true
---




# Coinw




## API简介

**欢迎使用CoinW API！**

此文档是CoinWAPI的唯一官方文档，CoinWAPI提供的能力会在此持续更新，请大家及时关注。

你可以通过点击上方菜单来切换获取不同业务的API，还可通过点击右上方的语言按钮来切换文档语言。

描述

1. 获取API认证的api key和secret key

> 申请API即可获得api key和secret key，其中api key是提供给API用户的访问密钥，secret key用于对请求参数签名的私钥。 注意： 请勿向任何人泄露这两个参数，这两个参数关乎账号安全。

2. 生成待签名字符串

>用户提交的参数除sign外，都要参与签名。 待签名字符串要求按照参数名进行排序（首先比较所有参数名的第一个字母，按abcd顺序排列，若遇到相同首字母,则看第二个字母, 以此类推。） 例如：对于如下的参数进行签名 `string[] parameters={"api_key=c821db84-6fbd-11e4-a9e3-c86000d26d7c"，"symbol=btc_cny"type=0"price=680"amount=1.0"}; 生成待签名字符串为：amount=1.0&api_key=c821db84-6fbd-11e4-a9e3-c86000d26d7c&price=680&symbol=btc_cny&type=0`

3. MD5签名

> 在MD5签名时,需要私钥secret key参与签名。 将待签名字符串添加私钥参数生成最终待签名字符串， 例如：amount=1.0&api_key=c821db84-6fbd-11e4-a9e3-c86000d26d7c&price=680&symbol=btc_cny&type=0&secret_key=secretKey 注意“&secret_key=secretKey” 为签名必传参数。 利用32位MD5算法 对最终待签名字符串进行签名运算,从而得到签名结果字符串(该字符串赋值于参数 sign)，MD5计算结果中字母全部大写。



联系我们

使用过程中如有问题或者建议，您可选择以下任一方式联系我们：

加入官方QQ群（967798979），进群时请注明UID 和编程语言。

## 基础API

获取币赢的交易市场基础信息。 其他api接口使用的symbol参数从此处获取，所以使用其他api前，请先通过此api获取必要的信息。

获取交易对摘要信息

检索交易所中列出的每个货币对的摘要信息

HTTP 请求

> GET /api/v1/public?command=returnTicker

`curl "http://api.coinw.pw/api/v1/public?command=returnTicker"`

**请求参数**

> 此接口不接受任何参数。

**返回字段**

|参数名称|   数据类型   |  描述    |
| ---- | ---- | ---- |
|id |string |id|
|last |string |最新成交价|
|lowestAsk  |string |最低卖价|
|highestBid |string|  最高买价|
|percentChange| string| 涨跌幅|
|isFrozen |string |是否冻结|
|high24hr|  string  |24小时最高价|
|low24hr  |string|  最低价|
|baseVolume |string|  基础币种24小时交易量|


<details>
<summary>Response示例</summary>

```
 {
                    "code":"200",
                    "data":{
                        "EET_CNYT":{
                            "percentChange":"0",
                            "high24hr":"0.0",
                            "last":"0.008",
                            "highestBid":"0.007",
                            "id":39,
                            "isFrozen":0
                            "baseVolume":"0.0",
                            "lowestAsk":"0.008",
                            "low24hr":"0.0"
                        }
                    },
                    "msg":"SUCCESS"
                }
```
</details>




​               

币种信息

返回币种的相关信息

HTTP 请求

GET/api/v1/public?command=returnCurrencies

curl "http://api.coinw.pw/api/v1/public?command=returnCurrencies"

请求参数

此接口不接受任何参数。

返回字段

参数名称  数据类型  描述
maxQty  string  最大提币量
minQty  string  最小提币量
recharge  string  是否能够充值，0不能1可以
symbol  string  币种
symbolId  string  币种符号
txFee string  手续费
withDraw  string  是否能够提现，0不能1可以
Response:




            {
                "code":"200",
                "data":{
                    "HC":{
                        "maxQty":"1000000.0",
                        "minQty":"10.0",
                        "recharge":"1",
                        "symbol":"HC",
                        "symbolId":"6",
                        "txFee":"0.0",
                        "withDraw":"1"
                    },
                    "LEEK":{
                        "maxQty":"999999.0",
                        "minQty":"1000.0",
                        "recharge":"1",
                        "symbol":"LEEK",
                        "symbolId":"55",
                        "txFee":"0.0",
                        "withDraw":"1"
                    },"XMR":{
                        "maxQty":"1000.0",
                        "minQty":"0.1",
                        "recharge":"1",
                        "symbol":"XMR",
                        "symbolId":"88",
                        "txFee":"0.0",
                        "withDraw":"1"
                    },"GLA":{
                        "maxQty":"9.99999999E8",
                        "minQty":"100.0",
                        "recharge":"0",
                        "symbol":"GLA",
                        "symbolId":"39",
                        "txFee":"0.0",
                        "withDraw":"0"
                        }
                    },
                    "msg":"SUCCESS"
                }

## 行情API

获取最新市场行情数据

获取深度

返回给定交易对的深度信息

HTTP 请求

GET /api/v1/public?command=returnOrderBook

curl "https://api.coinw.pw/api/v1/public?command=returnOrderBook&symbol=BTC_CNYT&size=20"

请求参数

参数名称  数据类型  必须  默认值 描述
size  Int false 5 深度大小 （5，20）
symbol  string  true    交易对如：BTC_CNYT
返回字段

参数名称  数据类型  描述
asks  string  买方深度
bids  string  卖方深度
Response:

                  {
                      "code":"200",
                      "data":{
                          "asks":[
                              ["3.263","0.01"],
                              ["7.90",0.01],
                              ["8.00","20.55"],
                              ["8.177","51.25"],
                              ["8.287","202.05"]
                          ],
                          "bids":[
                              ["3.262","302.00"],
                              ["3.261","233.10"],
                              ["3.26","34.00"],
                              ["3.259","416.90"],
                              ["3.258","113.50"]
                          ]
                      },
                      "msg":"SUCCESS"
                  }

交易对最近成交

返回给定交易对的过去200笔交易

HTTP 请求

GET /api/v1/public?command=returnTradeHistory

curl "https://api.coinw.pw/api/v1/public?command=returnTradeHistory&symbol=CWT_CNYT&start=1579238517000&end=1581916917660"

请求参数

参数名称  数据类型  必须  默认值 描述
symbol  string  true    交易对如：BTC_CNYT
start string  false   开始时间：UNIX时间戳
end string  false   结束时间：UNIX时间戳
返回字段

参数名称  数据类型  描述
id  string  交易ID
type  string  买卖类型
price string  单价
amount  string  总数量
total string  总金额
time  string  交易时间
Response:

                  {
                      "code":"200",
                      "data":[
                      {
                          "amount":49.2,
                          "total":160.5888,
                          "price":3.264,
                          "id":416253782,
                          "time":"2020-01-09 10:10:15",
                          "type":"buy"
                      },{
                          "amount":63.4,
                          "total":207.1278,
                          "price":3.267,
                          "id":416253778,
                          "time":"2020-01-09 10:10:15",
                          "type":"sell"
                          }
                      ],
                      "msg":"SUCCESS"
                  }


​              
K线数据

返回k线数据

HTTP 请求

GET /api/v1/public?command=returnChartData

curl "https://api.coinw.pw/api/v1/public?currencyPair=CWT_CNYT&command=returnChartData&period=1800&start=1580992380&end=1582288440"

请求参数

参数名称  数据类型  必须  默认值 描述
period  Int True    周期：单位秒，如：60，180，300, 900, 1800, 7200, 14400
currencyPair  String  True    交易对如：BTC_CNYT
start String  False   开始时间:UNIX时间戳
end String  False   结束时间:UNIX时间戳
返回字段

参数名称  数据类型  描述
date  string  时间
high  string  高
low string  低
open  string  开
close string  收
volume  string  成交量
Response:

```java
    {
      "code":"200",
      "data":[{
          "date":1.5809922E12,
          "volume":0.0,
          "high":8.803,
          "low":8.803,
          "close":8.803,
          "open":8.803
        },{
          "date":1.580994E12,
          "volume":0.0,
          "high":8.803,
          "low":8.803,
          "close":8.803,
          "open":8.803
        },{
          "date":1.5822864E12,
          "volume":121503.276,
          "high":9.587,
          "low":9.169,
          "close":9.4949,
          "open":9.4274
        },{
          "date":1.5822882E12,
          "volume":137447.694,
          "high":9.575,
          "low":9.191,
          "close":9.4599,
          "open":9.4949
        }
      ],
      "msg":"SUCCESS"
    }
```

热门币种成交量

获取热门市场24小时成交量

HTTP 请求

GET /api/v1/public?command=return24hVolume

curl "https://api.coinw.pw/api/v1/public?command=return24hVolume"

请求参数

此接口不接受任何参数。

返回字段

Response:

```java
    {
      "code":"200",
      "data":{
        "totalETH":"0",
        "USDT_CNYT":{
          "USDT":"9507323.2456",
          "CNYT":"66883008.0578"
        },
        "totalUSDT":"375022045.7914",
        "ETH_USDT":{
          "ETH":"0",
          "USDT":"0"
        },
        "totalBTC":"0",
        "CWT_CNYT":{
          "CWT":"6003449.28",
          "CNYT":"55189103.6876"
        },
        "BTC_CNYT":{
          "BTC":"46550.1319",
          "CNYT":"2516207980.3979"
        },
        "BTC_USDT":{
          "BTC":"0",
          "USDT":"0"
        },
        "ETH_CNYT":{
          "ETH":"0",
          "CNYT":"0"
        }
      },
      "msg":"SUCCESS"
    }
```

