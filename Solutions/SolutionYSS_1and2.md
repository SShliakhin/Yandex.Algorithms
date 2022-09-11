Темы:
1. Сложность, тестирование, особые случаи
2. Линейный поиск

# ДЗ Занятие 1 (Разбор случаев, линейный поиск)
https://contest.yandex.ru/contest/39359/problems/

## A. Строительство лесенок
https://contest.yandex.ru/contest/39359/problems/A/

```swift
let parts = Int(readLine()!)!
var sum = 0
for i in 1...parts + 1 {
    sum += i
    if sum > parts {
        print(i - 1)
        break
    }
}
```

## B. Канонический путь
https://contest.yandex.ru/contest/39359/problems/B/

```swift
import Foundation

let arrayUnix = readLine()!.components(separatedBy: "/")
var ans = ""
var countContinue = 0
for element in arrayUnix.reversed() {
    if !element.isEmpty {
        if element == ".." {
            countContinue += 1
            continue
        }
        if element == "." {
            continue
        }
        if countContinue > 0 {
            countContinue -= 1
            continue
        }
        ans = element + (ans.isEmpty ? "" : "/") + ans
    }
}
print("/" + ans)
```

## C. Купить и продать
https://contest.yandex.ru/contest/39359/problems/C/

```swift
import Foundation

let days = Int(readLine()!)!
let cost = readLine()!.split(separator: " ").map{Int($0)!}

var bestBuyDay = 0
var bestSellDay = 0
var minCostDay = 0

for day in 1..<days {
	if cost[minCostDay] > cost[day] {
        minCostDay = day
        continue
    }
    if cost[bestSellDay] * cost[minCostDay] < cost[bestBuyDay] * cost[day] {
        bestBuyDay = minCostDay
        bestSellDay = day
    }
    
}

if bestBuyDay == 0, bestSellDay == 0 {
    print(0,0)
} else {
    print(bestBuyDay + 1, bestSellDay + 1)
}
```

## D. Разница во времени
https://contest.yandex.ru/contest/39359/problems/D/

```swift
import Foundation

let countTrains = Int(readLine()!.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines))!
let arrayTrains = readLine()!.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines).components(separatedBy: " ")
var arrayTimes = [Int]()
for time in arrayTrains {
    let arrayTime = time.components(separatedBy: ":")
    arrayTimes.append(Int(arrayTime[0])! * 60 + Int(arrayTime[1])!)
}
arrayTimes.sort(by: <)
arrayTimes.append(arrayTimes[0] + 24 * 60)
//var minTime = arrayTimes[0] + 24 * 60 - arrayTimes[countTrains - 1]
var minTime = 60 * 24
for i in 1...countTrains {
    let curDiff = arrayTimes[i] - arrayTimes[i - 1]
    if curDiff < minTime {
        minTime = curDiff
    }
}
print(minTime)
```

## E. Сломай палиндром
https://contest.yandex.ru/contest/39359/problems/E/

```swift
import Foundation

var pallidrome = readLine()!
if pallidrome.count == 1 {
    print("")
    exit(0)
}
var middle = pallidrome.index(pallidrome.startIndex, offsetBy: pallidrome.count / 2)
var index = pallidrome[pallidrome.startIndex..<middle].firstIndex { $0 != "a" }

if let index = index {
    let index2 = pallidrome.index(after: index)
    pallidrome = pallidrome[..<index] + "a" + pallidrome[index2...]
} else {
    pallidrome = pallidrome[..<pallidrome.index(before: pallidrome.endIndex)] + "b"
}
print(pallidrome)
```
