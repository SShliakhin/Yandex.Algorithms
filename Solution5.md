# ДЗ 5 Префиксные суммы и два указателя
https://contest.yandex.ru/contest/27794/enter/

## A. Стильная одежда
https://contest.yandex.ru/contest/27794/problems/A/

```swift
let n = Int(readLine()!)!
var arrN = readLine()!.split(separator: " ").map{Int($0)!}
let m = Int(readLine()!)!
var arrM = readLine()!.split(separator: " ").map{Int($0)!}
var pN = 0
var pM = 0
var ans = (arrN[pN], arrM[pM])
var count = abs(arrN[pN] - arrM[pM])
while pN < n, pM < m {
    if abs(arrN[pN] - arrM[pM]) < count {
        ans = (arrN[pN], arrM[pM])
        count = abs(arrN[pN] - arrM[pM])
    }
    if arrN[pN] < arrM[pM] {
        pN += 1
    } else {
        pM += 1
    }
}
print(ans.0, ans.1)
```

## B. Сумма номеров
https://contest.yandex.ru/contest/27794/problems/B/

```swift
let arrNK = readLine()!.split(separator: " ").map{Int($0)!}
let k = arrNK[1]
let n = arrNK[0]
var arrM = [Int]()

if n == 1 {
    arrM.append(Int(readLine()!)!)
} else {
    arrM = readLine()!.split(separator: " ").map{Int($0)!}
}

var l = 0
var r = 0
var count = 0
var sum = 0

for i in 0..<n {
    sum += arrM[i]
    while sum > k {
        l += 1
        sum -= arrM[l - 1]
    }
    if sum == k {
        count += 1
        l += 1
        sum -= arrM[l - 1]
    }
}

print(count)
```

## C. Туризм
https://contest.yandex.ru/contest/27794/problems/C/

```swift
let n = Int(readLine()!)!
var arrN = [[Int]]()
for _ in 0..<n {
    arrN.append(readLine()!.split(separator: " ").map{Int($0)!})
}

let m = Int(readLine()!)!
var arrM = [[Int]]()
for _ in 0..<m {
    arrM.append(readLine()!.split(separator: " ").map{Int($0)!})
}

var ans = Array(repeating: 0, count: m)
var ansUp = Array(repeating: 0, count: n + 1)
var ansDown = Array(repeating: 0, count: n + 1)
// пройдемся один раз по всему массиву вперед и создадим два префиксных массива для трассы при проходе вперед и для трассы для прохода назад
for i in 1..<n {
    let h = arrN[i][1] - arrN[i - 1][1]
    if h > 0 {
        ansUp[i] = ansUp[i - 1] + h
        ansDown[i] = ansDown[i - 1]
    } else {
        ansDown[i] = ansDown[i - 1] + -h
        ansUp[i] = ansUp[i - 1]
    }
}

for i in 0..<m {
    let start = arrM[i][0] - 1
    let finish = arrM[i][1] - 1
    if start == finish {
    } else if start > finish {
        // вниз
        //ans[i] = -ansDown[finish + 1...start].reduce(0, +)
        ans[i] = ansDown[start] - ansDown[finish]
    } else {
        // вверх
        //ans[i] = ansUp[start + 1...finish].reduce(0, +)
        ans[i] = ansUp[finish] - ansUp[start]
    }
}

print(ans.map{"\($0)\n"}.joined())
```

## D. Город Че
https://contest.yandex.ru/contest/27794/problems/D/

```swift
let arrNR = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNR[0]
let r = arrNR[1]
let arrD = readLine()!.split(separator: " ").map{Int($0)!}
var count = 0

var pMax = 0
for pMin in 0..<n {
    while pMax < n, arrD[pMax] - arrD[pMin] <= r {
            pMax += 1
    }
    count += n - pMax
}

print(count)
```

## E. Красота превыше всего
https://contest.yandex.ru/contest/27794/problems/E/

```swift
let arrNK = readLine()!.split(separator: " ").map{Int($0)!}
let k = arrNK[1]
let n = arrNK[0]
let arrR = readLine()!.split(separator: " ").map{Int($0)!}

var minCount = n // минимальная последовательность
var ans = (0, n - 1) // найденные индексы

var r = 0 // правый указатель
var dic = [Int: Int]() // есть ли у нас такой цвет и сколько раз
for l in 0..<n {
    // надо двигать r
    while r < n, dic.keys.count < k {
        let cur = arrR[r]
        if !dic.keys.contains(cur) {
            dic[cur] = 0
        }
        dic[cur]! += 1
        r += 1
    }
    if dic.keys.count == k {
        // последовательность собралась
        if minCount > r - l {
            minCount = r - l
            ans = (l, r - 1)
        }
    }
    
    let cur = arrR[l]
    dic[cur]! -= 1
    if dic[cur] == 0 {
        dic[cur] = nil
    }
}
print(ans.0 + 1, ans.1 + 1)
```

## F. Кондиционеры
https://contest.yandex.ru/contest/27794/problems/F/

```swift
let n = Int(readLine()!)!
var arrA = [Int]() // мощности
if n == 1 {
    arrA.append(Int(readLine()!)!)
} else {
    arrA = readLine()!.split(separator: " ").map{Int($0)!}
}
let m = Int(readLine()!)!
var arrB = [[Int]]() // мощность - стоимость
for _ in 0..<m {
    arrB.append(readLine()!.split(separator: " ").map{Int($0)!})
}
// отсортируем по мощности
arrA.sort(by: <)
// отсортируем по стоимости
arrB.sort(by: { a, b in
            if a[1] == b[1] {
                return a[0] < b[0]
            } else {
                return a[1] < b[1]
            } })

var count = 0
var pA = 0
for i in 0..<m {
    while pA < n, arrB[i][0] >= arrA[pA]  {
        count += arrB[i][1]
        pA += 1
    }
}
print(count)
```

## G. Счет в гипершашках
https://contest.yandex.ru/contest/27794/problems/G/

```swift
let arrNK = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNK[0]
let k = arrNK[1]
var arrN = readLine()!.split(separator: " ").map{Int($0)!}

var dic = [Int: Int]()
for i in arrN {
    if !dic.keys.contains(i) {
        dic[i] = 0
    }
    dic[i]! += 1
}
// когда игроки набрали равные баллы
let ans1 = dic.values.filter({$0 > 2}).count

// когда все разные
var ans2 = 0
var keys = dic.keys.sorted()
var j = 0
for i in 0..<keys.count {
    while j < i, keys[j] * k < keys[i] {
        j += 1
    }
    ans2 += 6 * (i - j) * (i - j - 1) / 2
}

// когда два равные баллы, один больше или меньше
var ans3 = 0
j = 0
for i in 1..<keys.count {
    while  j < i, keys[j] * k < keys[i] {
        j += 1
    }
    if dic[keys[i]]! > 1 {
        ans3 += 3 * (i - j)
    }
}

j = keys.count - 1
for i in (0..<keys.count - 1).reversed() {
    while j > i, keys[i] * k < keys[j] {
        j -= 1
    }
    if dic[keys[i]]! > 1 {
        ans3 += 3 * (j - i)
    }
}

print(ans1 + ans2 + ans3)
```

## H. Подстрока
https://contest.yandex.ru/contest/27794/problems/H/

```swift
let arrNK = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNK[0]
let k = arrNK[1]
let str = readLine()!

var dic = [Character: Int]()
var countCh = 0
// посчитаем сколько у нас символов и сколько раз они встречаются и зафиксируем первый максимум
var r = 0
var maxCount = 0
var start = 0
var dicSub = [Character: Int]()

var l = 0
var first = false
for ch in str {
    if !dic.keys.contains(ch) {
        dic[ch] = 0
        countCh += 1
    }
    dic[ch]! += 1
    if !first {
        l += 1
        if dic[ch]! > k {
            dicSub = dic
            dicSub[ch]! -= 1
            maxCount = l - 1
            first = true
        }
    }
}
var maxV = dic.values.max()!
var curCount = maxCount

if maxV > k, maxCount != countCh * k {
    for i in l - 1..<n {
        let ch = str[str.index(str.startIndex, offsetBy: i)]
        if !dicSub.keys.contains(ch) {
            dicSub[ch] = 0
        }
        dicSub[ch]! += 1
        curCount += 1
        if dicSub[ch]! > k {
            if maxCount < i - r {
                // зафиксируем промежуточный результат
                maxCount = i - r
                start = r
                // интересный крайний случай - возможный максимум
                if maxCount == countCh * k {
                    curCount = maxCount
                    break
                }
            }
            var s = str[str.index(str.startIndex, offsetBy: r)]
            while s != ch {
                dicSub[s]! -= 1
                r += 1
                curCount -= 1
                s = str[str.index(str.startIndex, offsetBy: r)]
            }
            dicSub[s]! -= 1
            r += 1
            curCount -= 1
        }
    }
    maxCount = curCount
    start = r
}
if maxCount == 0 {
    print(n, start + 1)
} else {
    print(maxCount, start + 1)
}
```

## I. Робот
https://contest.yandex.ru/contest/27794/problems/I/

```swift
let k = Int(readLine()!)!
let str = readLine()!.map{String($0)}
var r = 0
var ans = 0
for l in 0..<(str.count - k) {
    r = max(r,l + k)
    while r < str.count, str[r] == str[r - k] {
        r += 1
    }
    ans += r - l - k
}
print(ans)
```

## J. Треугольники
https://contest.yandex.ru/contest/27794/problems/J/

```swift
let n = Int(readLine()!)!
var arrXY = [[Int]]()
for _ in 0..<n {
    arrXY.append(readLine()!.split(separator: " ").map{Int($0)!})
}

func gcd(_ a: Int, _ b: Int) -> Int {
    if b == 0 { return a }
    return gcd(b, a % b);
}

var ans = 0
for i in 0..<n {
    var dicEqual = [Int: Int]()
    var dicDescent = [[Int]: Int]()
    for j in 0..<n {
        if i == j {
            continue
        }
        // расстояние от точки i до точки j
        let a = (arrXY[j][0] - arrXY[i][0]) * (arrXY[j][0] - arrXY[i][0]) + (arrXY[j][1] - arrXY[i][1]) * (arrXY[j][1] - arrXY[i][1])
        if !dicEqual.keys.contains(a) {
            dicEqual[a] = 0
        }
        dicEqual[a]! += 1
        // уменьшенный склон
        let x = arrXY[i][0] - arrXY[j][0]
        let y = arrXY[i][1] - arrXY[j][1]
        
        let g = gcd(x, y)
        let descent = [a, x / g, y / g]
        if !dicDescent.keys.contains(descent) {
            dicDescent[descent] = 0
        }
        dicDescent[descent]! += 1
    }
    // всего возможно треугольников с вершиной в i
    for (_, value) in dicEqual {
        ans += (value * (value - 1)) / 2
    }
    // всего возможно точек на одной прямой
    for (_, value) in dicDescent {
        ans -= value * (value - 1) / 2
    }
}

print(ans)
```
