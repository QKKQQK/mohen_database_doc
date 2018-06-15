# 目录

* [数据库数据格式](#数据库数据格式)  
  * [tbl_report_raw 集合 文档结构](#tbl_report_raw-集合-文档结构)  
  * [tbl_report_raw 集合 字段要求(BSON)](#tbl_report_raw-集合-字段要求)  
  * [tbl_report_min 集合 文档结构](#tbl_report_min-集合-文档结构)  
  * [tbl_report_min 集合 字段要求(BSON)](#tbl_report_min-集合-字段要求)
* [API](#api)  
  * [POST 上传 JSON格式](#post-上传-json格式)
  * [POST 上传 字段要求](#post-上传-字段要求)
  * [POST 搜索 JSON格式](#post-搜索-json格式)
  * [POST 搜索 class Search](#post-搜索类-class-search)  
 
## 数据库数据格式

### tbl_report_raw 集合 文档结构
**注：** 当新的原始数据通过POST发送给服务器时，如符合规则，原始数据将插入tbl_report_raw集合，并按情况与tbl_report_min集合里同一时间(精确到分钟)的同类数据合并或覆盖。

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
        "openid" : String,
        "v1" : Double,
        "v2" : Double,
        "v3" : Object
        "cfg" : String,
        "utc_date" : Date
    }

### tbl_report_raw 集合 字段要求  

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
openid | 数据提交第三方 _id, uuid格式 | String | 是  | "04ca7d4a-706e-11e8-adc0-fa7ae01bbebc"  
v1 | 数值：操作次数 | Double | 是 | 10.0  
v2 | 数值：事件时长 | Double | 是 | 10.0  
v3 | 拓展数值(备用) | Object | 是  (例子：v3, v3.test_val1, v3.test_val2) | {test_val1 : 10.0, test_val2 : 9999.0}  
cfg | 字符串值 | String | 是 | "Y\|Y\|Y\|"  
utc_date | 数据创建日期时间(UTC+0) | Date | 是 | "2018-06-12 10:53:54.247"  


### tbl_report_min 集合 文档结构  
**注：** tbl_report_min 集合会将同一分钟(y/m/d/h/m相同)的，相同name, exttype, type, uyear, uid, fid, eid, openid值的数据合并。合并后数据的pid, tag, klist, rlist, extlist, cfg值将会被最后一条原始数据覆盖，v1, v2, v3值相加累计，v1_norm, v2_norm, v3_norm值根据新的v1, v2, v3值重新计算

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
        "openid" : String,
        "v1" : Double,
        "v1_norm" : Double,
        "v2" : Double,
        "v2_norm" : Double,
        "v3" : Object,
        "v3_norm" : Object,
        "cfg" : String,
        "utc_date" : Date
    }

### tbl_report_min 集合 字段要求  

字段 | 意义 | BSON类型 | 建立索引 | 例子
---- | ---- | --- | ---- | ----
_id  | 数据的MongoDB _id(自动创建) | ObjectId | 是 | ObjectId("5b3a62680000000000000000")  
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
openid | 数据提交第三方 _id | String | 是  | "04ca7d4a-706e-11e8-adc0-fa7ae01bbebc"  
v1 | 数值：操作次数 | Double | 是 | 10.0  
v1_norm | 数值：归一化操作次数 | Double | 是 | 0.321  
v2 | 数值：事件时长 | Double | 是 | 10.0  
v2_norm | 数值：归一化事件时长 | Double | 是 | 0.5234
v3 | 拓展数值(备用) | Object | 是  (例子：v3, v3.test_val1, v3.test_val2) | {test_val1 : 10.0, test_val2 : 9999.0}  
v3_norm | 归一化拓展数值(备用) | Object | 是 (例子: v3_norm, v3_norm.test_val1, v3_norm.test_val2) | {test_val1 : 0.2311, test_val2 : 0.885} 
cfg | 字符串值 | String | 是 | "Y\|Y\|Y\|"  
utc_date | 数据创建日期时间(UTC+0) | Date | 是 | "2018-06-12 10:53:54.247"  

## API  

### POST 上传 JSON格式

    {
        "_id" : "5a0ab7dad5cb310b9830ef27",  
        "pid"  : "5a0ab7dad5cb310b9830ef27",  
        "name" : "密码重置",   
        "exttype" : 512,  
        "type" : 50,  
        "tag": ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"],  
        "klist" : ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"],  
        "rlist" : ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"], 
        "extlist" : { "path_1" : ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"] },  
        "ugroup" : 2015,  
        "uid" : "5a0ab7dad5cb310b9830ef27",  
        "fid" : "5a0ab7dad5cb310b9830ef27",  
        "eid" : "5a0ab7dad5cb310b9830ef27",  
        "openid" : "f857e9f6-6e26-11e8-adc0-fa7ae01bbebc",  
        "v1" : 10.0,  
        "v2" : 15.0,  
        "v3" : {  "val_1" : 123.456 },  
        "cfg" : "Y|Y|Y|",  
        "utc_date" : "2018-06-12 10:53:54.247"
    }

### POST 上传 字段要求  

字段 | 意义 | 必需 | 类型 |  要求 | 默认值 | 例子  
---- | ---- | ---- | ---- | ---- | ----- | ----- 
_id  | 数据的MongoDB _id | 是 | String | 24位16进制数字字符串，不可重复 | 无 | "5a0ab7dad5cb310b9830ef27"  
pid  | 数据的父级节点 _id | 否 | String | 24位16进制数字字符串 | "000000000000000000000000" | "5a0ab7dad5cb310b9830ef27"  
name | 事件名称 | 否 | String | 无 | "" | "密码重置"   
exttype | 事件所属小类别代码 | 是 | Number | 无 | 无 | 512  
type | 事件所属大类别代码 | 是 | Number | 无 | 无 |50  
tag | 事件相关标签(备用) | 否 | [String] | 24位16进制数字字符串 | [] | ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"]  
klist | 知识点树路径 | 否 | [String] | 24位16进制数字字符串 | [] | ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"]  
rlist | 关系树路径 | 否 | [String] | 24位16进制数字字符串 | [] | ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"]  
extlist | 拓展路径(备用) | 否 | Object | 24位16进制数字字符串 | {} | { "path_1" : ["5a0ab7dad5cb310b9830ef26", "5a0ab7dad5cb310b9830ef27"] }  
ugroup | 用户所属大分类(如“届”)代码 | 否 | Number | 无 | 0 | 2015  
uid | 用户 _id | 否 | String | 24位16进制数字字符串 | "000000000000000000000000" | "5a0ab7dad5cb310b9830ef27"  
fid | 文件 _id(备用) | 否 | String | 24位16进制数字字符串 | "000000000000000000000000" | "5a0ab7dad5cb310b9830ef27"  
eid | 设备 _id | 是 | String | 24位16进制数字字符串 | 无 | "5a0ab7dad5cb310b9830ef27"  
openid | 数据提交第三方 _id | 是 | String | 使用UUID | 无 | "f857e9f6-6e26-11e8-adc0-fa7ae01bbebc"  
v1 | 数值：操作次数 | 是 | Number | 无 | 无 | 10.0  
v2 | 数值：事件时长 | 是 | Number | 无 | 无 | 15.0  
v3 | 拓展数值(备用) | 否 | Object | 无 | {} | {  "val_1" : 123.456 }  
cfg | 字符串值 | 否 | String | 无 | "" | "Y\|Y\|Y\|"  
utc_date | 数据创建日期时间(UTC+0) | 是 | String | UTC+0标准时间 | 无 | "2018-06-12 10:53:54.247"  

### POST 搜索 JSON格式  

    {
        // 用于判定搜索类别的元数据
        "metadata" : {
            "query_count" : 3,
            "coefficient" : {
                // 例子： 多个Query合并
                // "set_a"，用来区分不同的组合和标注数据csv文件  
                "set_a" : {
                    "5xtw0001" : 0.7,
                    "5xtw0002" : 0.3,
                    "5xtw0003" : 0.7
                },
                // 例子： 只有1个Query
                "set_b" : {
                    "5xtw0003" : 1.0
                },
                // 一次POST可以有多个组合
                "set_c" : {
                    "5xtw0001" : 0.3,
                    "5xtw0003" : 0.7
                }
            }
        },
        // 序列后的class Search数据
        "data" : [
            {
                // “query_id”，用来区分query和标注数据csv文件
                "query_id" : "5xtw0001",
                // 序列化的class Search
                "data" : {
                    "序列化的Query 1"
                }
            },
            {
                "query_id" : "5xtw0002",
                "data" : {
                    "序列化的Query 2"
                }
            },
            {
                "query_id" : "5xtw0003",
                "data" : {
                    "序列化的Query 3"
                }
            }
        ]
    }

### POST 搜索类 class Search  

    class Search {
        String openid; // 第三方ID

        // 匹配搜索

        String[] rlist_match; // [匹配搜索] 零到多个关系树节点ID
        Dict extlist_match; // [匹配搜索] 零到多个 拓展树名称：[拓展树节点ID]
        Int[] ugroup_match; // [匹配搜索] 零到多个用户分组数值
        String[] uid_match; // [匹配搜索] 零到多个用户ID
        String[] fid_match; // [匹配搜索] 零到多个文件ID
        String[] eid_match; // [匹配搜索] 零到多个设备ID
        String[] name_match; // [匹配搜索] 零到多个事件名称
        Int[] exttype_match; // [匹配搜索] 零到多个事件小分类代码
        Int[] type_match; // [匹配搜索] 零到多个事件大分类代码
        String[] tag_match; // [匹配搜索] 零到多个标签
        String[] klist_match; // [匹配搜索] 零到多个知识点树节点ID
        String[] date; // [匹配搜索] 日期，精确到分钟
        Float v1; // [匹配搜索] v1数值
        Float v2; // [匹配搜索] v2数值
        Dict v3; // [匹配搜索] v3数值，零到多个 变量名称：数值
        String cfg; // [匹配搜索] 字符串数值

        // 范围搜索，使用匹配搜索变量作为下限(包含)
        // 匹配搜索时不需要POST以下对应变量
        
        // 以下变量用于范围搜索, 变量foo与变量foo_upper同一index上的两个元素表示一组范围(包含)
        // 例子：ugroup_match[0] 与 ugroup_match_upper[0] 组成一个范围
        // ugroup_match 与 ugroup_match_upper 每一位相对应
        // 例子：搜索符合 {ugroup <= 2013 OR 2016 <= ugroup <= 2017 OR 2019 <= ugroup} 的数据时
        // ugroup_match = [min, 2016, 2019] ugroup_match_upper = [2013, 2017, max]
        // min, max 根据业务逻辑由前端决定
        Int[] ugroup_match_upper; // [范围搜索] ugroup 搜索上限(包含)
        Int[] exttype_match_upper; // [范围搜索] exttype 搜索上限(包含)
        Int[] type_match_upper; // [范围搜索] type 搜索上限(包含)
        String[] date_upper; // [匹配搜索] 日期搜索上限(包含)
        Float v1_upper; // [范围搜索] v1数值 搜索上限(包含)
        Float v2_upper; // [范围搜索] v2数值 搜索上限(包含)
        Dict v3_upper; // [范围搜索] v3数值，零到多个 变量名称：搜索上限(包含)

        // 排序，默认以Date从小到大排序， 一般不推荐使用

        Int asc; // 1 从小到大， -1 从大到小
        String order_by; // 可用字段： [fid, eid, uid, name, (exttype.var), type, v1, v2, (v3.var), v1_norm, v2_norm, (v3_norm.var), cfg]

        // 聚合
        String[] group_type; // 可用选项：[sum, avg, min, max]
        String group_by; // 可选变量: [year, month, day, hour, minute, fid, eid, name, rlist(最底层节点), (extlist.var), ugroup, uid, exttype, type, klist(最底层节点), cfg]
    }

[返回目录](#目录)
