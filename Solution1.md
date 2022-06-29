# ДЗ 1 Сложность, тестирование, особые случаи
https://contest.yandex.ru/contest/27393/enter/

## A. Кондиционер
https://contest.yandex.ru/contest/27393/problems/A/

```swift
let arr = readLine()!.split(separator: " ").map{Int($0)!}
let tRoom = arr[0]
let tCond = arr[1]
let type = readLine()!

var ans = tRoom
switch type {
case "freeze":
    if tRoom > tCond {
        ans = tCond
    }
case "heat":
    if tRoom < tCond {
        ans = tCond
    }
case "auto": ans = tCond
case "fan": break
default: break
}

print(ans)
```

## B. Треугольник
https://contest.yandex.ru/contest/27393/problems/B/

```swift
var a = Int(readLine()!)!
var b = Int(readLine()!)!
var c = Int(readLine()!)!
if a < b {
    (a, b) = (b, a)
}
if a < c {
    (a, c) = (c, a)
}
if a < b + c {
    print("YES")
} else {
    print("NO")
}
```

## C. Телефонные номера
https://contest.yandex.ru/contest/27393/problems/C/ 

```swift
func newNumber(_ str: String) {
    var new = ""
    var isPlus = false
    for c in str {
        if c == "+" {
            isPlus = true
            continue
        }
        if c == "7", isPlus {
            isPlus = false
            new += "8"
            continue
        }
        if "()-".contains(c) {
            continue
        }
        new += String(c)
    }
    if new.count == 7 {
        new = "8495\(new)"
    }
    arr.append(new)
}

var arr = [String]()

newNumber(readLine()!)
newNumber(readLine()!)
newNumber(readLine()!)
newNumber(readLine()!)

for i in 1...3 {
    if arr[0] == arr[i] {
        print("YES")
    } else {
        print("NO")
    }
}
```

## D. Уравнение с корнем
https://contest.yandex.ru/contest/27393/problems/D/

```swift
let a = Int(readLine()!)!
let b = Int(readLine()!)!
let c = Int(readLine()!)!

let ansNO = "NO SOLUTION"
let ansMANY = "MANY SOLUTIONS"

if c < 0 {
    print(ansNO)
} else if a == 0 {
    if c * c - b == 0 {
        print(ansMANY)
    } else {
        print(ansNO)
    }
} else {
    if (c * c - b) % a == 0 {
        print((c * c - b) / a)
    } else {
        print(ansNO)
    }
}
```

## E. Скорая помощь
https://contest.yandex.ru/contest/27393/problems/E/ 
```swift
// проверка на соответствие старых данных и заполнение глоб переменных
func isOkOldData(flat: Int, entrance: Int, floor: Int, floors: Int) -> Bool {
    guard floors >= floor else {
        return false // этажность дома не должно быть меньше переданного этажа
    }
    // всего этажей до квартиры - как бы один подъезд
    floorIfOneEntrance = floor + (entrance - 1) * floors
    if floorIfOneEntrance > flat {
        return false // на этаже меньше одной кв
    }
    // количество квартир на этаже
    flatsPerFloor = flat / floorIfOneEntrance + (flat % floorIfOneEntrance == 0 ? 0 : 1)
    if flatsPerFloor * (floorIfOneEntrance - 1) >= flat {
        return false // кв не на своем этаже
    }
    return true
}

let arr = readLine()!.split(separator: " ").map{Int($0)!}
let flatNumber = arr[0] // искомая квартира
let numberOfFloors = arr[1] // количество этажей в доме
let oldFlatNumber = arr[2] // квартира из прошлого
let oldEntrance = arr[3] // подъезд из прошлого
let oldFloor = arr[4] // этаж из прошлого

// глобальные
var floorIfOneEntrance = 0
var flatsPerFloor = 0
var entrance = -1 // ищем подъезд
var floor = -1 // ищем этаж

if isOkOldData(flat: oldFlatNumber, entrance: oldEntrance, floor: oldFloor, floors: numberOfFloors) {
    if floorIfOneEntrance > 1 {
        // здесь возможны варианты - определим границы
        let left = oldFlatNumber / floorIfOneEntrance + (oldFlatNumber % floorIfOneEntrance == 0 ? 0 : 1)
        let right = (oldFlatNumber - 1) / (floorIfOneEntrance - 1)
        // найдем для каждого варианта свой подъезд и этаж
        // если для всех вариантов будет один подъезд или этаж это можно использовать
        for pFloor in left...right {
            let allFloors = (flatNumber + pFloor - 1) / pFloor
            if entrance == -1 {
                entrance = (allFloors + numberOfFloors - 1) / numberOfFloors
            } else if entrance != 0, entrance != (allFloors + numberOfFloors - 1) / numberOfFloors {
                entrance = 0
            }
            if floor == -1 {
                floor = allFloors % numberOfFloors == 0 ? numberOfFloors : allFloors % numberOfFloors
            } else if floor != 0, floor != (allFloors % numberOfFloors == 0 ? numberOfFloors : allFloors % numberOfFloors) {
                floor = 0
            }
            if floor == 0, entrance == 0 {
                break
            }
        }
    } else if floorIfOneEntrance == 1 {
        if oldFlatNumber >= flatNumber {
            entrance = 1
            floor = 1
        } else if oldFlatNumber * numberOfFloors >= flatNumber {
            entrance = 1
            floor = 0
        } else {
            floor = 0
            entrance = 0
        }
    }
    if numberOfFloors == 1 {
        floor = 1
    }
}

print(entrance, floor)
```

## F. Расстановка ноутбуков
https://contest.yandex.ru/contest/27393/problems/F/ 

```swift
let arr = readLine()!.split(separator: " ").map{(Int($0)!)}
let a1 = arr[0]
let a2 = arr[1]
let b1 = arr[2]
let b2 = arr[3]

// в лоб
var res1 = a1 + b1
var res2 = max(a2, b2)
var res3 = res1 * res2
if res3 > ((a1 + b2) * max(a2, b1)) {
    res1 = a1 + b2
    res2 = max(a2, b1)
    res3 = res1 * res2
}
if res3 > ((a2 + b1) * max(a1, b2)) {
    res1 = a2 + b1
    res2 = max(a1, b2)
    res3 = res1 * res2
}
if res3 > ((a2 + b2) * max(a1, b1)) {
    res1 = a2 + b2
    res2 = max(a1, b1)
    res3 = res1 * res2
}
print(res1, res2)
```

## G. Детали
https://contest.yandex.ru/contest/27393/problems/G/ 

```swift
let arr = readLine()!.split(separator: " ").map{(Int($0)!)}
let n = arr[0] // начальная масса
let k = arr[1] // заготовки
let m = arr[2] // деталь

var count = 0
var rest = n
for _ in 0..<n {
    if rest < k {
        break
    }
    rest -= k
    count += k / m
    rest += (k % m)
}
print(count)
```

## H. Метро
https://contest.yandex.ru/contest/27393/problems/H/

```swift
let i1 = Int(readLine()!)!
let i2 = Int(readLine()!)!
let n1 = Int(readLine()!)!
let n2 = Int(readLine()!)!

let resMax = min(n1 + (n1 + 1) * i1, n2 + (n2 + 1) * i2)
let resMin = max(n1 + (n1 - 1) * i1, n2 + (n2 - 1) * i2)
if resMin > resMax {
    print(-1)
} else {
    print(resMin, resMax)
}
```

## I. Узник замка Иф
https://contest.yandex.ru/contest/27393/problems/I/ 

```swift
var arr = [(Int, Int)]()
arr.append((Int(readLine()!)!, 1))
arr.append((Int(readLine()!)!, 1))
arr.append((Int(readLine()!)!, 1))
arr.append((Int(readLine()!)!, 0))
arr.append((Int(readLine()!)!, 0))

arr.sort(by: {a, b in
            if a.0 == b.0 {
                return a.1 > b.1
            } else {
                return a.0 < b.0
            }})

var ans = "YES"
var count = 0
for i in arr {
    if i.1 == 0 {
       if count == 0 {
            ans = "NO"
       } else {
            count -= 1
       }
    } else {
        count += 1
    }
}
print(ans)
```

## J. Система линейных уравнений - 2
https://contest.yandex.ru/contest/27393/problems/J/

```swift
import Foundation
//J. Система линейных уравнений - 2
// Если система не имеет решений, то программа должна вывести единственное число 0.
// = 0 - не имеет решений
// Если система имеет бесконечно много решений, каждое из которых имеет вид y=kx+b, то программа должна вывести число 1, а затем значения k и b.
// = 1 k b - имеет бесконечно много решений, каждое из которых имеет вид y=kx+b
// Если система имеет единственное решение (x0,y0), то программа должна вывести число 2, а затем значения x0 и y0.
// = 2 x y - имеет единственное решение (x0,y0)
// Если система имеет бесконечно много решений вида x=x0, y — любое, то программа должна вывести число 3, а затем значение x0.
// = 3 x - имеет бесконечно много решений вида x=x0, y — любое
// Если система имеет бесконечно много решений вида y=y0, x — любое, то программа должна вывести число 4, а затем значение y0.
// = 4 y - имеет бесконечно много решений вида x=x0, y — любое
// Если любая пара чисел (x,y) является решением, то программа должна вывести число 5.
// = 5 - любая пара чисел (x,y) является решением

func solutions(_ arr: [Float]) -> [String] {
    var (a, b, c, d, e, f) = (arr[0], arr[1], arr[2], arr[3], arr[4], arr[5])
    let determinant = a * d - b * c
    if determinant == 0 {
        if a == 0, c == 0 {
            if b == 0, d == 0 {
                if e == 0, f == 0 {
                    return ["5"]
                }
                return ["0"]
            }
            if e * d != f * b {
                return ["0"]
            } else {
                if b != 0 {
                    return ["4", String(format: "%.5f", e / b)]
                } else {
                    return ["4", String(format: "%.5f", f / d)]
                }
            }
        }
        if b == 0, d == 0 {
            if e * c != f * a {
                return ["0"]
            } else {
                if a != 0 {
                    return ["3", String(format: "%.5f", e / a)]
                } else {
                    return ["3", String(format: "%.5f", f / c)]
                }
            }
        }
        if a != 0 {
            let koef = c / a
            c = 0
            d -= koef * b
            f -= koef * e
            if d == 0, f == 0 {
                return ["1", String(format: "%.5f", -a / b), String(format: "%.5f", e / b)]
            }
        } else {
            let koef = a / c
            a = 0
            b -= koef * d
            e -= koef * f
            if b == 0, e == 0 {
                return ["1", String(format: "%.5f", -c / d), String(format: "%.5f", f / d)]
            }
        }
        return ["0"]
    } else {
        if d != 0 {
            let x = (e - b * f / d) / (a - b * c / d)
            let y = (f - c * x) / d
            return ["2", String(format: "%.5f", x), String(format: "%.5f", y)]
        } else {
            let x = (f - d * e / b) / (c - d * a / b)
            let y = (e - a * x) / b
            return ["2", String(format: "%.5f", x), String(format: "%.5f", y)]
        }
    }
}

let arrABCDEF = [Float(readLine()!)!, Float(readLine()!)!, Float(readLine()!)!, Float(readLine()!)!, Float(readLine()!)!, Float(readLine()!)!]
print(solutions(arrABCDEF).joined(separator: " "))
```
