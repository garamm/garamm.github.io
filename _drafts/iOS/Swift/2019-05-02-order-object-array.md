---
layout: posts
title: "[iOS/Swift] 객체 배열 정렬하기"
comments: true
categories: [iOS/Swift]
---


```swift
import UIKit

class Student {
    var code: String
    var name: String
    var grade: Int
    
    init(code: String, name: String, grade: Int) {
        self.code = code
        self.name = name
        self.grade = grade
    }
}

var students = [Student]()
students.append(Student(code: "a-121", name: "garam", grade: 3))
students.append(Student(code: "a-112", name: "mimi", grade: 1))
students.append(Student(code: "b-132", name: "jhon", grade:6))
students.append(Student(code: "b-111", name: "jihee", grade: 4))


print("정렬 전")
for student in students {
    print("\(student.code), \(student.name), \(student.grade)")
}

print("\n\n코드 오름차순")
students = students.sorted(by: {$0.code < $1.code})
for student in students {
    print("\(student.code), \(student.name), \(student.grade)")
}

print("\n\n학년 내림차순")
students = students.sorted(by: {$0.grade > $1.grade})
for student in students {
    print("\(student.code), \(student.name), \(student.grade)")
}

print("\n\n배열 역순으로 정렬")
students.reverse()
for student in students {
    print("\(student.code), \(student.name), \(student.grade)")
}
```
