# ДЗ 7 Сортировка событий
https://contest.yandex.ru/contest/27883/enter/

## A. Наблюдение за студентами
https://contest.yandex.ru/contest/27883/problems/A/

```swift
var arrNM = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNM[0]
let m = arrNM[1]
var arrM = [(Int, Int)]()
for _ in 0..<m {
    let arrBE = readLine()!.split(separator: " ").map{Int($0)!}
    arrM.append((arrBE[0], 0)) // начало
    arrM.append((arrBE[1], 1)) // конец
}
// отсортируем массив отрезков
arrM.sort(by: { (a, b) in
            if a.0 == b.0 {
                return a.1 < b.1
            } else {
                return a.0 < b.0
            }
})

var ans = 0 // за кем следят
var countM = 0 // сколько в данный момент следят
var b = 0 // начало
var e = 0 // окончание
for i in arrM {
    if i.1 == 0 {
        // начало отрезка
        countM += 1 // увеличиваем наблюдающих
        if countM == 1 {
            b = i.0 // наблюдение только началось
        }
    } else {
        // окончание отрезка
        countM -= 1 // уменьшаем наблюдающих
        e = max(e, i.0)
        if countM == 0 {
            ans += e - b + 1
            e = 0
            b = 0
        }
    }
}
print(n - ans)
```

## B. Точки и отрезки
https://contest.yandex.ru/contest/27883/problems/B/

```swift
import Foundation

func readFile() -> [String.SubSequence] {
    guard let lines = try? String(contentsOfFile: "input.txt") else {
        return []
    }
    return lines.split(separator: "\n") //.map{String(describing: $0)}
}

func writeFile(_ result: String) {
    try? result.write(toFile: "output.txt", atomically: true, encoding: .utf8)
}

let arrString = readFile()

var arrNM = arrString[0].split(separator: " ").map{Int($0)!}
let n = arrNM[0] // количество отрезков
let m = arrNM[1] // количество точек
var arrN = Array(repeating: (0, 0), count: n * 2 + m)
for i in 0..<n {
    let arrBE = arrString[i + 1].split(separator: " ").map{Int($0)!}
    arrN[2 * i] = (min(arrBE[0], arrBE[1]), -1) // начало отрезка
    arrN[2 * i + 1] = (max(arrBE[0], arrBE[1]), 10000000000) // конец отрезка
}

let arrM = arrString[arrString.count - 1].split(separator: " ").map{Int($0)!}
for i in 0..<arrM.count {
    arrN[2 * n + i] = (arrM[i], i) // точка и ее порядок
}
// отсортируем массив отрезков - точки должны оказаться внутри отрезков
arrN.sort(by: { (a, b) in
            if a.0 == b.0 {
                return a.1 < b.1
            } else {
                return a.0 < b.0
            }
})
var ans = Array(repeating: "", count: m)
var countN = 0 // количество открытых отрезков
for i in arrN {
    switch i.1 {
    case 10000000000: countN -= 1
    case -1: countN += 1
    default: ans[i.1] = "\(countN) "
    }
}
writeFile(ans.joined())
```

## C. Рассадка в аудитории
https://contest.yandex.ru/contest/27883/problems/C/

```swift
let arrND = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrND[0] // студентов
let d = arrND[1] // расстояние разговора
let arrN = readLine()!.split(separator: " ").map{Int($0)!}

var ans = Array(repeating: "", count: 2)
var arrV = Array(repeating: "1 ", count: n)
if d == 0 || n == 1 {
    ans[0] = "1\n"
} else {
    // считываем расположение и запоминаем порядок
    var arrR = Array(repeating: (0, 0), count: n)
    for i in 0..<arrN.count {
        arrR[i] = (arrN[i], i)
    }

    // отсортируем массив рассадки по расположению
    arrR.sort(by: { (a, b) in return a.0 < b.0 })
    var curV = 1 // текущий вариант
    var maxV = 1 // максимальный вариант

    // сначала ищем макс вариант
    for i in 1..<arrR.count {
        if arrR[i].0 - arrR[i - maxV].0 <= d {
            maxV += 1
        } 
    }
    // раздадим варианты
    for i in 0..<arrR.count {
        arrV[arrR[i].1] = "\(curV) "
        curV += 1
        if curV > maxV {
            curV = 1
        }
    }
    ans[0] = "\(maxV)\n"
}
ans[1] = arrV.joined()
print(ans.joined())
```

## D. Реклама
https://contest.yandex.ru/contest/27883/problems/D/

```swift
// фишка - схлопнуть отрезки на время рекламы
// фишка - понимать по ограничениям какая сложность алгоритма зайдет, если 2000, то O(N2) вполне за 1 секунду
// по задаче - для каждого покупателя запускать первый ролик по его приходу и относительно него искать запуск второго, при чем для второго можно по событию ухода. цикл в цикле. Во втором цикле не учитывать тех кто учтен в первом.
// ограничения - ни одного, один максимальный. Ролики не должны пересекаться

import Foundation

func readFile() -> [String.SubSequence] {
    guard let lines = try? String(contentsOfFile: "input.txt") else {
        return []
    }
    return lines.split(separator: "\n") //.map{String(describing: $0)}
}

func writeFile(_ result: String) {
    try? result.write(toFile: "output.txt", atomically: true, encoding: .utf8)
}

let d = 5 // промежуток ролика
var arrT = [(time: Int, type: Int, id: Int)]() // время прихода-ухода и тип и идентификатор
let arrTBE = readFile()
let n = Int(arrTBE[0])! // число покупателей 1 ≤ N ≤ 2000
for i in 1...n {
    let arrBE = arrTBE[i].split(separator: " ").map{Int($0)!}
    let b = arrBE[0]
    let end = arrBE[1]
    if end - b >= d {
        // выбрали только тех кто сможет посмотреть ролик целиком с определенной мин
        arrT.append((b, -1, i))
        arrT.append((end - d, 1, i))
    }
}
var ans = ""
if arrT.count == 0 {
    ans = "0 1 6"
} else if arrT.count == 2 {
    ans = "1 \(arrT[0].time) \(arrT[0].time + d)"
} else {
    var setTF = Set<Int>() // фиксация включения для покупателей
    var bestCnt = 0
    var nowCnt = 0
    var bestT1 = -1
    var bestT2 = -1
    arrT.sort(by: { (a, b) in
                    if a.0 == b.0 {
                        return a.1 < b.1
                    } else {
                        return a.0 < b.0
                    }
        })
    for i in 0..<arrT.count {
        switch arrT[i].type {
        case 1: // встретили конец отрезка
            setTF.remove(arrT[i].id)
        case -1: // вcтретили начало отрезка
            setTF.insert(arrT[i].id)
            nowCnt = setTF.count
            
            // для случая с одним максимумом
            if nowCnt > bestCnt {
                bestCnt = nowCnt
                bestT1 = arrT[i].time
                bestT2 = bestT1 + d
            }
            // перебор возможных вторых максимумов
            var nowCnt2 = 0
            for j in i + 1..<arrT.count {
                switch arrT[j].type {
                case 1:
                    if !setTF.contains(arrT[j].id) {
                        // зафиксируем возможный максимум
                        if nowCnt2 + nowCnt > bestCnt, arrT[j].time >= arrT[i].time + d {
                            bestCnt = nowCnt2 + nowCnt
                            bestT1 = arrT[i].time
                            bestT2 = arrT[j].time
                        }
                        nowCnt2 -= 1
                    }
                case -1: nowCnt2 += 1
                default: break
                }
            }
        default: break
        }
    }
    ans = "\(bestCnt) \(bestT1) \(bestT2)"
}
writeFile(ans)
```

## E. Кассы
https://contest.yandex.ru/contest/27883/problems/E/

```swift
// фишка - разбить отрезки на цифирблате на два если проходит полуночь
// фишка - время превратить в минуты, чтобы расположить отрезки на одной прямой и легко было считать ответ

import Foundation

func readFile() -> [String.SubSequence] {
    guard let lines = try? String(contentsOfFile: "input.txt") else {
        return []
    }
    return lines.split(separator: "\n") //.map{String(describing: $0)}
}

func writeFile(_ result: String) {
    try? result.write(toFile: "output.txt", atomically: true, encoding: .utf8)
}

let arrT = readFile()
let n = Int(arrT[0])! // число касс
var arrTBE = [(t: Int, type: Int, id: Int)]()
for i in 1...n {
    let arr = arrT[i].split(separator: " ").map{Int($0)!}
    let b = arr[0] * 60 + arr[1]
    let e = arr[2] * 60 + arr[3]
    if b < e {
        arrTBE.append((t: b, type: -1, id: i))
        arrTBE.append((t: e, type: 1, id: i))
    } else {
        arrTBE.append((t: b, type: -1, id: i))
        arrTBE.append((t: 24 * 60, type: 1, id: i))
        arrTBE.append((t: 0, type: -1, id: i))
        arrTBE.append((t: e, type: 1, id: i))
    }
}
arrTBE.sort(by: {(a, b) in
                if a.t == b.t {
                    return a.type < b.type
                } else {
                    return a.t < b.t
                }
})
var ans = 0
var count = 0
var tBegin = 0
var tEnd = 0
for i in arrTBE {
    switch i.type {
    case -1:
        count += 1
        if n == count {
            tBegin = i.t
        }
    case 1:
        if n == count {
            ans += i.t - tBegin
        }
        count -= 1
    default: break
    }
}
writeFile(String(describing: ans))
```

## F. Современники
https://contest.yandex.ru/contest/27883/problems/F/

```swift
import Foundation

func readFile() -> [String.SubSequence] {
    guard let lines = try? String(contentsOfFile: "input.txt") else {
        return []
    }
    return lines.split(separator: "\n") //.map{String(describing: $0)}
}

func writeFile(_ result: String) {
    try? result.write(toFile: "output.txt", atomically: true, encoding: .utf8)
}

func get18_80(for arr: [Int]) -> [(Int, Int, Int)]{
    var age18 = (arr[0], arr[1], arr[2] + 18)
    var age80 = (arr[0], arr[1], arr[2] + 80)
    if arr[0] == 29, arr[1] == 2 {
        age18 = (1, 3, arr[2] + 18)
        if (arr[2] + 80) % 100 == 0, (arr[2] + 80) % 400 != 0 {
            age80 = (1, 3, arr[2] + 80)
        }
    }
    // проверка на 18
    let arrForSorted = [age18, age80, (arr[3], arr[4], arr[5])]
    return arrForSorted.sorted(by: {(a, b) in
        if a.2 == b.2, a.1 == b.1 {
            return a.0 < b.0
        } else if a.2 == b.2, a.1 != b.1 {
            return a.1 < b.1
        } else {
            return a.2 < b.2
        }
    })
}

let arrT = readFile()
let n = Int(arrT[0])! // число дат
var arrTBE = [(t: Int, type: Int, id: Int)]()
for i in 1...n {
    let arr = arrT[i].split(separator: " ").map{Int($0)!}
    let arrSorted = get18_80(for: arr)
    if arr[3] == arrSorted[0].0, arr[4] == arrSorted[0].1, arr[5] == arrSorted[0].2 {
        // не участвует
    } else {
        arrTBE.append((t: arrSorted[0].0 + arrSorted[0].1 * 100 + arrSorted[0].2 * 10000, type: -1, id: i))
        arrTBE.append((t: arrSorted[1].0 + arrSorted[1].1 * 100 + arrSorted[1].2 * 10000, type: 1, id: i))
    }
}
arrTBE.sort(by: {(a, b) in
                if a.t == b.t {
                    return a.type > b.type
                } else {
                    return a.t < b.t
                }
})
var ans = [String]()
var setS = Set<Int>()
var setAns = Set<Int>()
for i in arrTBE {
    switch i.type {
    case -1:
        setS.insert(i.id)
    case 1:
        if !setAns.contains(i.id) || setS.subtracting(setAns).count != 0 {
            ans.append("\(setS.sorted(by: <).map{String(describing: $0)}.joined(separator: " "))\n")
            for i in setS {
                setAns.insert(i)
            }
        }
        setS.remove(i.id)
    default: break
    }
}
if ans.count == 0 {
    ans.append("0")
}
writeFile(ans.joined())
```

## G. Детский праздник
https://contest.yandex.ru/contest/27883/problems/G/

```swift
import Foundation

func readFile() -> [String.SubSequence] {
    guard let lines = try? String(contentsOfFile: "input.txt") else {
        return []
    }
    return lines.split(separator: "\n") //.map{String(describing: $0)}
}

func writeFile(_ result: String) {
    try? result.write(toFile: "output.txt", atomically: true, encoding: .utf8)
}

let arrFile = readFile()
let arrMN = arrFile[0].split(separator: " ").map{Int($0)!}
let m = arrMN[0]
let n = arrMN[1]

var users = [(t: Int, v: Int, id: Int)]()
var ball = Array(repeating: 0, count: n)
var rest = Array(repeating: 0, count: n)
var count = Array(repeating: 0, count: n)
var time = 0

if m != 0 {
    var minTime = 0
    var maxTime = 0
    for i in 0..<n {
        let user = arrFile[i + 1].split(separator: " ").map{Int($0)!}
        users.append((t: 0, v: user[0], id: i))
        ball[i] = user[1]
        rest[i] = user[2]
        
        let userT = m * user[0] + (m / ball[i]  - (m % ball[i] == 0 ? 1 : 0)) * rest[i]
        if minTime == 0 {
            minTime = userT
        } else {
            minTime = min(minTime, userT)
        }
        maxTime = max(maxTime, userT)
    }
    
    if m == 1 || minTime == maxTime {
        for _ in 1...m {
            // отсортировали помощников по скорости надувания
            users.sort(by: {(a, b) in
                        if a.t == b.t {
                            return a.v < b.v
                        } else {
                            return a.t < b.t
                        }
            })
            let index = 0
            let id = users[index].id
            count[id] += 1
            users[index].t += users[index].v
            time = max(time, users[index].t)
            if count[id] % ball[id] == 0 {
                users[index].t += rest[id]
            }
        }
    } else {
        // создадим концы отрезков, для каждого помощника, как будто он один
        var arrEnd = [(t: Int, id: Int)]()
        for i in users {
            var balls = 0
            var t = 0
            let id = i.id
            let v = i.v
            while t < minTime, balls < m {
                t += v
                arrEnd.append((t: t, id: id))
                balls += 1
                if balls % ball[id] == 0 {
                    t += rest[id]
                }
            }
        }
        arrEnd.sort(by: {(a, b) in a.t < b.t})
        var balls = 0
        for i in arrEnd {
            balls += 1
            count[i.id] += 1
            if balls == m {
                time = i.t
                break
            }
        }
    }
}

var ans = [String]()
ans.append("\(time)\n")
ans.append((count.map{String(describing: $0)}.joined(separator: " ")))
writeFile(ans.joined())
```

## H. Охрана
https://contest.yandex.ru/contest/27883/problems/H/

```swift
import Foundation

let k = Int(readLine()!)!
var ans = Array(repeating: "", count: k)

for test in 0..<k {
    let arr = readLine()!.split(separator: " ").map{Int($0)!}
    let n = arr[0] // количество охранников
    var events = [(time: Int, type: Int, id: Int)]()
    for i in 0..<n {
        events.append((arr[2 * i + 1], -1, i)) // приход
        events.append((arr[2 * i + 2], 1, i)) // уход
    }
    events.sort(by: {(a, b) in
        if a.time == b.time {
            return a.type < b.type
        } else {
            return a.time < b.time
        }
    })
    
    var isOK = false
    var dicGood = [Int: Bool]()
    if events.last!.time == 10000, events.first!.time == 0 {
        isOK = true
        var nowSeq = [Int: Bool]()
        var prevTime = -1
        for event in events {
            // остался один охранник
            if nowSeq.count == 1 , prevTime != event.time {
                dicGood[nowSeq.first!.key] = true
            }
            if event.type == -1 {
                nowSeq[event.id] = true
            } else {
                nowSeq[event.id] = nil
                // это не первый отрезок и нет охранников - объект без охраны
                if nowSeq.count == 0, event.time != 10000 {
                    isOK = false
                    break
                }
            }
            prevTime = event.time
        }
    }
    if isOK, dicGood.count == n {
        ans[test] = "Accepted"
    } else {
        ans[test] = "Wrong Answer"
    }
}

print(ans.joined(separator: "\n"))
```

## I. Автобусы
https://contest.yandex.ru/contest/27883/problems/I/

```swift
// проверка на возможность - в городах не должно оставаться автобусов
// как считаем: на момент полночи автобусы в городах + автобусы в дороге - ночные автобусы
// ночные автобусы - время отправления больше времени прибытия
// при отправлении автобуса из города считаем что он там есть всегда
// время переводим в наименьшую валюту - минуты

import Foundation

func readFile() -> [String.SubSequence] {
    guard let lines = try? String(contentsOfFile: "input.txt") else {
        return []
    }
    return lines.split(separator: "\n") //.map{String(describing: $0)}
}

func writeFile(_ result: String) {
    try? result.write(toFile: "output.txt", atomically: true, encoding: .utf8)
}

let fromFile = readFile()
let arrNM = fromFile[0].split(separator: " ").map{Int($0)!}
let n = arrNM[0] // города
let m = arrNM[1] // маршруты
var buses = Array(repeating: 0, count: n + 1) // авботусы по городам + 1
var balance = Array(repeating: 0, count: n + 1) // сколько приехало, столько должно уехать за сутки
var nightBus = 0 // ночные рейсы
var events = [(town: Int, time: Int, type: Int)]()
for i in 1...m {
    let bus = fromFile[i].split(separator: " ")
    let cDep = Int(bus[0])!
    let cArr = Int(bus[2])!
    // время отправления и прибытия, подсчет ночных автобусов
    let d = bus[1].split(separator: ":").map{Int($0)!}
    let tDep = d[0] * 60 + d[1]
    let a = bus[3].split(separator: ":").map{Int($0)!}
    let tArr = a[0] * 60 + a[1]
    if tArr < tDep {
        nightBus += 1
    }
    // баланс
    balance[cDep] -= 1 // отправление
    balance[cArr] += 1 // прибытие
    // события
    events.append((town: cDep, time: tDep, type: -1))
    events.append((town: cArr, time: tArr, type: 1))
}
let disbalance = balance.filter({$0 != 0 }).count > 0
var ans = 0
if disbalance {
    ans = -1
} else {
    events.sort(by: {(a, b) in
        if a.time == b.time {
            return a.type > b.type
        } else {
            return a.time < b.time
        }
    })
    for event in events {
        buses[event.town] += event.type
        if buses[event.town] < 0 {
            buses[event.town] = 0
        }
    }
    ans = buses.reduce(0, +) + nightBus
}

writeFile(String(describing: ans))
```

## J. НГУ-стройка
https://contest.yandex.ru/contest/27883/problems/J/

```swift
import Foundation

func readFile() -> [String.SubSequence] {
    guard let lines = try? String(contentsOfFile: "input.txt") else {
        return []
    }
    return lines.split(separator: "\n") //.map{String(describing: $0)}
}

func writeFile(_ result: String) {
    try? result.write(toFile: "output.txt", atomically: true, encoding: .utf8)
}

let fromFile = readFile()
let arrNWL = fromFile[0].split(separator: " ").map{Int($0)!}
let n = arrNWL[0] // количество блоков
let area = arrNWL[1] * arrNWL[2] // площадь которую надо покрыть
// события по оси Z, начало -1, окончание 1, покрывают area, номер i
var events = [(z: Int, type: Int, area: Int, id: Int)]()
for i in 1...n {
    let line = fromFile[i].split(separator: " ").map{Int($0)!}
    let s = abs(line[0] - line[3]) * abs(line[1] - line[4]) // площадь блока
    events.append((z: line[2], type: 1, area: s, id: i))
    events.append((z: line[5], type: -1, area: s, id: i))
}
events.sort(by: {(a, b) in
                if a.z == b.z {
                    return a.type < b.type
                } else {
                    return a.z < b.z
                }
})
// первый проход определяем минимальное количество блоков для покрытия area
var minUsed = n + 1
var nowArea = 0
var nowUsed = 0
for event in events {
    switch event.type {
    case 1:
        nowUsed += 1
        nowArea += event.area
        if area == nowArea, nowUsed < minUsed {
            minUsed = nowUsed
        }
    default:
        nowUsed -= 1
        nowArea -= event.area
    }
}

// второй - моделируем мин состав фиксируя номерба блоков и при воссоздание выходим
var ans = [String]()
if minUsed == n + 1 {
    ans.append("NO")
} else {
    ans.append("YES")
    ans.append("\(minUsed)")
    var usedBloks = Set<Int>()
    bb: for event in events {
        switch event.type {
        case 1:
            nowUsed += 1
            nowArea += event.area
            usedBloks.insert(event.id)
            if area == nowArea, nowUsed == minUsed {
                ans.append(usedBloks.map{String(describing: $0)}.joined(separator: " "))
                break bb
            }
        default:
            nowUsed -= 1
            nowArea -= event.area
            usedBloks.remove(event.id)
        }
    }
}

writeFile(ans.joined(separator: "\n"))
```
