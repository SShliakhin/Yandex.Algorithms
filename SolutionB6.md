# ДЗ B6 Бинарный поиск
https://contest.yandex.ru/contest/29188/enter/

## A. Быстрый поиск в массиве
https://contest.yandex.ru/contest/29188/problems/A/

```swift
import Foundation

func lBSearch(for number: Int, from arr: [Int]) -> Int {
    // первое подходящее
    var l = 0
    var r = arr.count - 1
    while l < r {
        let m = (l + r) / 2
        if arr[m] >= number {
            r = m
        } else {
            l = m + 1
        }
    }
    return l
}

func rBSearch(for number: Int, from arr: [Int]) -> Int {
    // последнее подходящее
    var l = 0
    var r = arr.count - 1
    while l < r {
        let m = (l + r + 1) / 2
        if arr[m] <= number {
            l = m
        } else {
            r = m - 1
        }
    }
    return l
}

let _ = readLine()
let arr = readLine()!.split(separator: " ").map({Int($0)!}).sorted(by: <)
let q = Int(readLine()!)!
var ans = Array(repeating: 0, count: q)

for index in 0..<q {
    let arrSE = readLine()!.split(separator: " ").map({Int($0)!})
    let start = arrSE.first!
    let end = arrSE.last!
    
    // TODO: сколько чисел между s и e включительно
    // найти индексы левой и правой границ, вывести разницу между ними + 1
    
    let l = lBSearch(for: start, from: arr)
    let r = rBSearch(for: end, from: arr)
    var preAns = r - l + 1
    if l == r {
        if arr[l] <= end, arr[r] >= start {
        } else {
            preAns = 0
        }
    }
    ans[index] = preAns
}

ans.forEach({print($0, terminator: " ")})
```

## B. Номер левого и правого вхождения
https://contest.yandex.ru/contest/29188/problems/B/

```swift
import Foundation

let _ = readLine()
let arr = [-1] + readLine()!.split(separator: " ").map({Int($0)!})
let _ = readLine()
let q = readLine()!.split(separator: " ").map({Int($0)!})

func lBSearch(for number: Int, from arr: [Int]) -> Int {
    // первое подходящее
    var l = 0
    var r = arr.count - 1
    while l < r {
        let m = (l + r) / 2
        if arr[m] >= number {
            r = m
        } else {
            l = m + 1
        }
    }
    return l
}

func rBSearch(for number: Int, from arr: [Int]) -> Int {
    // последнее подходящее
    var l = 0
    var r = arr.count - 1
    while l < r {
        let m = (l + r + 1) / 2
        if arr[m] <= number {
            l = m
        } else {
            r = m - 1
        }
    }
    return l
}

q.forEach { item in
    let l = lBSearch(for: item, from: arr)
    let r = rBSearch(for: item, from: arr)
    let ansL = arr[l] == item ? l : 0
    let ansR = arr[r] == item ? r : 0
    print(ansL, ansR)
}
```

## C. Корень кубического уравнения
https://contest.yandex.ru/contest/29188/problems/C/

```swift
import Foundation

var arr = readLine()!.split(separator: " ").map({Double($0)!})

let (a, b, c, d) = (arr[0], arr[1], arr[2], arr[3])

func lBSeach(check: (Double) -> Bool) -> Double {
    var l = -1000000.0
    var r = 1000000.0
    let eps = 0.000001
    
    while l + eps < r {
        let m = (l + r) / 2
        if check(m) {
            //print("R:", l, r, m)
            r = m
        } else {
            //print("L:", l, r, m)
            l = m
        }
    }
    
    return l
}

func check(m: Double) -> Bool {
    let ev = a * pow(m, 3) + b * pow(m, 2) + c * m + d
    return a > 0 ? ev > 0 : ev < 0
}

print(lBSeach(check: check(m:)))
```

## D. Вырубка леса
https://contest.yandex.ru/contest/29188/problems/D/

```swift

```

## E. Покрытие K отрезками
https://contest.yandex.ru/contest/29188/problems/E/

```swift
```
