# ДЗ 4 Словари и сортировка подсчётом
https://contest.yandex.ru/contest/27665/enter/

## A. Словарь синонимов
https://contest.yandex.ru/contest/27665/problems/A/

```swift
let n = Int(readLine()!)!
var dic = [String: String]()
for _ in 0..<n {
    let str = readLine()!.split(separator: " ")
    dic[String(str[0])] = String(str[1])
    dic[String(str[1])] = String(str[0])
}
print(dic[String(readLine()!)]!)
```

## B. Номер появления слова
https://contest.yandex.ru/contest/27665/problems/B/

```swift
var dir = [String: Int]()
var ans = [Int]()
while let str = readLine(), !str.isEmpty {
    str.split(separator: " ").map{String($0)}.forEach { item in
        if dir[item] == nil {
            dir[item] = 0
            ans.append(0)
        } else {
            ans.append(dir[item]!)
        }
        dir[item]! += 1
    }
}
print(ans.map{String($0)}.joined(separator: " "))
```

## C. Самое частое слово
https://contest.yandex.ru/contest/27665/problems/C/

```swift
var dir = [String: Int]()
var ans = [String]()
var cntMax = 0
while let str = readLine(), !str.isEmpty {
    str.split(separator: " ").map{String($0)}.forEach { item in
        if dir[item] == nil {
            dir[item] = 0
        }
        dir[item]! += 1
        if dir[item]! > cntMax {
            ans = []
            ans.append(item)
            cntMax += 1
        } else if dir[item]! == cntMax {
            ans.append(item)
        }
    }
}
print(ans.sorted(by: <)[0])
```

## D. Клавиатура
https://contest.yandex.ru/contest/27665/problems/D/

```swift
let count = Int(readLine()!)! // всего клавиш
let beforeBroken = readLine()!.split(separator: " ").map{Int($0)!} // кол-во для каждой клавиши
let totalPress = Int(readLine()!)!
let howPress = readLine()!.split(separator: " ").map{Int($0)!}

let dic = howPress.reduce(into: [:]) { $0[$1, default: 0] += 1 } // клавиша : количество нажатий

for i in 0..<count {
    print(dic[i + 1]! <= beforeBroken[i] ? "NO" : "YES")
}
```

## E. Пирамида
https://contest.yandex.ru/contest/27665/problems/E/

```swift
let n = Int(readLine()!)!
var dic = [Int: Int]()
for _ in 0..<n {
    let arr = readLine()!.split(separator: " ").map{Int($0)!}
    if !dic.keys.contains(arr[0]) {
        dic[arr[0]] = 0
    }
    dic[arr[0]]! = max(dic[arr[0]]!, arr[1])
}
var h = 0
for (_, value) in dic.sorted(by: {a,b in return a.key > b.key}) {
    h += value
}
print(h)
```

## F. Продажи
https://contest.yandex.ru/contest/27665/problems/F/

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
var lines = [String]()
var shopersGoods = [String: [String : Int]]()
var shopers = [String]()
for i in 0..<fromFile.count {
    let line = fromFile[i].split(separator: " ")
    let shoper = String(line[0])
    let lineK = String(line[1])
    let lineV = Int(line[2])!
    if !shopersGoods.keys.contains(shoper) {
        shopersGoods[shoper] = [String : Int]()
        shopers.append(shoper)
    }
    if !shopersGoods[shoper]!.keys.contains(lineK) {
        shopersGoods[shoper]![lineK] = 0
    }
    shopersGoods[shoper]![lineK]! += lineV
}
var ans = ""
for value in shopers.sorted(by: <) {
    ans += "\(value):\n"
    for (key, value) in shopersGoods[value]!.sorted(by: {$0.key < $1.key}) {
        ans += "\(key) \(value)\n"
    }
}
print(ans)
```

## G. Банковские счета
https://contest.yandex.ru/contest/27665/problems/G/

- DEPOSIT name sum - зачислить сумму sum на счет клиента name.
- WITHDRAW name sum - снять сумму sum со счета клиента name.
- BALANCE name - узнать остаток средств на счету клиента name.
- TRANSFER name1 name2 sum - перевести сумму sum со счета клиента name1 на счет клиента name2.
- INCOME p - начислить всем клиентам, у которых открыты счета, p% от суммы счета.

// Важно как начислять проценты, работая с Int - accounts[key]! += accounts[key]! * p / 100

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
var ans = [String]()
var accounts = [String: Int]()
func checkAccount(for name: String) {
    if !accounts.keys.contains(name) {
        accounts[name] = 0
    }
}
for i in 0..<fromFile.count {
    let line = fromFile[i].split(separator: " ")
    let com = String(line[0])
    switch com {
    case "DEPOSIT":
        //DEPOSIT name sum - зачислить сумму sum на счет клиента name.
        let name = String(line[1])
        let sum = Int(line[2])!
        checkAccount(for: name)
        accounts[name]! += sum
    case "WITHDRAW":
        //WITHDRAW name sum - снять сумму sum со счета клиента name.
        let name = String(line[1])
        let sum = Int(line[2])!
        checkAccount(for: name)
        accounts[name]! -= sum
    case "TRANSFER":
        //TRANSFER name1 name2 sum - перевести сумму sum со счета клиента name1 на счет клиента name2.
        let name = String(line[1])
        let name2 = String(line[2])
        let sum = Int(line[3])!
        checkAccount(for: name)
        accounts[name]! -= sum
        checkAccount(for: name2)
        accounts[name2]! += sum
    case "INCOME":
        //INCOME p - начислить всем клиентам, у которых открыты счета, p% от суммы счета.
        let p = Int(line[1])!
        for (key, value) in accounts {
            if value > 0 {
                accounts[key]! += accounts[key]! * p / 100
            }
        }
    default:
        //BALANCE name - узнать остаток средств на счету клиента name.
        let name = String(line[1])
        if accounts.keys.contains(name) {
            ans.append("\(accounts[name]!)")
        } else {
            ans.append("ERROR")
        }
    }
}

writeFile(ans.joined(separator: "\n"))
```

## H. Расшифровка письменности Майя
https://contest.yandex.ru/contest/27665/problems/H/

```swift
let arrWS = readLine()!.split(separator: " ").map{Int($0)!}
let ww = arrWS[0]
let ss = arrWS[1]

func createDic(_ str: [Character]) -> [String: Int] {
    var dic = [String: Int]()
    for i in 0..<ww {
        let ch = String(str[i])
        if !dic.keys.contains(ch) {
            dic[ch] = 0
        }
        dic[ch]! += 1
    }
    return dic
}

let dicW = createDic(Array(readLine()!))
var arrS = Array(readLine()!)
var dicCur = createDic(Array(arrS[0...ww - 1]))
var count = 0
var lastOK = false // последнее сравнение удачное
if dicW == dicCur {
    count += 1
    lastOK = true
}

for i in 1..<ss - ww + 1 {
    // по идее мы убираем первый символ и добавляем последний
    let firstSym = String(arrS[i - 1])
    let lastSym = String(arrS[i + ww - 1])
    
    if firstSym != lastSym {
        if dicCur[firstSym]! == 1 {
            dicCur[firstSym] = nil
        } else {
            dicCur[firstSym]! -= 1
        }
        // добавим новый
        if !dicCur.keys.contains(lastSym) {
            dicCur[lastSym] = 0
        }
        dicCur[lastSym]! += 1
    } else {
        if lastOK {
            count += 1
            continue
        }
    }
    
    if dicW == dicCur {
        count += 1
        lastOK = true
    } else {
        lastOK = false
    }
}
print(count)
```

## I. Контрольная по ударениям
https://contest.yandex.ru/contest/27665/problems/I/

```swift
let n = Int(readLine()!)!
var dic = Set<String>()
var dicL = Set<String>()
for _ in 0..<n {
    let str = readLine()!
    dic.insert(str)
    dicL.insert(str.lowercased())
}
let work = readLine()!.split(separator: " ")
var count = 0
for word in work {
    if dicL.contains(String(word).lowercased()) {
        // слово есть в словаре
        if !dic.contains(String(word)) {
            // но написано не верно
            count += 1
        }
    } else {
        // слова нет в словаре
        let str = Array(String(word))
        if str.filter({("A"..."Z").contains($0)}).count != 1 {
            // нет ударения или больше одного ударения
            count += 1
        }
    }
}
print(count)
```

## J. Дополнительная проверка на списывание
https://contest.yandex.ru/contest/27665/problems/J/

Важно:
1. Писать пояснения
2. Делать заглушки
3. Не забывать про идею, помнить про детали

- строка через свои индексы
- функции которые оценивают символ if ch.isLetter || ch.isNumber || ch == "_" {
- констуктор строки через массив и джоин
- роверка строки на число
```swift
if let _ = Int(str) {
    return false
}
```

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

// для каждой строки замени все символы кроме букв, цифр и подчеркиваний символом пробел
func lineConstructor(_ str: String) -> String {
    var ans = [String]()
    for ch in str {
        if ch.isLetter || ch.isNumber || ch == "_" {
            ans.append(String(ch))
        } else {
            ans.append(" ")
        }
    }
    return ans.joined()
}
// не число, не начинается с цифры если нельзя
func isCorrect(_ str: String) -> Bool {
    if let _ = Int(str) {
        return false
    }
    if !str[str.startIndex].isNumber || isStartWithDigit {
        return true
    }
    return false
}

let fromFile = readFile()
// создай два буля: чувствительность к регистру и старт с цифры
var firstStr = fromFile[0].split(separator: " ")
let countKyewords = Int(firstStr[0])!
let isCaseSensetive = String(firstStr[1]) == "yes"
let isStartWithDigit = String(firstStr[2]) == "yes"
// создай множество ключевых слов
var keywords = Set<String>()
for i in 0..<countKyewords {
    var keyword = String(fromFile[i + 1])
    if !isCaseSensetive {
        keyword = keyword.lowercased()
    }
    keywords.insert(keyword)
}
// создай словарь: идентификатор - количество и первое вхождение в текст
var wordsCntAndFirstPos = [String: [Int]]()
// создай счетчик слов
var cntWords = 0
// пробегись по всем строчкам текста
for i in countKyewords + 1..<fromFile.count {
    // для каждой строки замени все символы кроме букв, цифр и подчеркиваний символом пробел
    let line = lineConstructor(String(fromFile[i]))
    // нарежь строку на слова
    let words = line.split(separator: " ")
    // для каждого слова проверь не ключевое ли оно с учетом регистра если надо
    for w in words {
        var word = String(w)
        if !isCaseSensetive {
            word = word.lowercased()
        }
        if keywords.contains(word) {
            continue
        }
        // если не ключевое, проверить что корректное
        if isCorrect(word) {
            // увеличить счетчик слов
            cntWords += 1
            // зафиксировать появление нового слова и его первую позицию
            if !wordsCntAndFirstPos.keys.contains(word) {
                wordsCntAndFirstPos[word] = [0, cntWords]
            }
            // увеличить счетчик вхождения слова
            wordsCntAndFirstPos[word]![0] += 1
        }
    }
}

// пройтись по словарю счетчиков и выбери лучшее слово
var bestWord = ""
var paramets = [0, 0] // количество и первое вхождение
for (w, par) in wordsCntAndFirstPos {
    if par[0] > paramets[0] || (par[0] == paramets[0] && par[1] < paramets[1]) {
        bestWord = w
        paramets = par
    }
}
print(bestWord)
```
