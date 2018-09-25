# Bition-exchange-api
## 1.目录
* /open/api/all_order —— 获取全部委托

* /open/api/all_trade —— 获取全部成交记录

* /open/api/cancel_order —— 取消挂单

* /open/api/common/symbols —— 查询系统支持的所有交易对及精度

* /open/api/create_order —— 下单

* /open/api/get_records —— 获取K线

* /open/api/get_ticker —— 获取当前行情

* /open/api/get_trades —— 获取市场成交记录

* /open/api/market_dept —— 查询买卖盘深度

* /open/api/market —— 获取各个交易对的最新成交价

* /open/api/new_order —— 获取当前委托

* /open/api/order_info —— 获取订单详情

* /open/api/user/account —— 获取资产信息

## 2.地址
```https://openapi.bition.pro/exchange-open-api```

## 3.通用规则
签名：

请求参数按照字典排序，然后以keyvalue的形式拼接成字符串string，最后sign=MD5(string+secretKey)。注意：如果请求参数中value为NULL的情况，则在拼接字符串时不计入签名字符串。

例如：参数如下：
```
{
	country = 86;
	mobile = 15882133579;
	password = 654321zz;
	time = 1516007245;
}
```
 
按照参数首字母排序拼接后：
```
string = country86mobile15882133579password654321zztime1516007278

sign=MD5(string+secretKey)
```
## 4.post请求参数采用表单格式提交数据
```
content-type:application/x-www-form-urlencoded
```

## 5.错误码
|错误码|说明|
|:---|:---|
|0|成功|
|5|下单失败|
|6|数量小于最小值|
|7|数量大于最小值|
|8|订单取消失败|
|9|交易被冻结|
|13|程序错误|
|19|余额不足|
|22|订单不存在|
|23|缺少交易数量参数|
|24|缺少交易价格参数|
|100001|系统异常|
|100002|系统升级|
|100004|请求参数不合法|
|100005|参数签名错误|
|100007|非法IP|
|110002|未知币种代号|

## 市场标记(symbol)

|虚拟币|BTC交易区|USDT交易区|ETH交易区|
|:---|:---|:---|:---|
|btc|-|btcusdt|-|
|eth|ethbtc|ethusdt|-|
|mvp|mvpbtc|mvpusdt|mvpeth|
|ht|htbtc|htusdt|hteth|

## 6.API
### 1) /open/api/all_order —— 获取全部委托，包括已成交和已取消(get)
|参数|填写类型|说明|
|:---|:---|:---|
|symbol|必填|市场标记，例如：ethbtc。见文档最后表一|
|pageSize|选填|页面大小|
|page|选填|页码|
|api_key|必填|api_key|
|time|必填|时间戳|
|sign|必填|签名|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下||
```
{
    "count":10,
    "orderList":[
        {
            "side":"BUY",
            "total_price":"0.10000000",
            "created_at":1510993841000,
            "avg_price":"0.10000000",
            "countCoin":"btc",
            "source":1,
            "type":1,
            "side_msg":"买入",
            "volume":"1.000",
            "price":"0.10000000",
            "source_msg":"WEB",
            "status_msg":"完全成交",
            "deal_volume":"1.00000000",
            "id":424,
            "remain_volume":"0.00000000",
            "baseCoin":"eth",
            "tradeList":[
                {
                    "volume":"1.000",
                    "feeCoin":"BTB",
                    "price":"0.10000000",
                    "fee":"0.16431104",
                    "ctime":1510996571195,
                    "deal_price":"0.10000000",
                    "id":306,
                    "type":"买入"
                }
            ],
            "status":2
        },
        {
            "side":"SELL",
            "total_price":"0.09900000",
            "created_at":1510993715000,
            "avg_price":"0.10000000",
            "countCoin":"btc",
            "source":1,
            "type":1,
            "side_msg":"卖出",
            "volume":"1.000",
            "price":"0.09900000",
            "source_msg":"WEB",
            "status_msg":"完全成交",
            "deal_volume":"1.00000000",
            "id":423,
            "remain_volume":"0.00000000",
            "baseCoin":"eth",
            "tradeList":[
                {
                    "volume":"1.000",
                    "feeCoin":"BTB",
                    "price":"0.10000000",
                    "fee":"0.16597075",
                    "ctime":1510993723973,
                    "deal_price":"0.10000000",
                    "id":261,
                    "type":"卖出"
                }
            ],
            "status":2
        }
    ]
}
```
### 2) /open/api/all_trade 获取全部成交记录 (get)
|参数|填写类型|说明|
|:---|:---|:---|
|symbol|必填|市场标记，例如：ethbtc。见文档最后表一|
|pageSize|选填|页面大小|
|page|选填|页码|
|api_key|必填|api_key|
|time|必填|时间戳|
|sign|必填|签名|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下||
```
{
    "count":22,
    "resultList":[
            {
                    "volume":"1.000",
                    "side":"BUY",
                    "feeCoin":"BTB",
                    "price":"0.10000000",
                    "fee":"0.16431104",
                    "ctime":1510996571195,
                    "deal_price":"0.10000000",
                    "id":306,
                    "type":"买入"
            },
            {
                "volume":"0.850",
                "side":"BUY",
                "feeCoin":"BTB",
                "price":"0.10000000",
                "fee":"0.13966438",
                "ctime":1510996571190,
                "deal_price":"0.08500000",
                "id":305,
                "type":"买入"
            },
            {
                "volume":"0.010",
                "side":"BUY",
                "feeCoin":"BTB",
                "price":"0.10000000",
                "fee":"0.00164311",
                "ctime":1510995560344,
                "deal_price":"0.00100000",
                "id":291,
                "type":"买入"
            },
            {
                "volume":"0.010",
                "side":"BUY",
                "feeCoin":"BTB",
                "price":"0.10000000",
                "fee":"0.00164311",
                "ctime":1510995560338,
                "deal_price":"0.00100000",
                "id":290,
                "type":"买入"
            },
            {
                "volume":"0.010",
                "side":"BUY",
                "feeCoin":"BTB",
                "price":"0.10000000",
                "fee":"0.00164311",
                "ctime":1510995560331,
                "deal_price":"0.00100000",
                "id":289,
                "type":"买入"
            },
            {
                "volume":"0.010",
                "side":"BUY",
                "feeCoin":"BTB",
                "price":"0.10000000",
                "fee":"0.00164311",
                "ctime":1510995555323,
                "deal_price":"0.00100000",
                "id":288,
                "type":"买入"
            }
	    ...
        ]
}
```
### 3) /open/api/cancel_order 取消委托单 (post)
|参数|填写类型|说明|
|:---|:---|:---|
|order_id|必填|订单ID|
|symbol|必填|市场标记，例如：ethbtc。见文档最后表一|
|api_key|必填|api_key|
|time|必填|时间戳|
|sign|必填|签名|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|""||

### 4) /open/api/common/symbols 查询系统支持的所有交易对及精度(get)
|参数|填写类型|说明|
|:---|:---|:---|
|无|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下||
```
{
	"code": "0",
	"msg": "suc",
	"data": [{
		"symbol": "ethbtc",
		"count_coin": "BTC",
		"amount_precision": 3,
		"base_coin": "ETH",
		"price_precision": 6
	}, {
		"symbol": "btcusdt",
		"count_coin": "USDT",
		"amount_precision": 4,
		"base_coin": "BTC",
		"price_precision": 2
	}, {
		"symbol": "ethusdt",
		"count_coin": "USDT",
		"amount_precision": 4,
		"base_coin": "ETH",
		"price_precision": 2
	}, {
		"symbol": "mvpusdt",
		"count_coin": "USDT",
		"amount_precision": 3,
		"base_coin": "MVP",
		"price_precision": 6
	}, {
		"symbol": "mvpeth",
		"count_coin": "ETH",
		"amount_precision": 3,
		"base_coin": "MVP",
		"price_precision": 8
	}, {
		"symbol": "wicceth",
		"count_coin": "ETH",
		"amount_precision": 2,
		"base_coin": "WICC",
		"price_precision": 8
	}]
}
```
### 5) /open/api/create_order 创建订单(post)
|参数|填写类型|说明|
|:---|:---|:---|
|side|必填|买卖方向BUY、SELL|
|type|必填|挂单类型，1:限价委托、2:市价委托|
|volume|必填|购买数量（多义，复用字段）;<br>type=1:表示买卖数量;<br>type=2:买则表示总价格，卖表示总个数。|
|price|选填|委托单价：type=2：不需要此参数|
|symbol|必填|市场标记，例如：ethbtc。见文档最后表一|
|fee_is_user_exchange_coin|选填|0，当开放平台币时，此参数表示是否使用用平台币支付手续费，0否，1是|
|api_key|必填|api_key|
|time|必填|时间戳|
|sign|必填|签名|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|{“order_id”:123123}|成功返回交易ID|

### 6) /open/api/get_records 获取K线数据(get)
|参数|填写类型|说明|
|:---|:---|:---|
|symbol|必填|市场标记，例如：ethbtc。见文档最后表一|
|period|必填|单位为分钟，比如1分钟则为1，一天则为1440|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下|

```
{
	"code": "0",
	"msg": "suc",
	"data": [
		[1536293520, 6482.23000000, 6485.29000000, 6482.23000000, 6485.29000000, 0.00060000],
		//时间戳,开盘价，最高价，最低价，收盘价，成交量
		[1536293580, 6485.73000000, 6486.90000000, 6485.73000000, 6486.90000000, 0.00070000],
		[1536293640, 6483.64000000, 6485.13000000, 6483.64000000, 6485.13000000, 0.00080000],
		[1536293700, 6485.51000000, 6488.20000000, 6485.51000000, 6488.20000000, 0.00110000],
		[1536293760, 6488.52000000, 6488.66000000, 6488.52000000, 6488.66000000, 0.00070000],
		...
		[1536308820, 6442.16000000, 6446.02000000, 6442.16000000, 6443.29000000, 0.00090000],
		[1536308880, 6444.56000000, 6444.56000000, 6443.78000000, 6443.78000000, 0.00050000],
		[1536308940, 6443.36000000, 6443.54000000, 6443.36000000, 6443.54000000, 0.00050000],
		[1536309000, 6446.42000000, 6451.19000000, 6446.42000000, 6450.56000000, 0.00040000],
	]
}
```
### 7) /open/api/get_ticker 获取当前行情(get)
|参数|填写类型|说明|
|:---|:---|:---|
|symbol|必填|市场标记，例如：ethbtc。见文档最后表一|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下||

```
{
    "high": 1.12,//最高值
    "vol": 7232.26315789,//交易量
    "last": 173.60263169,//最新成交价
    "low": 0.01,//最低值
    "buy": "0.01000000",//买一价
    "sell": "1.12345680",//卖一价
    "time": 1514466455542
}
```
### 8) /open/api/get_trades 获取行情成交记录(get)
|参数|填写类型|说明|
|:---|:---|:---|
|symbol|必填|市场标记，例如：ethbtc。见文档最后表一|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下||
```
[
    {
        "amount": 0.55,//成交量
        "price": 0.18519949,//成交价
        "id": 447121,
        "type": "buy"//买卖type，买为buy，买sell
    },
    {
        "amount": 16.45,
        "price": 0.18335468,
        "id": 447120,
        "type": "buy"
    },
    {
        "amount": 2.92,
        "price": 0.183324003,
        "id": 447118,
        "type": "sell"
    }
]
```
### 9) /open/api/market_dept 查询买卖盘深度(get)
|参数|填写类型|说明|
|:---|:---|:---|
|symbol|必填|市场标记，例如：ethbtc。见文档最后表一|
|type|必填|深度类型，step0，step1，step2|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下||
```
{
    "tick":{
        "asks":[//卖盘
            {22112.22,0.9332},
            {22112.21,0.2},
            {22112.21,0.2},
            {22112.21,0.2},
            {22112.21,0.2},
        ],
        "bids":[//买盘
            {22111.22,0.9332},
            {22111.21,0.2},
            {22112.21,0.2},
            {22112.21,0.2},
            {22112.21,0.2},
        ]
    }
}
```

### 10) /open/api/market 获取各个币对的最新成交价(get)

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下||
```
{
	'btcusdt': 6487.07,
	'mvpeth': 6.878e-05,
	'hteth': 0.00765646,
	'rwcteth': 0.01,
	'miceth': 3.5e-06,
	'htbtc': 0.00027066,
	'bnbbtc': 0.0015624,
	'mvpusdt': 0.015634,
	'wicceth': 0.0008035,
	'ethbtc': 0.035293,
	'bnbeth': 0.044306,
	'micbtc': 3.6e-07,
	'ethusdt': 228.9
}
```
### 11) /open/api/new_order 获取当前委托，包括未成交和正在成交的委托(get)

|参数|填写类型|说明|
|:---|:---|:---|
|symbol|必填|市场标记，例如：ethbtc。见文档最后表一|
|pageSize|选填|页面大小|
|page|选填|页码|
|api_key|必填|api_key|
|time|必填|时间戳|
|sign|必填|签名|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下||
```
{
    "count": 10,
    "resultList": [
        {
            "side": "BUY",
            "total_price": "0.10000000",
            "created_at": 1510993841000,
            "avg_price": "0.10000000",
            "countCoin": "btc",
            "source": 1,
            "type": 1,
            "side_msg": "买入",
            "volume": "1.000",
            "price": "0.10000000",
            "source_msg": "WEB",
            "status_msg": "完全成交",
            "deal_volume": "1.00000000",
            "id": 424,
            "remain_volume": "0.00000000",
            "baseCoin": "eth",
            "tradeList": [
                {
                    "volume": "1.000",
                    "feeCoin": "BTB",
                    "price": "0.10000000",
                    "fee": "0.16431104",
                    "ctime": 1510996571195,
                    "deal_price": "0.10000000",
                    "id": 306,
                    "type": "买入"
                }
            ],
            "status": 2
        },
        {
            "side": "SELL",
            "total_price": "0.09900000",
            "created_at": 1510993715000,
            "avg_price": "0.10000000",
            "countCoin": "btc",
            "source": 1,
            "type": 1,
            "side_msg": "卖出",
            "volume": "1.000",
            "price": "0.09900000",
            "source_msg": "WEB",
            "status_msg": "完全成交",
            "deal_volume": "1.00000000",
            "id": 423,
            "remain_volume": "0.00000000",
            "baseCoin": "eth",
            "tradeList": [
                {
                    "volume": "1.000",
                    "feeCoin": "BTB",
                    "price": "0.10000000",
                    "fee": "0.16597075",
                    "ctime": 1510993723973,
                    "deal_price": "0.10000000",
                    "id": 261,
                    "type": "卖出"
                }
            ],
            "status": 2
        }
    ]
}
```
### 12) /open/api/order_info 获取订单详情(get)
|参数|填写类型|说明|
|:---|:---|:---|
|order_id|必填|订单ID|
|symbol|必填|市场标记，例如：ethbtc。见文档最后表一|
|api_key|必填|api_key|
|time|必填|时间戳|
|sign|必填|签名|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下||
```
{
    "order_info":{
        "id":343,
        "side":"sell",
        "side_msg":"卖出",
        "created_at":"09-2212:22",
        "price":222.33,
        "volume":222.33,
        "deal_volume":222.33,
        "total_price":222.33,
        "fee":222.33,
        "age_price":222.33
    }，
    "trade_list":[
        {
            "id":343,
            "created_at":"09-22 12:22",
            "price":222.33,
            "volume":222.33,
            "deal_price":222.33,
            "deal_fee":222.33
        },
        {
            "id":345,
            "created_at":"09-22 12:22",
            "price":222.33,
            "volume":222.33,
            "deal_price":222.33,
            "deal_fee":222.33
        }
    ]
}
```

### 13) /open/api/user/account 资产余额(get)
|参数|填写类型|说明|
|:---|:---|:---|
|api_key|必填|api_key|
|time|必填|时间戳|
|sign|必填|签名|

返回

|字段|实例|说明|
|:---|:---|:---|
|code|0||
|msg|"suc"|code>0失败|
|data|如下||
```
{
    "total_asset": 432323.23, //总资产
    "coin_list": [
        {
            "coin": "btc",
            "normal": 3.233,//余额账户
            "locked": 0.233,//冻结账户
            "btcValuatin": 3.233 //BTC估值
        },
        {
            "coin": "eth",
            "normal": 3.233,
            "locked": 3.233,
            "btcValuatin": 0.312
        }
    ]
}
```


