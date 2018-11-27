---
title: "数据库索引类型及使用场景"
date: 2018-11-19T09:00:44+08:00
draft: false
---

- b-tree适合所有的数据类型，支持排序，支持大于、小于、等于、大于或等于、小于或等于的搜索。
- hash 只支持等值查询,特别适用于字段VALUE非常长,例如很长的字符串，并且用户只需要等值搜索，建议使用hash index。
- gin是倒排索引,适合多值类型，例如数组、全文检索、TOKEN。


详细介绍 https://leopard.in.ua/2015/04/13/postgresql-indexes
