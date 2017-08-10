# jsonrpc

## 发行代币

```go
type IssueArg struct {
	Coin   Asset  `json:"coin"`
	Issuer string `json:"issuer"` // 发行者私钥
}

```

json调用

```json
{
	"method": "RpcServer.Issue", 
	"id": "123",  
	"params": [{
		"coin": {
			"amount": 1000, 
			"symbol": "RMB"
		}, 
		"issuer": "26PMucQQpcQXBN64Xv2HtyWsrJ6NjJSpq1wtK5EkPZmN4HqFdz59naMuBLokvfgUesBT3ts9xUgzHvX5NSCzYvhc"
	}]
}
```

json返回

```json
{"id":123,"result":true or false,"error":null}
```

## 支付代币

```go
type PyamentArg struct {
	Coin      Asset  `json:"coin"`
	Sender    string `json:"sender"`    // 发送者私钥
	Recipient string `json:"recipient"` // 接收者公钥
}

```

json调用

```json
{
	"method": "RpcServer.Payment", 
	"id": "123",  
	"params": [{
		"coin": {
			"amount": 1000, 
			"symbol": "RMB"
		}, 
		"sender": "26PMucQQpcQXBN64Xv2HtyWsrJ6NjJSpq1wtK5EkPZmN4HqFdz59naMuBLokvfgUesBT3ts9xUgzHvX5NSCzYvhc",
		"recipient": "EoJ6t3d9T4FfNwkjznRHybH1AzBahuV2JZhSsCA1e88T"
	}]
}
```

json返回

```json
{"id":123,"result":true or false,"error":null}
```

## 查询余额

```go
type BalanceArg struct {
	Who string `json:"who"` // 公钥账号
}

type BalanceResult struct {
	Who   string  `json:"who"`
	Coins []Asset `json:"coins"`
	Count int     `json:"txcount"` // 交易的数量
}

```

json 调用

```json
{
	"method": "RpcServer.Balance", 
	"id": "123",  
	"params": [{
		"who": "EoJ6t3d9T4FfNwkjznRHybH1AzBahuV2JZhSsCA1e88T"
	}]
}
```

json返回

```json
{
	"id":123,
	"result":{
		"who": "EoJ6t3d9T4FfNwkjznRHybH1AzBahuV2JZhSsCA1e88T",
		"coins":[
			{
				"amount": 1000, 
				"symbol": "RMB"
			},
			{
				"amount": 100000, 
				"symbol": "USD"
			}			
		],
		"txcount": 1000
	},
	"error":null
}
```

## 交易列表

```go 
type TxListArg struct {
	Who   string `json:"who"`   // 公钥账号
	Start int    `json:"start"` // 起始
	Count int    `json:"count"` // 数量
}

// 交易结果
type TxResult struct {
	Who       string `json:"who"`       // 公钥账号
	Type      int    `json:"type"`      // 0 is Issue, 1 is Payment
	Coin      Asset  `json:"coin"`      // 支付或发行的代币
	Time      int64  `json:"time"`      // 时间
	Recipient string `json:"recipient"` // 接收者公钥, if Type == 0, null
}

// 列表结果
type TxListResult struct {
	List []*TxResult `json:"list"`
}

```

json 调用

```json
{
	"method": "RpcServer.TxList", 
	"id": "123",  
	"params": [{
		"who": "EoJ6t3d9T4FfNwkjznRHybH1AzBahuV2JZhSsCA1e88T",
		"start": 500,
		"count": 100
	}]
}
```

json 返回

```json
{
	"id":123,
	"result": {
		"list": [
			{
				"who": "EoJ6t3d9T4FfNwkjznRHybH1AzBahuV2JZhSsCA1e88T",
				"type": 0,
				"coin":{
					"amount": 1000, 
					"symbol": "RMB"
				},
				"time": 12345677,
				"recipient": ""
			},
			{
				"who": "EoJ6t3d9T4FfNwkjznRHybH1AzBahuV2JZhSsCA1e88T",
				"type": 1,
				"coin":{
					"amount": 1000, 
					"symbol": "USD"
				},
				"time": 12345677,
				"recipient": "EoJ6t3d9T4FfNwkjznRHybH1AzBahuV2JZhSsCA1e88T"
			}
		]
	},
	"error":null
}
```

