# ДЗ B7 Сортировка событий
https://contest.yandex.ru/contest/29396/enter/

## A. Закраска прямой
https://contest.yandex.ru/contest/29396/problems/A/

```swift
var arrSegments = [(val: Int, type: Int)]()
// запоминаем отрезки
for _ in 0..<Int(readLine()!)! {
    let arr = readLine()!.split(separator: " ").map{Int($0)!}
    arrSegments.append((val: arr.first!, type: 0))
    arrSegments.append((val: arr.last!, type: 1))
}
// отсортируем отрезки по возрастанию
arrSegments.sort(by: < )
var curBegin = arrSegments[0].val
var cntSegments = 1
var ans = 0
arrSegments[1...].forEach { val, type in
    if type == 1 {
        // очередной отрезок заканчивается
        cntSegments -= 1
        if cntSegments == 0 {
            // надо зафиксировать очередной покрашенный участок
            ans += val - curBegin
        }
    } else {
        // очередной отрезок начался
        cntSegments += 1
        if cntSegments == 1 {
            // это первый на новом участке
            curBegin = val
        }
    }
}
print(ans)
```

## B. Таможня
https://contest.yandex.ru/contest/29396/problems/B/

```swift
var arrGoods = [(val: Int, type: Int)]()
// запоминаем товары и время прибытия и окончания проверки
for _ in 0..<Int(readLine()!)! {
    let arr = readLine()!.split(separator: " ").map{Int($0)!}
    arrGoods.append((val: arr.first!, type: 1))
    arrGoods.append((val: arr.first! + arr.last!, type: 0))
}
// отсортируем товары по возрастанию времени окончания проверки/прибытия
arrGoods.sort(by: < )

var ans = 0
var curCnt = 0
arrGoods.forEach { _, type in
    if type == 1 {
        // пришел товар
        curCnt += 1
    } else {
        // ушел товар
        ans = max(ans, curCnt)
        curCnt -= 1
    }
}

print(ans)
```

## C. Минимальное покрытие
https://contest.yandex.ru/contest/29396/problems/C/

```swift
let m = Int(readLine()!)! // отрезок от 0...m
var arrSegments = [(start: Int, end: Int)]()

var arr = readLine()!.split(separator: " ").map{Int($0)!}
var (s, e) = (arr.first!, arr.last!)

while !(s == 0 && e == 0) {
    if e > 0, s < m {
        arrSegments.append((start: s, end: e))
    }
    arr = readLine()!.split(separator: " ").map{Int($0)!}
    (s, e) = (arr.first!, arr.last!)
}

arrSegments.sort(by: <)
var ans = [(start: Int, end: Int)]()

var nowBest: (start: Int, end: Int) = (0,0) // самый длинный подходящий отрезок
var nowRight = 0 // критерий подходимости
var nextRight = 0 // следующий критерий от самого длинного запомненного

for seg in arrSegments {
    if seg.start > nowRight {
        // время запомнить и поменять критерии
        ans.append(nowBest)
        nowRight = nextRight
        if nowRight >= m {
            break
        }
    }
    if seg.start <= nowRight, seg.end > nextRight {
        // более длинный отрезок
        nextRight = seg.end
        nowBest = seg
    }
}

if nowRight < m {
    // еще не покрыли
    ans.append(nowBest)
    nowRight = nextRight
}

if nowRight < m {
    print("No solution")
} else {
    print(ans.count)
    ans.forEach { (s,e) in
        print(s,e)
    }
}
```

## D. Наполненность котятами
https://contest.yandex.ru/contest/29396/problems/D/

```swift
let arrNM = readLine()!.split(separator: " ").map{Int($0)!} // n котят m отрезков
let arrN = readLine()!.split(separator: " ").map{Int($0)!}

var arrSegments = [(val: Int, type: Int, order: Int)]()
var ans = [Int: Int]()

arrN.forEach({arrSegments.append((val: $0, type: 0, order: -1))}) // заполнили котятами

for order in 0 ..< arrNM.last! {
    let seg = readLine()!.split(separator: " ").map{Int($0)!}
    arrSegments.append((val: seg.first!, type: -1, order: order)) // котят на начало
    arrSegments.append((val: seg.last!, type: 1, order: order)) // котят на конец
    ans[order] = 0
}

var cnt = 0
arrSegments.sort(by: <)
for item in arrSegments {
    switch item.type {
    case 0: cnt += 1 // увеличиваем котят
    case -1: ans[item.order] = cnt // начало отрезка, запоминаем котят до
    case 1: ans[item.order] = cnt - ans[item.order]! // конец отрезка, считаем сколько котят на отрезке
    default: break
    }
}

ans.sorted(by: { $0.key < $1.key }).forEach({ print($0.value, terminator: " ")})
```

## E. Полярные прямоугольники
https://contest.yandex.ru/contest/29396/problems/E/

```swift
import Foundation

let n = Int(readLine()!)! // количество прямоугольников
var events = [(phi: Double, type: Int)]()
// для поиска высоты
var rmin = 0.0
var rmax = 1000000.0

for i in 1...n {
    let arr = readLine()!.split(separator: " ").map{Double($0)!}
    let (r1, r2, phi1, phi2) = (arr[0], arr[1], arr[2], arr[3])
    rmin = max(rmin, r1)
    rmax = min(rmax, r2)
    events.append((phi: phi1, type: -i))
    events.append((phi: phi2, type: i))
}

events.sort(by: <)

var used = Set<Int>()
var cnt = 0

// первый проход
for event in events {
    if event.type < 0 {
        cnt += 1
        used.insert(-event.type)
    } else if used.contains(event.type) {
        cnt -= 1
    }
}

var ans = 0.0
for i in 0..<events.count {
    let event = events[i]
    if event.type < 0 {
        cnt += 1
    } else {
        cnt -= 1
    }
    if cnt == n {
        if i < events.count - 1 {
            ans += (events[i + 1].phi - events[i].phi) * (rmax * rmax - rmin * rmin) / 2
        } else {
            ans += (events[0].phi - events[events.count - 1].phi + 2 * Double.pi) * (rmax * rmax - rmin * rmin) / 2
        }
    }
}
print(ans)
```
