# ДЗ 6 Бинарный поиск
https://contest.yandex.ru/contest/27844/enter/

## A. Двоичный поиск
https://contest.yandex.ru/contest/27844/problems/A/

```swift
let arrNK = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNK[0]
let k = arrNK[1]

let arrN = (readLine()!.split(separator: " ").map{Int($0)!}).sorted()
let arrK = readLine()!.split(separator: " ").map{Int($0)!}

func bSearch(for number: Int, from arr: [Int]) -> Int {
    var l = -1
    var r = arr.count - 1
    while r > l + 1 {
        let middle = (l + r) / 2
        if number <= arr[middle] {
            r = middle
        } else {
            l = middle
        }
    }
    return r
}

var ans = [String]()
var dic = [Int: String]()
for i in arrK {
    if !dic.keys.contains(i) {
        if i == arrN[bSearch(for: i, from: arrN)] {
            dic[i] = "YES"
        } else {
            dic[i] = "NO"
        }
    }
    ans.append(dic[i]!)
}
print(ans.map{"\($0)\n"}.joined())
```

## B. Приближенный двоичный поиск
https://contest.yandex.ru/contest/27844/problems/B/

```swift
let arrNK = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNK[0]
let k = arrNK[1]

let arrN = readLine()!.split(separator: " ").map{Int($0)!}
let arrK = readLine()!.split(separator: " ").map{Int($0)!}

func bSearch(for number: Int, from arr: [Int]) -> Int {
    var l = -1
    var r = arr.count - 1
    while r > l + 1 {
        let middle = (l + r) / 2
        if number <= arr[middle] {
            r = middle
        } else {
            l = middle
        }
    }
    return r
}

var ans = [Int]()
var dic = [Int: Int]()
for i in arrK {
    if !dic.keys.contains(i) {
        let index = bSearch(for: i, from: arrN)
        if i == arrN[index] {
            dic[i] = i
        } else {
            // найти ближайшее к i
            if index == 0 {
                dic[i] = arrN[index]
            } else {
                let f = arrN[index]
                let s = arrN[index - 1]
                if f - i < i - s {
                    dic[i] = f
                } else {
                    dic[i] = s
                }
            }
        }
    }
    ans.append(dic[i]!)
}
print(ans.map{"\($0)\n"}.joined())
```

## C. Дипломы
https://contest.yandex.ru/contest/27844/problems/C/

```swift
let arrWHN = readLine()!.split(separator: " ").map{Int($0)!}

func bSearch(from arrWHN: [Int]) -> Int {
    let w = arrWHN[0]
    let h = arrWHN[1]
    let n = arrWHN[2]
    var l = min(w, h)
    var r = max(w, h) * n
    while r > l + 1 {
        let middle = (l + r) / 2
        let res = (middle / w) * (middle / h)
        if n <= res {
            r = middle
        } else {
            l = middle
        }
    }
    return r
}

print(bSearch(from: arrWHN))
```

## D. Космическое поселение
https://contest.yandex.ru/contest/27844/problems/D/

```swift
let arrNABWH = readLine()!.split(separator: " ").map{Int($0)!}

func bSearch(from arrNABWH: [Int]) -> Int {
    // формула для модуля (a+2d)(b+2d), d - толщина, которую ищем
    // на поле размером wh метров
    let n = arrNABWH[0] // всего модулей
    let a = arrNABWH[1]
    let b = arrNABWH[2]
    let w = arrNABWH[3]
    let h = arrNABWH[4]
    
    var l = 0
    var r = min(max(w - a, w - b, 0), max(h - a, h - b, 0))
    while r > l + 1 {
        let d = (l + r) / 2
        let moduleA = (a + 2 * d)
        let moduleB = (b + 2 * d)
        //let res = min((w / moduleA) * (h / moduleB), (w / moduleB) * (h / moduleA))
        let res2 = max((w / moduleA) * (h / moduleB), (w / moduleB) * (h / moduleA))
        //print(d, res, res2, l, r)
        if res2 < n {
            r = d
        } else {
            l = d
        }
    }
    return l
}

print(bSearch(from: arrNABWH))
```

## E. Улучшение успеваемости
https://contest.yandex.ru/contest/27844/problems/E/

```swift
var countM = 0
var sumM = 0
for i in 2...4 {
    let n = Int(readLine()!)!
    sumM += i * n
    countM += n
}

// сначала все плохо, потом хорошо - первое подходящее
func lBS(from l: inout Int, and r: inout Int, for check: (_ m: Int, _ par: [Int])-> Bool, with par: [Int]) -> Int {
    while l < r {
        let m = (l + r) / 2
        if check(m, par) {
            r = m
        } else {
            l = m + 1
        }
    }
    return l
}

func checkBall(_ m: Int, _ par: [Int]) -> Bool {
    let sum = par[0] + m * 5
    let d = par[1] + m
    var ball = sum / d
    if d - sum % d <= sum % d {
        ball += 1
    }
    return ball >= 4
}

var l = 0
var r = countM
print(lBS(from: &l, and: &r, for: checkBall(_:_:), with: [sumM, countM]))
```

## F. Очень легкая задача
https://contest.yandex.ru/contest/27844/problems/F/

```swift
let arrNXY = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNXY[0]
var x = arrNXY[1]
var y = arrNXY[2]
if x > y {
    (y, x) = (x, y)
}
if x * (n - 1) > y {
    // возможно использовать два
    if x != y {
        var l = 0
        var r = x * (n - 1)
        while r > l + 1 {
            let middle = (l + r) / 2
            if  n - 1 <= middle / x + middle / y {
                r = middle
            } else {
                l = middle
            }
        }
        print(r + x)
    } else {
        print((n - 1) / 2 * x + (n - 1) % 2 * x + x)
    }
    
} else {
    print(x * n)
}
```

## G. Площадь
https://contest.yandex.ru/contest/27844/problems/G/

```swift
let n = Int(readLine()!)!
let m = Int(readLine()!)!
let count = Int(readLine()!)!

// сначала все хорошо, потом плохо - последнее подходящее
func rBS(from l: inout Int, and r: inout Int, for check: (_ m: Int, _ par: [Int])-> Bool, with par: [Int]) -> Int {
    while l < r {
        let m = (l + r + 1) / 2 // округление вверх
        if check(m, par) {
            l = m
        } else {
            r = m - 1
        }
    }
    return l
}

func checkW(_ m: Int, _ par: [Int]) -> Bool {
    let n = par[0] - m * 2
    let m = par[1] - m * 2
    let res = n * m
    return par[2] >= par[0] * par[1] - res
}

var l = 0
var r = min(n / 2, m / 2)

print(rBS(from: &l, and: &r, for: checkW(_:_:), with: [n, m, count]))
```

## H. Провода
https://contest.yandex.ru/contest/27844/problems/H/

```swift
let arrNK = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNK[0]
let k = arrNK[1]
var arrL = [Int]()
for _ in 0..<n {
    arrL.append(Int(readLine()!)!)
}

// сначала все хорошо, потом плохо - последнее подходящее
func rBS(from l: inout Int, and r: inout Int, for check: (_ m: Int, _ par: [Int])-> Bool, with par: [Int]) -> Int {
    while l < r {
        let m = (l + r + 1) / 2 // округление вверх
        if check(m, par) {
            l = m
        } else {
            r = m - 1
        }
    }
    return l
}

var l = 0
var r = arrL.reduce(0, +) / k // длина максимально возможного отрезка

func checkW(_ m: Int, _ par: [Int]) -> Bool {
    var count = 0
    for i in par {
        count += i / m
    }
    return count >= k
}

print(rBS(from: &l, and: &r, for: checkW(_:_:), with: arrL))
```

## I. Субботник
https://contest.yandex.ru/contest/27844/problems/I/

```swift
let arrNRC = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNRC[0] // количество человек
let b = arrNRC[1] // количество бригад
let c = arrNRC[2] // человек в бригаде
var arrH = [Int]()
for _ in 0..<n {
    arrH.append(Int(readLine()!)!)
}
arrH.sort()

func lBS(from l: inout Int, and r: inout Int, for check: (_ m: Int, _ par: [Int])-> Bool, with par: [Int]) -> Int {
    while l < r {
        let m = (l + r) / 2
        if check(m, par) {
            r = m
        } else {
            l = m + 1
        }
    }
    return l
}

var l = 0
var r = arrH.last! - arrH.first!

func checkH(_ m: Int, _ par: [Int]) -> Bool {
    var pSdvigov = n - b * c
    // посмотрим сможем ли создать бригады под m - разницу в росте
    var l = 0 // указатель на первого члена бригады
    var r = c - 1 // указатель на последнего
    for _ in 0..<(n - pSdvigov) / c {
        while par[r] - par[l] > m, pSdvigov > 0 {
            pSdvigov -= 1
            l += 1
            r += 1
        }
        if par[r] - par[l] > m {
            return false
        }
        l = r + 1
        r = l + c - 1
    }
    return true
}
print(lBS(from: &l, and: &r, for: checkH(_:_:), with: arrH))
```

## J. Медиана объединения
https://contest.yandex.ru/contest/27844/problems/J/

```swift
let arrNL = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNL[0]
let ll = arrNL[1]
var arrP = [[Int]]()
for _ in 0..<n {
    arrP.append([0] + readLine()!.split(separator: " ").map{Int($0)!})
}
for i in 0..<n - 1 {
    let p1 = arrP[i]
    
    for j in i + 1..<n {
        let p2 = arrP[j]
        
        var l = 1
        var r = ll
        
        while l < r {
            let m = (l + r) / 2
            if p1[m] >= p2[ll - m] {
                r = m
            } else {
                l = m + 1
            }
        }
        print(min(p1[l], p2[ll - l + 1]))
    }
}
```

## K. Медиана объединения-2
https://contest.yandex.ru/contest/27844/problems/K/

```swift
let arrNL = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNL[0]
let ll = arrNL[1]
var arrP = [[Int]]()

for _ in 0..<n {
    let arrXDACM = readLine()!.split(separator: " ").map{Int($0)!}
    
    var arrX = [0, arrXDACM[0]]
    var arrD = [0, arrXDACM[1]]
    let a = arrXDACM[2]
    let c = arrXDACM[3]
    let m = arrXDACM[4]
    
    if ll > 1 {
        for i in 2...ll {
            let d1 = (a * arrD[i - 1] + c) % m
            arrD.append(d1)
            let x1 = arrX[i - 1] + arrD[i - 1]
            arrX.append(x1)
        }
    }
    arrP.append(arrX)
}

for i in 0..<n - 1 {
    let p1 = arrP[i]

    for j in i + 1..<n {
        let p2 = arrP[j]

        var l = 1
        var r = ll

        while l < r {
            let m = (l + r) / 2
            if p1[m] >= p2[ll - m] {
                r = m
            } else {
                l = m + 1
            }
        }
        print(min(p1[l], p2[ll - l + 1]))
    }
}
```
