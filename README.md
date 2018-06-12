# 目录

* [数据格式](#数据格式) 
  * [tbl_report_raw 集合 数据记录行结构](#tbl_report_raw-集合-数据记录行结构)
  * [tbl_report_raw 集合 数据字段格式](#tbl_report_raw-集合-数据字段格式)  
    * [_id](#_id)  
    * [pid](#pid)
    * [name](#name)
    * [flag](#flag)
    * [exttype](#exttype)
    * [type](#type)
    * [tag](#tag)
    * [klist](#klist)
    * [rlist](#rlist)
    * [extlist](#extlist)
    * [ugroup](#ugroup)
    * [uid](#uid)
    * [fid](#fid)
    * [eid](#eid)
    * [openid](#openid)
    * [v1](#v1)
    * [v2](#v2)
    * [v3](#v3)
    * [cfg](#cfg)
    * [utc_date](#utc_date)
    * [utc_ts](#utc_ts)
    
  
## 数据格式

### tbl_report_raw 集合 数据记录行结构
    {
    	"_id" : ObjectId,
    	"pid" : ObjectId,
    	"name" : String,
    	"flag" : Int32,
    	"exttype" : Int32,
    	"type" : Int32,
    	"tag" : [ObjectId],
    	"klist" : [ObjectId],
    	"rlist" : [ObjectId],
    	"extlist" : Object,
    	"uyear" : Int32,
    	"uid" : ObjectId,
    	"fid" : ObjectId,
    	"eid" : ObjectId,
    	"openid" : ObjectId,
    	"v1" : Double,
    	"v2" : Double,
    	"v3" : Object
    	"cfg" : String,
    	"utc_date" : Date,
    	"utc_ts" : Double
    }

### tbl_report_raw 集合 数据字段格式

#### `_id` 
**数据的MongoDB _id**  
类型：`ObjectId`， 24位16进制字符  
索引：是  
例子：`ObjectId("5b3a62680000000000000000")`  


#### `pid`
**数据的父级节点 _id**  
类型：`ObjectId`， 24位16进制字符  
索引：是  
例子：`ObjectId("5b3a62680000000000000000")`  


#### `name`
**事件名称**  
类型：`String`  
索引：是  
例子：`文档查看`  


#### `flag`
**数据状态标志**  
类型：`Int32`  
索引：是  
例子：`1`


#### `exttype`
**事件所属小类别代码**  
类型：`Int32`  
索引：是  
例子：`512`

#### `type`
**事件所属大类别代码**  
类型：`Int32`  
索引：是  
例子：`50`


#### `tag`  
**事件相关标签(备用)**  
类型：`Array(ObjectId)`  
索引：是  
例子：

    [
        ObjectId("5b3a62680000000000000000"),
        ObjectId("5b3a62680000000000000001")
    ]


#### `klist`
**知识点树路径**  
类型：`Array(ObjectId)`  
索引：是  
例子：  

    [
        ObjectId("5b3a62680000000000000000"),
        ObjectId("5b3a62680000000000000001")
    ]


#### `rlist`
**用户关系树路径**  
类型：`Array(ObjectId)`  
索引：是  
例子：  

    [
        ObjectId("5b3a62680000000000000000"),
        ObjectId("5b3a62680000000000000001")
    ]


#### `extlist`
**拓展路径(备用)**  
类型：`Object`  
索引：是  (extlist, extlist.test_path)  
例子：  

    extlist : {
        test_path : [ 
                        ObjectId("5b3a62680000000000000000"),
                        ObjectId("5b3a62680000000000000001")
                    ]
    }


#### `ugroup`  
**用户所属大分类(如“届”)代码**  
类型：`Int32`  
索引：是  
例子：`2016`


#### `uid`
**用户 _id**  
类型：`ObjectId`， 24位16进制字符  
索引：是  
例子：`ObjectId("5b3a62680000000000000000")` 


#### `fid`
**文件 _id(备用)**  
类型：`ObjectId`， 24位16进制字符  
索引：是  
例子：`ObjectId("5b3a62680000000000000000")` 


#### `eid`
**设备 _id**  
类型：`ObjectId`， 24位16进制字符  
索引：是  
例子：`ObjectId("5b3a62680000000000000000")` 


#### `openid`
**数据提交第三方 _id**  
类型：`ObjectId`， 24位16进制字符  
索引：是  
例子：`ObjectId("5b3a62680000000000000000")` 


#### `v1`
**数值：操作次数**  
类型：`Double`  
索引：是  
例子：`10.0` 


#### `v2`
**数值：事件时长**  
类型：`Double`  
索引：是  
例子：`10.0` 


#### `v3`
**拓展数值(备用)**  
类型：`Object`  
索引：是  (v3, v3.test_val1, v3.test_val2)  
例子： 

    v3 : {
        test_val1 : 10.0,
        test_val2 : 9999.0
    }


#### `cfg`
**字符串值**  
类型：`String`  
索引：否  
例子：`Y|Y|Y|`


#### `utc_date`
**UTC标准日期时间**  
类型：`Date`  
索引：是  
例子：`2018-06-12 10:53:54.247`


#### `utc_ts`
**UTC标准时间戳(秒)**  
类型：`Double`  
索引：是  
例子：`1528743234.2477944`

[返回目录](#目录)
