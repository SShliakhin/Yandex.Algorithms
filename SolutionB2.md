# ДЗ B2 Линейный поиск
https://contest.yandex.ru/contest/28738/enter/

## A. Количество равных максимальному
https://contest.yandex.ru/contest/28738/problems/A/

```swift
var number = -1
var maxNumber = 0
var count = 0
while number != 0 {
    number = Int(readLine()!)!
    if number > maxNumber {
        maxNumber = number
        count = 0
    }
    if maxNumber == number {
        count += 1
    }
}
print(count)
```

## B. Дома и магазины
https://contest.yandex.ru/contest/28738/problems/B/

```swift
let arr = readLine()!.split(separator: " ").map{ Int($0)! }
var arrR = Array(repeating: 100, count: 10)
var indexShop = -1
for (index, value) in arr.enumerated() {
    if value == 2 {
        indexShop = index
    }
    if value == 1, indexShop != -1, arrR[index] > index - indexShop {
        arrR[index] = index - indexShop
    }
}
indexShop = -1
for (index, value) in arr.reversed().enumerated() {
    if value == 2 {
        indexShop = index
    }
    if value == 1, indexShop != -1,
       arrR[arrR.count - 1 - index] > index - indexShop {
        arrR[arrR.count - 1 - index] = index - indexShop
    }
}

print(arrR.filter{ $0 != 100}.max()!)
```

## C. Изготовление палиндромов
https://contest.yandex.ru/contest/28738/problems/C/

```swift
let word = readLine()!.map{String($0)}
let med = (word.count - 1) / 2
var count = 0
for (index, value) in word.enumerated() {
    if index > med {
        break
    }
    if value != word[word.count - 1 - index] {
        count += 1
    }
}
print(count)
```

## D. Лавочки в атриуме
https://contest.yandex.ru/contest/28738/problems/D/

```swift
let arr = readLine()!.split(separator: " ").map{Int($0)!}
let arr2 = readLine()!.split(separator: " ").map{Int($0)!}
let mid = arr[0] / 2

if let indexMid = arr2.firstIndex(of: mid), arr[0] % 2 != 0 {
    print(arr2[indexMid])
} else {
    print(arr2.filter({$0 < mid}).max()!,arr2.filter({$0 >= mid}).min()!)
}
```

## E. Дипломы в папках
https://contest.yandex.ru/contest/28738/problems/E/

```swift
let n = Int(readLine()!)!
let arr = readLine()!.split(separator: " ").map{ Int($0)! }

print(arr.reduce(0, +) - arr.max()!)
```
