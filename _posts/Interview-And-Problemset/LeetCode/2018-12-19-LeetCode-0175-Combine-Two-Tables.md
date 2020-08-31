---
title: 【LeetCode】0175. Combine Two Tables
date: 2018-12-19
is_modified: false
categories:
- Interview/Problemset
tags:
- LeetCode
- Database
- SQL
- Left Join
--- 

Write a SQL query for a report that provides the following information for each person in the Person table, regardless if there is an address for each of those people:
`FirstName, LastName, City, State`

<!--more-->
<br>

Table:  `Person`

| Column Name | Type     |
|----------------- |--------- |
| PersonId     | int     |
| FirstName       | varchar  |
| LastName        | varchar  |

PersonId is the primary key column for this table.
<br>

Table:  `Address`

| Column Name | Type     |
|----------------- |--------- |
| AddressId        | int          |
| PersonId          | int          |
| City                  | varchar  |
| State                | varchar  |

AddressId is the primary key column for this table.

<br><br>

## 解題邏輯與實作

原來 LeetCode 還有出資料庫的題目阿，不過題目看來不多，而且題目也不太難，所以手癢稍微試了一下。

<br>

### Left Join
這題看起來就是聯合查表的題目，檢查一下兩張 Table 的欄位，發現 **PersonId** 這欄位兩張表都有且命名也相同，所以我選它直接使用 **Using** 來進行聯合。

```sql
SELECT p.FirstName, p.LastName, a.City, a.State 
FROM Person p LEFT OUTER JOIN Address a USING (PersonId);
```
<br><br>

## 其他連結
1. [【LeetCode】0000. 解題目錄](/LeetCode-0000-Contents/)