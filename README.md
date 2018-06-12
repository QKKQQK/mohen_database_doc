# 目录

* [数据库数据格式](#数据库数据格式)  
  * [tbl_report_raw 集合 文档结构](#tbl_report_raw-集合-文档结构)  
  * [tbl_report_raw 集合 字段格式(BSON)](#tbl_report_raw-集合-字段bson格式)  
* [API](#api)  
  * [POST 上传数据 字段格式(JSON)](#post-上传数据-字段json格式)  
  

 
## 数据库数据格式

### tbl_report_raw 集合 文档结构
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

### tbl_report_raw 集合 字段BSON格式  

字段 | 意义 | BSON类型 | 建立索引 | 例子
---- | ---- | --- | ---- | ----
_id  | 数据的MongoDB _id | ObjectId | 是 | ObjectId("5b3a62680000000000000000")  
pid | 数据的父级节点 _id | ObjectId | 是 | ObjectId("5b3a62680000000000000000")  
name | 事件名称 | String | 是 | "文档查看"  
flag | 数据状态标志 | Int32 | 是 | 1  
exttype | 事件所属小类别代码 | Int32 | 是 | 512  
type | 事件所属大类别代码 | Int32 | 是 | 50  
tag | 事件相关标签(备用) | Array | 是 | [ObjectId("5b3a62680000000000000000"), ObjectId("5b3a62680000000000000001")]  
klist | 知识点树路径 | Array | 是 | [ObjectId("5b3a62680000000000000000"), ObjectId("5b3a62680000000000000001")]  
rlist | 关系树路径 | Array | 是 | [ObjectId("5b3a62680000000000000000"), ObjectId("5b3a62680000000000000001")]  
extlist | 拓展路径(备用) | Object | 是  (例子：extlist, extlist.test_path) | {test_path : [ObjectId("5b3a62680000000000000000"), ObjectId("5b3a62680000000000000001")]}  
ugroup | 用户所属大分类(如“届”)代码 | Int32 | 是 | 2016  
uid | 用户 _id | ObjectId | 是 | ObjectId("5b3a62680000000000000000")  
fid | 文件 _id(备用) | ObjectId | 是 | ObjectId("5b3a62680000000000000000")  
eid | 设备 _id | ObjectId | 是 | ObjectId("5b3a62680000000000000000")  
openid | 数据提交第三方 _id | ObjectId | 是  | ObjectId("5b3a62680000000000000000")  
v1 | 数值：操作次数 | Double | 是 | 10.0  
v2 | 数值：事件时长 | Double | 是 | 10.0  
v3 | 拓展数值(备用) | Object | 是  (例子：v3, v3.test_val1, v3.test_val2) | {test_val1 : 10.0, test_val2 : 9999.0}  
cfg | 字符串值 | String | 否  | "Y\|Y\|Y\|"  
utc_date | 数据创建日期时间(UTC+0) | Date | 是 | "2018-06-12 10:53:54.247"  
utc_ts | 数据创建时间戳(UTC+0)(秒) | Double | 是 | 1528743234.2477944  

## API  

### POST 上传数据 字段JSON格式  

字段 | 意义 | 必需 | 类型 |  要求 | 默认值 | 例子  
---- | ---- | ---- | ---- | ---- | ----- | ----- 
_id  | 数据的MongoDB _id | 是 | String | 24位16进制数字字符串，不可重复 | 无 | "5a0ab7dad5cb310b9830ef27"  
pid  | 数据的父级节点 _id | 否 | String | 24位16进制数字字符串 | "000000000000000000000000" | "5a0ab7dad5cb310b9830ef27"  
name | 事件名称 | 是 | String | 不可为"" | 无 | "密码重置"   
exttype | 事件所属小类别代码 | 是 | Number | 无 | 无 | 512  
type | 事件所属大类别代码 | 是 | Number | 无 | 无 |50  
tag | 事件相关标签(备用) | 否 | [String] | 24位16进制数字字符串 | [] | ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"]  
klist | 知识点树路径 | 否 | [String] | 24位16进制数字字符串 | [] | ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"]  
rlist | 关系树路径 | 是 | [String] | 24位16进制数字字符串 | 无 | ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"]  
extlist | 拓展路径(备用) | 否 | Object | 24位16进制数字字符串 | {} | { "path_1" : ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"] }  
ugroup | 用户所属大分类(如“届”)代码 | 否 | Number | 无 | 0 | 2015  
uid | 用户 _id | 否 | String | 24位16进制数字字符串 | "000000000000000000000000" | "5a0ab7dad5cb310b9830ef27"  
fid | 文件 _id(备用) | 否 | String | 24位16进制数字字符串 | "000000000000000000000000" | "5a0ab7dad5cb310b9830ef27"  
eid | 设备 _id | 是 | String | 24位16进制数字字符串 | 无 | "5a0ab7dad5cb310b9830ef27"  
openid | 数据提交第三方 _id | 是 | String | 24位16进制数字字符串 | 无 | "5a0ab7dad5cb310b9830ef27"  
v1 | 数值：操作次数 | 是 | Number | 无 | 无 | 10.0  
v2 | 数值：事件时长 | 是 | Number | 无 | 无 | 15.0  
v3 | 拓展数值(备用) | 否 | Object | 无 | {} | {  "val_1" : 123.456 }  
cfg | 字符串值 | 否 | String | 无 | "" | "Y\|Y\|Y\|"  
local_ts | 当地时间戳(秒 ) | 是 | Number | 符合时间戳格式，使用当地时间 | 无 | 1528743234.2477944  
timezone | 当地时区(UTC) | 是 | Number |  整数，如北京时间UTC+8则输入8 | 无 | 8  

[返回目录](#目录)
