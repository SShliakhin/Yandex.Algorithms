# ДЗ 2 Линейный поиск
https://contest.yandex.ru/contest/27472

## A. Возрастает ли список?
https://contest.yandex.ru/contest/27472/problems/A/ 

```swift
let arr = readLine()!.split(separator: " ").map{Int($0)!}
var ans = "YES"
for i in 1..<arr.count {
    if arr[i - 1] >= arr[i] {
        ans = "NO"
        break
    }
}
print(ans)
```

## B. Определить вид последовательности
https://contest.yandex.ru/contest/27472/problems/B/

```swift
var dic = ["CONSTANT": true, "ASCENDING": true, "WEAKLY ASCENDING": true, "DESCENDING": true, "WEAKLY DESCENDING": true, "RANDOM": true]
var first = Int(readLine()!)!
for _ in 0...Int.max {
    let second = Int(readLine()!)!
    if second == -2000000000 {
        break
    }
    if second == first {
        dic["ASCENDING"] = false
        dic["DESCENDING"] = false
    } else if second > first {
        dic["WEAKLY DESCENDING"] = false
        dic["CONSTANT"] = false
        dic["DESCENDING"] = false
    } else {
        dic["WEAKLY ASCENDING"] = false
        dic["CONSTANT"] = false
        dic["ASCENDING"] = false
    }
    first = second
}
if dic.filter({$0.value == true}).count == 1 {
    print("RANDOM")
} else {
    if dic["CONSTANT"]! {
        print("CONSTANT")
    } else if dic["ASCENDING"]! {
        print("ASCENDING")
    } else if dic["DESCENDING"]! {
        print("DESCENDING")
    } else {
        dic["RANDOM"] = false
        for (key, _) in dic.filter({$0.value == true}) {
            print(key)
        }
    }
}
```

## C. Ближайшее число
https://contest.yandex.ru/contest/27472/problems/C/

```swift
let n = Int(readLine()!)!
var arr = readLine()!.split(separator: " ").map{Int($0)!}
let number = Int(readLine()!)!

arr.sort()
var left = 0
var right = n - 1

while right > left + 1 {
    let mid = (right + left) / 2
    if number > arr[mid] {
        left = mid
    } else {
        right = mid
    }
}

if arr[right] == number {
    print(number)
} else {
    if arr[right] - number <= number - arr[left] {
        print(arr[right])
    } else {
        print(arr[left])
    }
}
```

## D. Больше своих соседей
https://contest.yandex.ru/contest/27472/problems/D/

```swift
var arr = readLine()!.split(separator: " ").map{Int($0)!}
var count = 0
if arr.count > 2 {
    for i in 1..<arr.count - 1 {
        if arr[i] > arr[i - 1], arr[i] > arr[i + 1] {
            count += 1
        }
    }
}
print(count)
```

## E. Чемпионат по метанию коровьих лепешек
https://contest.yandex.ru/contest/27472/problems/E/

```swift
let n = Int(readLine()!)!
var arr = readLine()!.split(separator: " ").map{Int($0)!}
var res = n - 1
var winner = arr[0]
var possibleVasya = 0
for i in 1..<n - 1 {
    let cur = arr[i]
    if winner < cur {
        winner = cur
        possibleVasya = 0
    } else {
        if cur % 10 != 0, cur % 5 == 0, possibleVasya < cur, cur > arr[i + 1] {
            possibleVasya = cur
        }
    }
}
// крайний не рассмотренный случай
if winner < arr[n - 1] || possibleVasya == 0 {
    res = 0
} else {
    arr.sort(by: >)
    for i in 0..<n {
        if arr[i] == possibleVasya {
            res = i + 1
            break
        }
    }
}

print(res)
```

## F. Симметричная последовательность
https://contest.yandex.ru/contest/27472/problems/F/

```swift
let n = Int(readLine()!)!
var arr = readLine()!.split(separator: " ").map{Int($0)!}

func check(_ arr: [Int]) -> Int {
    var res = 0
    var l = 0
    var r = arr.count - 1
    while l < r {
        if arr[l] != arr[r] {
            res += 1
            r = arr.count - 1
            l = res
        } else {
            l += 1
            r -= 1
        }
    }
    return res
}

let pos = check(arr)
print(pos)
if pos != 0 {
    print(arr[0..<pos].reversed().map{String(describing: $0)}.joined(separator: " "))
}
```

## G. Наибольшее произведение двух чисел
https://contest.yandex.ru/contest/27472/problems/G/

```swift
var arr = [0, 0, 0, 0] + readLine()!.split(separator: " ").map{Int($0)!}

if arr.count == 6 {
    // имеем два числа, просто выводим им
    let a = arr[4]
    let b = arr[5]
    if a > b {
        print(b, a)
    } else {
        print(a, b)
    }
} else {
    var max1 = 0
    var max2 = 1
    var min1 = 2
    var min2 = 3
    
    for i in 4..<arr.count {
        let value = arr[i]
        if value > 0 {
            if value >= arr[max1] {
                max2 = max1
                max1 = i
            } else if value >= arr[max2] {
                max2 = i
            }
        } else if value < 0 {
            if value <= arr[min1] {
                min2 = min1
                min1 = i
            } else if value <= arr[min2] {
                min2 = i
            }
        } else {
            if value >= arr[max1] {
                max2 = max1
                max1 = i
            } else if value >= arr[max2] {
                max2 = i
            }
            if value <= arr[min1] {
                min2 = min1
                min1 = i
            } else if value <= arr[min2] {
                min2 = i
            }
        }
    }
    
    // проверка на наличие индексов
    if max1 > 3, max2 > 3, min1 > 3, min2 > 3 {
        if arr[max1] * arr[max2] >= arr[min1] * arr[min2] {
            print(arr[max2], arr[max1])
        } else {
            print(arr[min1], arr[min2])
        }
    } else if max1 > 3, max2 > 3 {
        print(arr[max2], arr[max1])
    } else {
        print(arr[min1], arr[min2])
    }
    
}
```

## H. Наибольшее произведение трех чисел
https://contest.yandex.ru/contest/27472/problems/H/

```swift
var arr = readLine()!.split(separator: " ").map{Int($0)!}

if arr.count == 3 {
    print(arr.map{String(describing: $0)}.joined(separator: " "))
} else {
    var h = max(arr[0], arr[1])
    var l = min(arr[0], arr[1])
    var h2s = Array(arr[0...1])
    var h2 = h2s.reduce(1, *)
    var l2s = h2s
    var l2 = h2
    var h3s = Array(arr[0...2])
    var h3 = h3s.reduce(1, *)
    
    for i in 2..<arr.count {
        let current = arr[i]
        // проверка на изменение результата и состава h3
        let ch2 = current * h2
        let cl2 = current * l2
        if ch2 > h3 || cl2 > h3 {
            if ch2 >= cl2 {
                h3 = ch2
                h3s = h2s + [current]
            } else {
                h3 = cl2
                h3s = l2s + [current]
            }
        }
        // проверка на изменение результата и состава h2 и l2
        let ch = current * h
        let cl = current * l
        if ch > h2 || cl > h2 {
            if ch >= cl {
                h2 = ch
                h2s = [h, current]
            } else {
                h2 = cl
                h2s = [l, current]
            }
        }
        if ch < l2 || cl < l2 {
            if ch < cl {
                l2 = ch
                l2s = [h, current]
            } else {
                l2 = cl
                l2s = [l, current]
            }
        }
        
        // минимальное и максимальное
        if current > h {
            h = current
        }
        if current < l {
            l = current
        }
    }
    
    print(h3s.map{String(describing: $0)}.joined(separator: " "))
}
```

## I. Сапер
https://contest.yandex.ru/contest/27472/problems/I/

```swift
let arrNMK = readLine()!.split(separator: " ").map{Int($0)!}
let nn = arrNMK[0]
let mm = arrNMK[1]
// подготовим поле
var field = Array(repeating: Array(repeating: 0, count: mm), count: nn)

func putNumbers(for bomb: [Int]) {
    let deltas = [[-1, -1],[-1, 0], [-1, 1],
                  [0, -1],          [0, 1],
                  [1, -1], [1, 0],  [1, 1]]
    // координаты с учетом индекса
    let n = bomb[0] - 1
    let m = bomb[1] - 1
    
    field[n][m] = -1 // сама бомба
    for d in deltas {
        let pn = d[0] + n
        let pm = d[1] + m
        if pn < nn, pn >= 0, pm < mm, pm >= 0, field[pn][pm] != -1 {
            field[pn][pm] += 1
        }
    }
}

let k = arrNMK[2]
for _ in 0..<k {
    putNumbers(for: readLine()!.split(separator: " ").map{Int($0)!})
}

for p in field {
    print(p.map{String(describing: $0)}.map{$0 == "-1" ? "*" : $0 }.joined(separator: " "))
}
```

## J. Треугольник Максима
https://contest.yandex.ru/contest/27472/problems/J/

```swift
```
