---
layout: post
title: "[Go] MySQL 연결하고 값 JSON으로 출력하기"
date: "2021-03-08 20:23"
updated: []
categories: [Go]
tags: [MySQL, json]
---

**[1] MySQL package 설치**
```bash
go get github.com/go-sql-driver/mysql
```
<br>
**[2] 코드 작성**
```go
package main

import (
	"fmt"
	"database/sql" // mysql
    _ "github.com/go-sql-driver/mysql" // mysql
	"log"
	"encoding/json" // json	
)

func main() {

	// DB 객체 생성
	db, err := sql.Open("mysql", "아이디:비밀번호@@tcp(IP:PORT)/DB명")
	if err != nil {
		log.Fatal(err)
	}
	
	// SQL 쿼리 작성
	rows, err := db.Query("SELECT user_id, user_pwd, user_name FROM user_info")
	if err != nil {
		log.Fatal(err)
	}
	defer rows.Close() // defer: return 하기 직전에 실행

	// 쿼리 실행 결과 JSON 형태로 변환
	columns, err := rows.Columns()
	if err != nil {
		log.Fatal(err)
	}
	count := len(columns)
	tableData := make([]map[string]interface{}, 0)
	values := make([]interface{}, count)
	valuePtrs := make([]interface{}, count)
	for rows.Next() {
		for i := 0; i < count; i++ {
		valuePtrs[i] = &values[i]
		}
		rows.Scan(valuePtrs...)
		entry := make(map[string]interface{})
		for i, col := range columns {
			var v interface{}
			val := values[i]
			b, ok := val.([]byte)
			if ok {
				v = string(b)
			} else {
				v = val
			}
			entry[col] = v
		}
		tableData = append(tableData, entry)
	}
	jsonString, err := json.Marshal(tableData)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(string(jsonString))
}
```
<br>
<br>
\* 참고<br>
[MySQL 연동: 예제로 배우는 Go 프로그래밍](http://golang.site/go/article/107-MySql-사용---쿼리){:target="_blank"}<br>
[JSON 만들기: Stack Overflow](https://stackoverflow.com/questions/19991541/dumping-mysql-tables-to-json-with-golang){:target="_blank"}

