# ДЗ B4 Словари и сортировка подсчётом
https://contest.yandex.ru/contest/28970/enter/

## A. Толя-Карп и новый набор структур, часть 2
https://contest.yandex.ru/contest/28970/problems/A/

```swift
import Foundation

let n = Int(readLine()!)!
var dir = [Int:Int]()
for _ in 0..<n {
    let arr = readLine()!.split(separator: " ").map({Int($0)!})
    let color = arr.first!
    let num = arr.last!
    if dir[color] == nil {
        dir[color] = 0
    }
    dir[color]! += num
}

if !dir.isEmpty {
    dir.sorted(by: {$0.key < $1.key}).forEach { (key, value) in
        print(key, value)
    }
}
```

## B. Выборы в США
https://contest.yandex.ru/contest/28970/problems/B/

```swift
import Foundation

var dir = [String: Int]()
while let election = readLine(), !election.isEmpty {
    let razbor = election.split(separator: " ").map({String($0)})
    let candidat = razbor.first!
    let voices = Int(razbor.last!)!
    if dir[candidat] == nil {
        dir[candidat] = 0
    }
    dir[candidat]! += voices
}

dir.sorted(by: {$0.key < $1.key}).forEach({ print($0.key, $0.value) })
```

## C. Частотный анализ
https://contest.yandex.ru/contest/28970/problems/C/

```swift
import Foundation

var dir = [String: Int]()
while let str = readLine(), !str.isEmpty {
    let razbor = str.split(separator: " ").map({ String($0) })
    razbor.forEach { item in
        if dir[item] == nil {
            dir[item] = 0
        }
        dir[item]! += 1
    }
}

dir.sorted(by: { (item1, item2) in
    if item1.value == item2.value {
        return item1.key < item2.key
    } else {
        return item1.value > item2.value
    }
}).forEach({ print($0.key) })
```

## D. Выборы Государственной Думы
https://contest.yandex.ru/contest/28970/problems/D/

```swift
import Foundation

var dir = [String: Int]()
var arr = [(name: String, tur1: Int, tur2: Int, less: Double)]()
var order = [String]()
var allVoices = 0
let num = 450
while let str = readLine(), !str.isEmpty {
    let razbor = str.split(separator: " ").map({ String($0) })
    let voices = Int(razbor.last!)!
    let partiya = razbor[0...razbor.count - 2].joined(separator: " ")
    if dir[partiya] == nil {
        dir[partiya] = 0
        order.append(partiya)
    }
    dir[partiya]! += voices
    allVoices += voices
}
let chastnoe = Double(allVoices) / Double(num)
var tur1free = 450

if Int(chastnoe) > 0 {
    arr = dir.map({ item in
        let intV = Int(Double(item.value) / chastnoe)
        tur1free -= intV
        return (item.key, intV, 0, Double(item.value) / chastnoe - Double(intV))
    })
    arr = arr.sorted(by: { $0.less > $1.less })
} else {
    arr = dir.map({ item in
        return (item.key, 0, 0, 0)
    })
}

var index = 0
let cnt = arr.count
while tur1free > 0 {
    arr[index % cnt].tur2 += 1
    tur1free -= 1
    index += 1
}

arr.forEach({dir[$0.name]! = $0.tur1 + $0.tur2 })

order.forEach({print($0, dir[$0]!)})
```

## E. Форум
https://contest.yandex.ru/contest/28970/problems/E/

```swift
import Foundation

let n = Int(readLine()!)! // количество сообщений
var arr = Array(repeating: 0, count: n + 1) // сообщения к чему привязаны
var arrT = Array(repeating: "", count: n + 1) // темы в порядке появления
var arrC = Array(repeating: 0, count: n + 1) // сколько привязано
var maxCnt = 0
for index in 1...n {
    let cod = Int(readLine()!)!
    if cod == 0 {
        // это новая тема
        let str1 = readLine()! // тема
        arr[index] = 0
        arrT[index] = str1
        let _ = readLine()! // сообщение
        arrC[index] += 1
        maxCnt = max(maxCnt, arrC[index])
    } else {
        arr[index] = cod
        arrT[index] = "" // не тема
        let _ = readLine()! // сообщение
        // надо найти индекс
        var i = cod
        while arr[i] != 0 {
            i = arr[i]
        }
        arrC[i] += 1
        maxCnt = max(maxCnt, arrC[i])
    }
}

for (index, val) in arrC.enumerated() {
    if val == maxCnt {
        print(arrT[index])
        break
    }
}
```
