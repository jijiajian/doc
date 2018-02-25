## 数据库基本操作

* 查看数据库  
  SHOW DATABASES;
* 使用某个数据库  
  use data_base_name;
* 查看表  
  SHOW TABLES;
* 修改数据库字符  
alter  database  character  set utf8;

## 查看表结构
1. `简略版`  
   describe  table_name;  
   desc  table_name;  /*简写*/

2. `详细版本(保留了个人认为比较重要的部分字段)`
```
SELECT
	column_name,
	column_comment,
	is_nullable,
	table_name,
	table_schema,
	character_set_name,
	column_type,/*数据类型 + 长度*/
	data_type,/*数据类型 不带长度*/
	column_key,
	column_default,
	ordinal_position
FROM
	information_schema. COLUMNS
WHERE
	table_schema = 'yuntask_test'
AND table_name = 'yk_user_seoer';
```

## 表结构相关

* `建表`

```
create table agent_rebate_record(
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `agent_id` int(11) not null default 0 comment '代理商Id',
    `amount` decimal(10,2) NOT NULL DEFAULT '0.00' COMMENT '金额',
    `rebate_proportion` int(11) not null default 0 comment '返现比例 退回类型为0',
    `balance_record_id` int(11) not null default 0 comment '商户流水Id',
    `site_id` int(11) not null default 0 comment '站点Id',
    `site_url` varchar(300) not null default '' comment '站点域名',
    `task_order_id` int(11) not null default 0 comment '任务Id',
    `record_type` tinyint  not null default 0 comment '记录类型 1预存返现 2服务费返现 3预存返现退回 4服务费返现退回',
    `rebate_bill_id` int(11) not null default 0 comment '返点账单id',
    `create_user_id`  int(11) not null default 0 comment '商户id(关联yk_user_company表的user_id)',
    `create_time`  datetime not null default CURRENT_TIMESTAMP comment '创建时间',
    `update_time` datetime default null COMMENT '更新时间',
    PRIMARY KEY (`id`),
    key(`agent_id`)    
)ENGINE=InnoDB  DEFAULT CHARSET=utf8 comment '代理商返现记录表';
```


* `删除表`
```
drop  table  `table_name`;
```

* `重命名表`

```
alter table  `old_name`  rename  `new_name`;
```

* `添加字段`
```
ALTER TABLE `table_name` ADD column_name VARCHAR(3) COMMENT '测试插入列' AFTER column_name0;
```

* `删除字段`
```
ALTER  TABLE  `table_name` DROP column_name;
```

* `修改字段属性`
```
ALTER TABLE  test_create  MODIFY after_colum2  VARCHAR(30) NOT NULL DEFAULT '' COMMENT '测试修改';
```

* `修改字段属性及字段名`  
`(change关键字必须要有old_column_name 和 new_column_name,即使不改变字段名,若只是修改字段属性,modify关键字更为合适)`
```
ALTER TABLE  test_rename  CHANGE after_colum2 new_column_name  VARCHAR(30) NOT NULL DEFAULT '' COMMENT '测试修改并重命名';  
```