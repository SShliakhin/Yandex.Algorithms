# ДЗ A1 Сложность, тестирование, особые случаи
https://contest.yandex.ru/contest/28724

## A. Сложное уравнение
https://contest.yandex.ru/contest/28724/problems/A/

```swift
let a = Int(readLine()!)!
let b = Int(readLine()!)!
let c = Int(readLine()!)!
let d = Int(readLine()!)!
var x: Int?

if a == 0, b == 0 {
    print("INF")
} else if b % a == 0 {
    //возможно решение, при условии что
    x = -b / a
    if let x = x, c * x + d != 0 {
        print(x)
    } else {
        print("NO")
    }
} else {
    print("NO")
}
```

## B. Параллелограмм
https://contest.yandex.ru/contest/28724/problems/B/
[параллельность двух прямых](https://ru.wikihow.com/%D0%BE%D0%BF%D1%80%D0%B5%D0%B4%D0%B5%D0%BB%D0%B8%D1%82%D1%8C-%D0%BF%D0%B0%D1%80%D0%B0%D0%BB%D0%BB%D0%B5%D0%BB%D1%8C%D0%BD%D0%BE%D1%81%D1%82%D1%8C-%D0%B4%D0%B2%D1%83%D1%85-%D0%BF%D1%80%D1%8F%D0%BC%D1%8B%D1%85)

```swift
let n = Int(readLine()!)!
for _ in 0..<n {
    //считываем точки
    let arr = readLine()!.split(separator: " ").map{Int($0)!}
    var arrK = Array(repeating: 0, count: 7)
    var arrR = Array(repeating: 0, count: 7)
    
    if (arr[1] - arr[3]) != 0 {
        arrK[1] = (arr[0] - arr[2])/(arr[1] - arr[3])
    }
    if (arr[5] - arr[7]) != 0 {
        arrK[2] = (arr[4] - arr[6])/(arr[5] - arr[7])
    }
    if (arr[1] - arr[5]) != 0 {
        arrK[3] = (arr[0] - arr[4])/(arr[1] - arr[5])
    }
    if (arr[3] - arr[7]) != 0 {
        arrK[4] = (arr[2] - arr[6])/(arr[3] - arr[7])
    }
    if (arr[1] - arr[7]) != 0 {
        arrK[5] = (arr[0] - arr[6])/(arr[1] - arr[7])
    }
    if (arr[3] - arr[5]) != 0 {
        arrK[6] = (arr[2] - arr[4])/(arr[3] - arr[5])
    }
    
    arrR[1] = (arr[0] - arr[2])*(arr[0] - arr[2]) + (arr[1] - arr[3])*(arr[1] - arr[3])
    arrR[2] = (arr[4] - arr[6])*(arr[4] - arr[6]) + (arr[5] - arr[7])*(arr[5] - arr[7])
    arrR[3] = (arr[0] - arr[4])*(arr[0] - arr[4]) + (arr[1] - arr[5])*(arr[1] - arr[5])
    arrR[4] = (arr[2] - arr[6])*(arr[2] - arr[6]) + (arr[3] - arr[7])*(arr[3] - arr[7])
    arrR[5] = (arr[0] - arr[6])*(arr[0] - arr[6]) + (arr[1] - arr[7])*(arr[1] - arr[7])
    arrR[6] = (arr[2] - arr[4])*(arr[2] - arr[4]) + (arr[3] - arr[5])*(arr[3] - arr[5])

    let s1 = arrK[1] == arrK[2] && arrR[1] == arrR[2]
    let s2 = arrK[3] == arrK[4] && arrR[3] == arrR[4]
    let s3 = arrK[5] == arrK[6] && arrR[5] == arrR[6]
    
    switch (s1, s2, s3) {
    case (true, true, false), (true, false, true), (false, true, true) :
        print("YES")
    default: print("NO")
    }
}
```

## C. Проверьте правильность ситуации
https://contest.yandex.ru/contest/28724/problems/C/

```swift
var arr = [0]
for _ in 1...3 {
    arr += readLine()!.split(separator: " ").map{Int($0)!}
}
let countX = arr.filter({$0 == 1}).count
let countO = arr.filter({$0 == 2}).count
if (countX != countO && countO > countX) || countX - countO > 1 {
    print("NO")
} else if let isWin = hasWinRight(), !isWin {
    print("NO")
} else {
    print("YES")
}

func hasWinRight() -> Bool? {
    let check = [[1, 2, 3], [4, 5, 6], [7, 8, 9], [1, 4, 7], [2, 5, 8], [3, 6, 9], [1, 5, 9], [3, 5, 7]]
    var wonX: Bool?
    for ar in check {
        if arr[ar[0]] != 0, arr[ar[0]] == arr[ar[1]], arr[ar[1]] == arr[ar[2]] {
            if let wonX = wonX, (wonX && arr[ar[0]] == 2) ||
                (!wonX && arr[ar[0]] == 1){
                return false
            }
            wonX = arr[ar[0]] == 1
        }
    }
    if let wonX = wonX {
        if wonX {
            return countX > countO
        } else {
            return countX == countO
        }
    }
    return nil
}
```

## D. Футурама
https://contest.yandex.ru/contest/28724/problems/D/

```swift
let arrNM = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNM[0]
var body = Array(repeating: 0, count: n + 1) // подготовили массив тел
// заполним его
for i in 1...n {
    body[i] = i
}
let m = arrNM[1] // количество перестановок
// приведем массив к данным перестановкам
for _ in 0..<m {
    let arr = readLine()!.split(separator: " ").map{Int($0)!}
    let a = arr[0]
    let b = arr[1]
    (body[a],body[b]) = (body[b],body[a])
}

func bodySwap(a: Int, b: Int) -> Int {
    print(a, b)
    (body[a],body[b]) = (body[b],body[a])
    return body[b]
}

for i in 1..<n-1 {
    if i != body[i] { // разум не в своем теле
        var now = i // берем тело
        while body[now] != i {
            // до тех пор пока мы не захотим поменять то с чего мы начали
            now = bodySwap(a: now, b: n - 1)
            // в now вернется разум предпоследнего тела
        }
        // стандартные три обмена - текущий (уже меняли) с последним, тело последнего
        now = bodySwap(a: now, b: n) // текущий на последнее
        now = bodySwap(a: now, b: n) // текущий на свое
        bodySwap(a: body[n - 1], b: n - 1)
    }
}
if body[n - 1] == n {
    bodySwap(a: n - 1, b: n)
}
```

## E. Another Pair of Triangles
https://contest.yandex.ru/contest/28724/problems/E/

1. Треугольник существует только тогда, когда сумма длин любых его двух сторон больше третьей стороны. Иначе две стороны просто “укладываются” на третьей.
- как разбивать
2. Площадь треугольника по трем сторонам - формула Герона
https://2mb.ru/matematika/geometriya/ploshhad-treugolnika-po-trem-storonam/ 
корень из произведения полупериметра и разниц между каждой из сторон и полупериметра S2 = p(p-a)(p-b)(p-c)

```swift
let p = Int(readLine()!)!
// самая маленькая сторона большего по площади треугольника
var a = p / 3
// средняя
var b = (p - a) / 2
// и самая большая
var c = p - a - b
// проверка на возможность быть
if a + b > c {
    print(a,b,c)
    // если мы здесь то найдем треугольник с наименьшей площадью
    // если p - четное то наименьшая сторона = 2
    // иначе = 1
    a = p % 2 == 0 ? 2 : 1
    b = (p - a) / 2
    c = p - a - b
    print(a,b,c)
} else {
    print(-1)
}
```
