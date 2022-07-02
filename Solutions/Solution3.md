# ДЗ 3 Множества
https://contest.yandex.ru/contest/27663/enter/

## A. Количество различных чисел
https://contest.yandex.ru/contest/27663/problems/A/

```swift
print(Set(readLine()!.split(separator: " ").map{Int($0)!}).count)
```

## B. Пересечение множеств
https://contest.yandex.ru/contest/27663/problems/B/

```swift
print(Set(readLine()!.split(separator: " ").map{Int($0)!})
  .intersection(Set(readLine()!.split(separator: " ").map{Int($0)!})).sorted().map{String(describing: $0)}.joined(separator: " "))
```

## C. Кубики
https://contest.yandex.ru/contest/27663/problems/C/

```swift
let arrNM = readLine()!.split(separator: " ").map{Int($0)!}
var setN = Set<Int>()
for _ in 0..<arrNM[0] {
    setN.insert(Int(readLine()!)!)
}
var setM = Set<Int>()
for _ in 0..<arrNM[1] {
    setM.insert(Int(readLine()!)!)
}
var set1 = setN.intersection(setM)
print(set1.count)
if set1.count > 0 {
    print(set1.sorted().map{String(describing: $0)}.joined(separator: " "))
} else {
    print()
}
set1 = setN.subtracting(setM)
print(set1.count)
if set1.count > 0 {
    print(set1.sorted().map{String(describing: $0)}.joined(separator: " "))
} else {
    print()
}
set1 = setM.subtracting(setN)
print(set1.count)
if set1.count > 0 {
    print(set1.sorted().map{String(describing: $0)}.joined(separator: " "))
} else {
    print()
}
```

## D. Количество слов в тексте
https://contest.yandex.ru/contest/27663/problems/D/

```swift
var words = Set<String>()
while let str = readLine(), !str.isEmpty {
    words = words.union(Set(str.split(separator: " ").map({String($0)})))
}
print(words.count)
```

## E. OpenCalculator
https://contest.yandex.ru/contest/27663/problems/E/

```swift
var buttons = Set(readLine()!.split(separator: " ").map{Int($0)!})
var number = Int(readLine()!)!

var count = 0
while number > 0 {
    let n = number % 10
    number /= 10
    if !buttons.contains(n) {
        count += 1
        buttons.insert(n)
    }
}
print(count)
```

## F. Инопланетный геном
https://contest.yandex.ru/contest/27663/problems/F/

```swift
let str = readLine()!
var prev = ""
var dic = [String: Int]()
for ch in str {
    if prev == "" {
        prev = String(ch)
        continue
    }
    let gen = "\(prev)\(ch)"
    if !dic.keys.contains(gen) {
        dic[gen] = 0
    }
    dic[gen]! += 1
    prev = "\(ch)"
}

let str2 = readLine()!
prev = ""
var count = 0
for ch in str2 {
    if prev == "" {
        prev = String(ch)
        continue
    }
    let gen = "\(prev)\(ch)"
    if dic.keys.contains(gen) {
        count += dic[gen]!
        dic[gen] = nil
    }
    prev = "\(ch)"
}
print(count)
```

## G. Черепахи
https://contest.yandex.ru/contest/27663/problems/G/

```swift
let n = Int(readLine()!)!
var count = 0
var check = Set<Int>()
for _ in 0..<n {
    let say = readLine()!.split(separator: " ").map{Int($0)!}
    if say.reduce(1, +) == n, say[0] >= 0, say[1] >= 0, !check.contains(say[0]) {
        count += 1
        check.insert(say[0])
    }
}
print(count)
```

## H. Злые свинки
https://contest.yandex.ru/contest/27663/problems/H/

```swift
let n = Int(readLine()!)!
var count = 0
var check = Set<Int>()
for _ in 0..<n {
    let bird = readLine()!.split(separator: " ").map{Int($0)!}
    // считаем только по х
    if !check.contains(bird[0]) {
        count += 1
        check.insert(bird[0])
    }
}
print(count)
```

## I. Полиглоты
https://contest.yandex.ru/contest/27663/problems/I/

```swift
let n = Int(readLine()!)!
var dic = [String: Int]()
for _ in 0..<n {
    let m = Int(readLine()!)!
    for _ in 0..<m {
        let lang = readLine()!
        if !dic.keys.contains(lang) {
            dic[lang] = 0
        }
        dic[lang]! += 1
    }
}
var arr = Array(dic.filter({$0.value == n}).keys)
print(arr.count)
print(arr.joined(separator: "\n"))
print(dic.keys.count)
print(dic.keys.joined(separator: "\n"))
```

## J. Пробежки по Манхэттену
https://contest.yandex.ru/contest/27663/problems/J/

```swift

```
