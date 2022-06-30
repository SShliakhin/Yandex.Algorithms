# ДЗ B3 Множества
https://contest.yandex.ru/contest/28964/enter/

## A. Количество совпадающих
https://contest.yandex.ru/contest/28964/problems/A/

```swift
import Foundation

let setArray1 = Set(readLine()!.split(separator: " ").map({Int($0)!}))
let setArray2 = Set(readLine()!.split(separator: " ").map({Int($0)!}))

print(setArray1.intersection(setArray2).count)
```

## B. Встречалось ли число раньше
https://contest.yandex.ru/contest/28964/problems/B/

```swift
import Foundation

let arr  = readLine()!.split(separator: " ").map({Int($0)!})
var dir = [Int:Bool]()
arr.forEach { item in
    if let _ = dir[item] {
        print("YES")
    } else {
        dir[item] = true
        print("NO")
    }
}
```

## C. Уникальные элементы
https://contest.yandex.ru/contest/28964/problems/C/

```swift
import Foundation

let arr  = readLine()!.split(separator: " ").map({Int($0)!})
var dir = [Int:[Int]]()

arr.enumerated().forEach { index, item in
    if let _ = dir[item] {
    } else {
        dir[item] = [Int]()
    }
    dir[item]!.append(index)
}

(dir.filter { $0.value.count == 1 })
    .sorted(by: { $0.value[0] < $1.value[0] } )
    .forEach { print($0.key, terminator: " ") }
```

## D. Угадай число
https://contest.yandex.ru/contest/28964/problems/D/

```swift
import Foundation

let maxNumber = Int(readLine()!)!
var setYES = Set<Int>(1...maxNumber)

var ans = readLine()!
while ans != "HELP" {
    let arr = Set(ans.split(separator: " ").map({Int($0)!}))
    ans = readLine()!
    if ans == "YES" {
        setYES = setYES.intersection(arr)
    } else {
        setYES.subtract(arr)
    }
    ans = readLine()!
}

setYES.sorted(by: <).forEach { item in
    print(item, terminator: " ")
}
```

## E. Автомобильные номера
https://contest.yandex.ru/contest/28964/problems/E/

```swift
import Foundation

let m = Int(readLine()!)!
var arrM = [Set<String>]()
for _ in 1...m {
    let guess = Set(readLine()!.map({String($0)}))
    arrM.append(guess)
}

let n = Int(readLine()!)!
var arrN = [(String, Int)]()
var maxWit = 0
for _ in 1...n {
    let num = readLine()!
    let numSet = Set(num.map({String($0)}))
    var witCnt = 0
    for m in arrM {
        if numSet.intersection(m).count == m.count {
            witCnt += 1
        }
    }
    arrN.append((num, witCnt))
    maxWit = max(maxWit, witCnt)
}

arrN.forEach { num, witCnt in
    if witCnt == maxWit {
        print(num)
    }
}
```
