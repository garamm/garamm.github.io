---
layout: post
title: "[iOS/Swift] UserDefaults"
category: swift
date: "2019-08-16"
---

---
저장
```swift
UserDefaults.standard.set("값입니다.", forKey: "mykey")
```

---
불러오기
```swift
let mykey = UserDefaults.standard.string(forKey: "mykey")
```

---
객체배열 저장

Student.swift
```swift
import Foundation

class Student: NSObject, NSCoding {
    var id: Int
    var name: String
    
    
    init(id: Int, name: String) {
        self.id = id
        self.name = name
        
    }
    
    required convenience init(coder aDecoder: NSCoder) {
        let id = aDecoder.decodeInteger(forKey: "id")
        let name = aDecoder.decodeObject(forKey: "name") as! String
        self.init(id: id, name: name)
    }
    
    func encode(with aCoder: NSCoder) {
        aCoder.encode(id, forKey: "id")
        aCoder.encode(name, forKey: "name")
    }
}
```

ViewController.swift
```swift
import UIKit

class ViewController: UIViewController {
    
    let userDefaults = UserDefaults.standard

    override func viewDidLoad() {
        super.viewDidLoad()
        //savePlaces()
        loadPlaces()
    }

    func savePlaces(){
        let students = [Student(id: 1, name: "orora"), Team(id: 2, name: "garam")]
        let encodedData: Data = NSKeyedArchiver.archivedData(withRootObject: students)
        userDefaults.set(encodedData, forKey: "students")
        userDefaults.synchronize()
    }
    
    func loadPlaces() {
        if let decoded  = userDefaults.object(forKey: "students") as? Data {
            let decodedStudents = NSKeyedUnarchiver.unarchiveObject(with: decoded) as! [Student]
            if decodedStudents.count == 0 {
                print("count == 0")
            }
        } else {
            print("empty")
        }
    }
        
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}
```