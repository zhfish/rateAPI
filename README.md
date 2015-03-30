汇率转换API
----
###币种数据: currency_cn.json
| 字段名称 | 描述 |
| -- | - |
| longname | 中文描述 |
| shortname | 英文简写 |
| symbol | 货币符号 |

```
这个文件是数组,可以根据需求自行删减和排序
```

###汇率API(HTTP GET):

http://query.yahooapis.com/v1/public/yql?q=select id,Rate from yahoo.finance.xchange where pair in (`"USDCNY", "CNYUSD"`)&env=store://datatables.org/alltableswithkeys&format=json

#####中间高亮部分可变,规则为 双引号("")内,原始货币英文简写+目标汇率英文简写,多条用逗号(,)分割  
#####返回值为JSON

```
{
"query": {  
	"count": 2,   //数量
	"created": "2015-03-30T08:52:01Z",  //查询时间
	"lang": "zh-CN",  //语言
	"results": {  //结果集合
		"rate": [  //汇率
			{  
				"id": "USDCNY",  //之前传入的参数 
				"Rate": "6.2060"  //汇率
			},  
			{  
				"id": "CNYUSD",  //之前传入的参数 
				"Rate": "0.1611"  //汇率
			}  
		]  
		}  
	}  
}  
```

###完整的例子
#####1.需求 日元<->欧元 互转,选择原始货币 日元,数值123456
通过currency_cn.json得知  
日元shortname为 JPY  
欧元shortname为 EUR  
为了减少请求,一次性取回 日元->欧元 汇率 和 欧元->日元 汇率  
#####2.调用接口  
http://query.yahooapis.com/v1/public/yql?q=select id,Rate from yahoo.finance.xchange where pair in (`"JPYEUR", "EURJPY"`)&env=store://datatables.org/alltableswithkeys&format=json  
#####3.取得数据
```
{
	"query": {
		"count": 2,
		"created": "2015-03-30T09:04:18Z",
		"lang": "zh-CN",
		"results": {
			"rate": [
				{
					"id": "JPYEUR",
					"Rate": "0.0077"
				},
				{
					"id": "EURJPY",
					"Rate": "130.0265"
				}
			]
		}
	}
}
```
#####4.计算  
日元转欧元  
123456 * 0.0077 = 950.61, 保留后两位
#####5.互换位置,计算
950.61 * 130.0265 = 123604.49 保留后两位









