# ДЗ A2 Линейный поиск
https://contest.yandex.ru/contest/28736/enter/

## A. Забавный конфуз
https://contest.yandex.ru/contest/28736/problems/A/

при применении конфуза суть минимального и максимального не меняется, происходит обмен, так как очередной член массива равен сумме остальных членов

```swift
let arrNK = readLine()!.split(separator: " ").map{Int($0)!}
let n = arrNK[0] //количество элементов массива B
let k = arrNK[1] //количество применения операции конфуз
let arrB = readLine()!.split(separator: " ").map{Int($0)!}

print(arrB.max()! - arrB.min()!)
```

## B. Изобретательный Петя
https://contest.yandex.ru/contest/28736/problems/B/

```swift
import Foundation

var arrTele = Array(readLine()!).map{String($0)}
var arrTele2 = arrTele
var arrOut = Array(readLine()!).map{String($0)}
var count = 0
let repeatT = arrOut.count / arrTele.count + 1
for _ in 1..<repeatT {
    arrTele += arrTele2
}

for ch in arrOut.reversed() {
    if ch != arrTele[arrTele.count - 1] {
        count += 1
    } else {
        let countF = arrTele.count + count - arrOut.count
        if arrTele.dropFirst(countF) ==
            arrOut.dropLast(count) {
            break
        } else {
            count += 1
        }
    }
}

print(arrOut.dropFirst(arrOut.count - count).joined())
```

## C. Шахматная доска
https://contest.yandex.ru/contest/28736/problems/C/

```swift
let n = Int(readLine()!)!
var chess = [[Int]]()
var nearby = Array(repeating: 0, count: n)
for _ in 0..<n {
    let arr = readLine()!.split(separator: " ").map{Int($0)!}
    chess.append(arr)
}
for (index,val) in chess.enumerated() {
    nearby[index] = getNearby(arr: val)
}

func getNearby(arr: [Int]) -> Int {
    let delta = [[-1, 0], [0, -1], [0, 1], [1, 0]]
    var res = 0
    for pos in delta {
        if chess.contains([arr[0] + pos[0], arr[1] + pos[1]]) {
            res += 1
        }
    }
    return res
}

print(nearby.map{4 - $0}.reduce(0, +))
```

## D. Петя, Маша и верёвочки
https://contest.yandex.ru/contest/28736/problems/D/

```swift
import Foundation

func readFile() -> [String] {
    guard let lines = try? String(contentsOfFile: "input.txt") else {
        return []
    }
    return lines.split(separator: "\n").map{String(describing: $0)}
}

let fromFile = readFile()
_ = Int(fromFile[0])!
let arrL = fromFile[1].split(separator: " ").map{Int($0)!}

let maxL = arrL.max()!
let sumL = arrL.reduce(0, +)
if sumL - maxL == maxL {
    print(maxL * 2)
} else {
    let ans = maxL * 2 - sumL
    print(ans > 0 ? ans : sumL)
}
```

## E. Газон
https://contest.yandex.ru/contest/28736/problems/E/

- floor(Double(x3) + xdelta) - округление вниз
- ceil(Double(x3) - xdelta) - округление вверх - для удаления дробной части и округления в большую сторону

```swift
import Foundation

var arr  = readLine()!.split(separator: " ").map({Int($0)!})
var (x1, y1, x2, y2) = (arr[0], arr[1], arr[2], arr[3])
arr  = readLine()!.split(separator: " ").map({Int($0)!})
let (x3, y3, r) = (arr[0], arr[1], arr[2])
(x1, x2) = (min(x1, x2), max(x1, x2))
(y1, y2) = (min(y1, y2), max(y1, y2))
var ans = 0
for y in max(y1, y3 - r)...min(y2, y3 + r) {
    let xdelta = sqrt(Double(r * r) - Double((y - y3) * (y - y3)))
    let xmax = min(x2, Int(floor(Double(x3) + xdelta)))
    let xmin = max(x1, Int(ceil(Double(x3) - xdelta)))
    if xmax >= xmin {
        ans += xmax - xmin + 1
    }
}
print(ans)
```
