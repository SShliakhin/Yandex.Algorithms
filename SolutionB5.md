# ДЗ B5 Префиксные суммы и два указателя
https://contest.yandex.ru/contest/29075/enter/

## A. Префиксные суммы
https://contest.yandex.ru/contest/29075/problems/A/

```swift
import Foundation

let nq = readLine()!.split(separator: " ").map({Int($0)!})
var arr = [0] + readLine()!.split(separator: " ").map({Int($0)!})

var sum = 0
arr = arr.map { item in
    sum += item
    return sum
}

for _ in 1...nq.last! {
    let arrQ = readLine()!.split(separator: " ").map({Int($0)!})
    let start = arrQ.first!
    let end = arrQ.last!
    print(arr[end] - arr[start - 1])
}
```

## B. Максимальная сумма
https://contest.yandex.ru/contest/29075/problems/B/

```swift
import Foundation

let n = Int(readLine()!)!
var arr = readLine()!.split(separator: " ").map({Int($0)!})

var sum = arr[0]
var maxRange = sum
arr[1...].forEach { item in
    if item > item + sum {
        sum = item
    } else {
        sum += item
    }
    if sum > maxRange {
        maxRange = sum
    }
}

print(maxRange)
```

## C. Каждому по компьютеру
https://contest.yandex.ru/contest/29075/problems/C/

```swift
import Foundation

let arrNM = readLine()!.split(separator: " ").map({Int($0)!})
var arrN = readLine()!.split(separator: " ").map({Int($0)!})
let arrM = readLine()!.split(separator: " ").map({Int($0)!})

let n = arrNM.first!

var ans = [0] + Array(repeating: 0, count: n)
var ansGroupCnt = 0

var arrN2: [(comp: Int, order: Int)] = arrN.enumerated().map { (index, val) in
    (val + 1, index + 1)
}.sorted(by: {$0.comp < $1.comp})

var arrM2: [(comp: Int, order: Int)] = arrM.enumerated().map { (index, val) in
    (val, index + 1)
}.sorted(by: {$0.comp < $1.comp})

var roomN = 0
arrN2.forEach { item in
    
    while roomN < arrM2.count, item.comp > arrM2[roomN].comp {
        roomN += 1
    }
    
    if roomN != arrM2.count, item.comp <= arrM2[roomN].comp {
        ans[item.order] = arrM2[roomN].order
        ansGroupCnt += 1
        roomN += 1
    }
    
}

print(ansGroupCnt)
ans[1...].forEach({print($0)})
```

## D. Правильная, круглая, скобочная
https://contest.yandex.ru/contest/29075/problems/D/

```swift
import Foundation

let arrStr = readLine()!.map({String($0)})
var open = 0

for item in arrStr {
    if item == "(" {
        open += 1
    } else {
        if open == 0 {
            open = -1
            break
        }
        open -= 1
    }
}

if open == 0 {
    print("YES")
} else {
    print("NO")
}
```

## E. Сумма трёх
https://contest.yandex.ru/contest/29075/problems/E/

```swift
import Foundation

let target = Int(readLine()!)!

func arrAndNum(nums: [Int]) -> [(val: Int, order: Int)] {
    // избавимся от дубликатов
    var newArr = [(val: Int, order: Int)]()
    var setArr = Set<Int>()
    nums[1...].enumerated().forEach { (index, val) in
        if !setArr.contains(val) {
            setArr.insert(val)
            newArr.append((val, index))
        }
        }
    return newArr.sorted(by: {$0.val < $1.val})
}

var arrA = arrAndNum(nums: readLine()!.split(separator: " ").map({Int($0)!}))
var arrB = arrAndNum(nums: readLine()!.split(separator: " ").map({Int($0)!}))
var arrC = arrAndNum(nums: readLine()!.split(separator: " ").map({Int($0)!}))

var isOK = false
var ans: (posA: Int, posB: Int, posC: Int)?

for itemA in arrA {
    var indexC = arrC.count - 1
    for itemB in arrB {
        while indexC > 0, itemA.val + itemB.val + arrC[indexC].val > target {
            indexC -= 1
        }
        if itemA.val + itemB.val + arrC[indexC].val == target &&
            (!isOK || (ans! > (itemA.order, itemB.order, arrC[indexC].order))) {
            ans = (posA: itemA.order, posB: itemB.order, posC: arrC[indexC].order)
            isOK = true
        }
    }
}

if isOK {
    print(ans!.posA, ans!.posB, ans!.posC)
} else {
    print(-1)
}
```
