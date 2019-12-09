---
layout: posts
title: "[iOS/Swift] 객체 배열 정렬"
comments: true
categories: [iOS/Swift]
---


```swift
// 객체 구조는 다음과 같습니다.
class Student {
    var name: String
    var score: Int
    
    init(name: String, score: Int) {
        self.name = name
        self.score = score
    }
}
```



```swift
// 배열을 출력하는 함수
func printArr() {
    for student in students {
        print("\(student.name) - \(student.score)")
    }
}

// 배열 정의
var students = [Student]()
students.append(Student(name: "이", score: 82))
students.append(Student(name: "임", score: 88))
students.append(Student(name: "고", score: 70))
students.append(Student(name: "박", score: 98))
students.append(Student(name: "김", score: 100))
students.append(Student(name: "최", score: 78))
students.append(Student(name: "조", score: 60))

print("== 정렬 전 ==")
printArr()

print("== 이름 오름차순 ==")
students = students.sorted(by: {$0.name < $1.name})
printArr()

print("== 성적 내림차순 ==")
students = students.sorted(by: {$0.score > $1.score})
printArr()
```