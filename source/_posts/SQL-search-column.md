---
title: SQL search column
date: 2025-09-30 19:37:39
tags:
  - SQL
categories:
  - SQL
  - Database
cover: /images/cover.jpg  # 站内图片
---

# ORACLE
```SQL
##Useful Sql
select
    column_name,
    data_type,
    data_length,
    DATA_PRECISION,
    DATA_SCALE
from all_tab_columns -- user have permiss/当前用户有权限访问的表, dba_tab_columns/数据库中所有表（需要 DBA 权限）
where table_name=upper('tablename');
##Useful Sql
select
	t.table_name,  -- tablename/表名
	t.column_name,-- columnname/字段名
	t.data_type,  -- columntype/字段类型
	t.data_length  -- columnsize/字段长度
from
	user_tab_columns t -- user having table/当前用户拥有的表
where
	t.table_name='tablename';
```

