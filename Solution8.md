# ДЗ 8 Деревья
https://contest.yandex.ru/contest/28069/enter/

## A. Высота дерева
https://contest.yandex.ru/contest/28069/problems/A/

```swift
func initMemory(_ maxN: Int) -> ([(key: Int, left: Int, right: Int)], Int) {
    var memory = [(key: Int, left: Int, right: Int)]()
    for i in 0..<maxN {
        memory.append((key: 0, left: i + 1, right: 0))
    }
    return (memory, 0)
}

func newNode(_ memory: [(key: Int, left: Int, right: Int)], _ firstFree: inout Int) -> Int {
    // выделение памяти
    let index = firstFree
    firstFree = memory[index].left
    return index
}

func delNode(_ memory: inout [(key: Int, left: Int, right: Int)], _ firstFree: inout Int, _ index: Int) {
    // для удаления по индексу, мы заменяем ссылку на следующий текущим первым свободным, и выдаем индекс как первый свободный
    memory[index].left = firstFree
    firstFree = index
}

// Бинарное дерево поиска
// Поиск
func find(from memory: [(key: Int, left: Int, right: Int)], start root: Int, for x: Int) -> Int {
    let key = memory[root].key
    if key == x {
        return root
    } else if key > x {
        // идем влево
        let left = memory[root].left
        if left == -1 {
            return -1
        } else {
            return find(from: memory, start: left, for: x)
        }
    } else {
        let right = memory[root].right
        if right == -1 {
            return -1
        } else {
            return find(from: memory, start: right, for: x)
        }
    }
}
// Добавление элемента и пример заполнения дерева
func createAndFillNode(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, with key: Int) -> Int {
    let index = newNode(memory, &firstFree)
    memory[index].key = key
    memory[index].left = -1
    memory[index].right = -1
    return index
}

func add(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, start root: Int, with x: Int) {
    let key = memory[root].key
    if key > x {
        //влево
        let left = memory[root].left
        if left == -1 {
            memory[root].left = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: left, with: x)
        }
    } else if key < x {
        //вправо
        let right = memory[root].right
        if right == -1 {
            memory[root].right = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: right, with: x)
        }
    }
}

func height(for memory: [(key: Int, left: Int, right: Int)], by root: Int) -> Int {
    if root == -1 {
        return 0
    } else {
        return 1 + max(height(for: memory, by: memory[root].left), height(for: memory, by: memory[root].right))
    }
}

let arrIn = readLine()!.split(separator: " ").map{Int($0)!}
// выделение памяти
var (memory, firstFree) = initMemory(arrIn.count)
// заполнение корня
let root = createAndFillNode(for: &memory, by: &firstFree, with: arrIn[0])
var ans = 0
// формирование дерева если возможно и вычисление высоты
if memory[root].key != 0 {
    for i in arrIn.dropFirst() {
        if i == 0 {
            break
        }
        add(for: &memory, by: &firstFree, start: root, with: i)
    }
    ans = height(for: memory, by: root)
}

print(ans)
```

## B. Глубина добавляемых элементов
https://contest.yandex.ru/contest/28069/problems/B/

```swift
func initMemory(_ maxN: Int) -> ([(key: Int, left: Int, right: Int)], Int) {
    var memory = [(key: Int, left: Int, right: Int)]()
    for i in 0..<maxN {
        memory.append((key: 0, left: i + 1, right: 0))
    }
    return (memory, 0)
}

func newNode(_ memory: [(key: Int, left: Int, right: Int)], _ firstFree: inout Int) -> Int {
    // выделение памяти
    let index = firstFree
    firstFree = memory[index].left
    return index
}

func delNode(_ memory: inout [(key: Int, left: Int, right: Int)], _ firstFree: inout Int, _ index: Int) {
    // для удаления по индексу, мы заменяем ссылку на следующий текущим первым свободным, и выдаем индекс как первый свободный
    memory[index].left = firstFree
    firstFree = index
}

// Бинарное дерево поиска
// Поиск
func find(from memory: [(key: Int, left: Int, right: Int)], start root: Int, for x: Int) -> Int {
    let key = memory[root].key
    if key == x {
        return root
    } else if key > x {
        // идем влево
        let left = memory[root].left
        if left == -1 {
            return -1
        } else {
            return find(from: memory, start: left, for: x)
        }
    } else {
        let right = memory[root].right
        if right == -1 {
            return -1
        } else {
            return find(from: memory, start: right, for: x)
        }
    }
}
// Добавление элемента и пример заполнения дерева
func createAndFillNode(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, with key: Int) -> Int {
    let index = newNode(memory, &firstFree)
    memory[index].key = key
    memory[index].left = -1
    memory[index].right = -1
    return index
}

func add(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, start root: Int, with x: Int) {
    let key = memory[root].key
    if key > x {
        //влево
        let left = memory[root].left
        if left == -1 {
            memory[root].left = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: left, with: x)
        }
    } else if key < x {
        //вправо
        let right = memory[root].right
        if right == -1 {
            memory[root].right = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: right, with: x)
        }
    }
}

func findDepth(from memory: [(key: Int, left: Int, right: Int)], start root: Int, for x: Int) -> Int {
    let key = memory[root].key
    if key == x {
        return 0
    } else if key > x {
        // идем влево
        let left = memory[root].left
        if left == -1 {
            return -1
        } else {
            return 1 + findDepth(from: memory, start: left, for: x)
        }
    } else {
        let right = memory[root].right
        if right == -1 {
            return -1
        } else {
            return 1 + findDepth(from: memory, start: right, for: x)
        }
    }
}

let arrIn = readLine()!.split(separator: " ").map{Int($0)!}
// выделение памяти
var (memory, firstFree) = initMemory(arrIn.count)
// заполнение корня
let root = createAndFillNode(for: &memory, by: &firstFree, with: arrIn[0])
var ans = ""
// формирование дерева если возможно и вычисление глубины
if memory[root].key != 0 {
    ans = "1"
    for i in arrIn.dropFirst() {
        if i == 0 {
            break
        }
        if find(from: memory, start: root, for: i) == -1 {
            add(for: &memory, by: &firstFree, start: root, with: i)
            ans += " \(findDepth(from: memory, start: root, for: i) + 1)"
        }
    }
}

print(ans)
```

## C. Второй максимум
https://contest.yandex.ru/contest/28069/problems/C/

```swift
func initMemory(_ maxN: Int) -> ([(key: Int, left: Int, right: Int)], Int) {
    var memory = [(key: Int, left: Int, right: Int)]()
    for i in 0..<maxN {
        memory.append((key: 0, left: i + 1, right: 0))
    }
    return (memory, 0)
}

func newNode(_ memory: [(key: Int, left: Int, right: Int)], _ firstFree: inout Int) -> Int {
    // выделение памяти
    let index = firstFree
    firstFree = memory[index].left
    return index
}

func delNode(_ memory: inout [(key: Int, left: Int, right: Int)], _ firstFree: inout Int, _ index: Int) {
    // для удаления по индексу, мы заменяем ссылку на следующий текущим первым свободным, и выдаем индекс как первый свободный
    memory[index].left = firstFree
    firstFree = index
}

// Бинарное дерево поиска
// Поиск
func find(from memory: [(key: Int, left: Int, right: Int)], start root: Int, for x: Int) -> Int {
    let key = memory[root].key
    if key == x {
        return root
    } else if key > x {
        // идем влево
        let left = memory[root].left
        if left == -1 {
            return -1
        } else {
            return find(from: memory, start: left, for: x)
        }
    } else {
        let right = memory[root].right
        if right == -1 {
            return -1
        } else {
            return find(from: memory, start: right, for: x)
        }
    }
}
// Добавление элемента и пример заполнения дерева
func createAndFillNode(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, with key: Int) -> Int {
    let index = newNode(memory, &firstFree)
    memory[index].key = key
    memory[index].left = -1
    memory[index].right = -1
    return index
}

func add(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, start root: Int, with x: Int) {
    let key = memory[root].key
    if key > x {
        //влево
        let left = memory[root].left
        if left == -1 {
            memory[root].left = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: left, with: x)
        }
    } else if key < x {
        //вправо
        let right = memory[root].right
        if right == -1 {
            memory[root].right = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: right, with: x)
        }
    }
}

func preMax(from memory: [(key: Int, left: Int, right: Int)], start root: Int) {
    func preMax2(from memory: [(key: Int, left: Int, right: Int)], start root: Int) {
        if memory[root].right == -1 {
            print(memory[root].key)
        } else {
            preMax2(from: memory, start: memory[root].right)
        }
    }
    if memory[root].right != -1 {
        // есть правый сын
        if memory[memory[root].right].right != -1 {
            // есть правый внук - ищем дальше
            preMax(from: memory, start: memory[root].right)
        } else  {
            // нет внука смотрим налево
            if memory[memory[root].right].left == -1 {
                // нет лева - печатаем себя
                print(memory[root].key)
            } else {
                // ищем правый слева
                preMax2(from: memory, start: memory[memory[root].right].left)
            }
        }
    } else {
        //ищем правый слева
        if memory[root].left == -1 {
            print(memory[root].key)
        } else {
            preMax2(from: memory, start: memory[root].left)
        }
    }
}

let arrIn = readLine()!.split(separator: " ").map{Int($0)!}
// выделение памяти
var (memory, firstFree) = initMemory(arrIn.count)
// заполнение корня
let root = createAndFillNode(for: &memory, by: &firstFree, with: arrIn[0])
// формирование дерева если возможно
if memory[root].key != 0 {
    for i in arrIn.dropFirst() {
        if i == 0 {
            break
        }
        add(for: &memory, by: &firstFree, start: root, with: i)
    }
}
preMax(from: memory, start: root)
```

## D. Обход
https://contest.yandex.ru/contest/28069/problems/D/

```swift
func initMemory(_ maxN: Int) -> ([(key: Int, left: Int, right: Int)], Int) {
    var memory = [(key: Int, left: Int, right: Int)]()
    for i in 0..<maxN {
        memory.append((key: 0, left: i + 1, right: 0))
    }
    return (memory, 0)
}

func newNode(_ memory: [(key: Int, left: Int, right: Int)], _ firstFree: inout Int) -> Int {
    // выделение памяти
    let index = firstFree
    firstFree = memory[index].left
    return index
}

func delNode(_ memory: inout [(key: Int, left: Int, right: Int)], _ firstFree: inout Int, _ index: Int) {
    // для удаления по индексу, мы заменяем ссылку на следующий текущим первым свободным, и выдаем индекс как первый свободный
    memory[index].left = firstFree
    firstFree = index
}

// Бинарное дерево поиска
// Поиск
func find(from memory: [(key: Int, left: Int, right: Int)], start root: Int, for x: Int) -> Int {
    let key = memory[root].key
    if key == x {
        return root
    } else if key > x {
        // идем влево
        let left = memory[root].left
        if left == -1 {
            return -1
        } else {
            return find(from: memory, start: left, for: x)
        }
    } else {
        let right = memory[root].right
        if right == -1 {
            return -1
        } else {
            return find(from: memory, start: right, for: x)
        }
    }
}
// Добавление элемента и пример заполнения дерева
func createAndFillNode(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, with key: Int) -> Int {
    let index = newNode(memory, &firstFree)
    memory[index].key = key
    memory[index].left = -1
    memory[index].right = -1
    return index
}

func add(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, start root: Int, with x: Int) {
    let key = memory[root].key
    if key > x {
        //влево
        let left = memory[root].left
        if left == -1 {
            memory[root].left = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: left, with: x)
        }
    } else if key < x {
        //вправо
        let right = memory[root].right
        if right == -1 {
            memory[root].right = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: right, with: x)
        }
    }
}

func traverseInOrder(from memory: [(key: Int, left: Int, right: Int)], start root: Int) {
    if memory[root].left != -1 {
        traverseInOrder(from: memory, start: memory[root].left)
    }
    print(memory[root].key)
    if memory[root].right != -1 {
        traverseInOrder(from: memory, start: memory[root].right)
    }
}

let arrIn = readLine()!.split(separator: " ").map{Int($0)!}
// выделение памяти
var (memory, firstFree) = initMemory(arrIn.count)
// заполнение корня
let root = createAndFillNode(for: &memory, by: &firstFree, with: arrIn[0])
// формирование дерева если возможно
if memory[root].key != 0 {
    for i in arrIn.dropFirst() {
        if i == 0 {
            break
        }
        add(for: &memory, by: &firstFree, start: root, with: i)
    }
}
traverseInOrder(from: memory, start: root)
```

## E. Вывод листьев
https://contest.yandex.ru/contest/28069/problems/E/

```swift
func initMemory(_ maxN: Int) -> ([(key: Int, left: Int, right: Int)], Int) {
    var memory = [(key: Int, left: Int, right: Int)]()
    for i in 0..<maxN {
        memory.append((key: 0, left: i + 1, right: 0))
    }
    return (memory, 0)
}

func newNode(_ memory: [(key: Int, left: Int, right: Int)], _ firstFree: inout Int) -> Int {
    // выделение памяти
    let index = firstFree
    firstFree = memory[index].left
    return index
}

func delNode(_ memory: inout [(key: Int, left: Int, right: Int)], _ firstFree: inout Int, _ index: Int) {
    // для удаления по индексу, мы заменяем ссылку на следующий текущим первым свободным, и выдаем индекс как первый свободный
    memory[index].left = firstFree
    firstFree = index
}

// Бинарное дерево поиска
// Поиск
func find(from memory: [(key: Int, left: Int, right: Int)], start root: Int, for x: Int) -> Int {
    let key = memory[root].key
    if key == x {
        return root
    } else if key > x {
        // идем влево
        let left = memory[root].left
        if left == -1 {
            return -1
        } else {
            return find(from: memory, start: left, for: x)
        }
    } else {
        let right = memory[root].right
        if right == -1 {
            return -1
        } else {
            return find(from: memory, start: right, for: x)
        }
    }
}
// Добавление элемента и пример заполнения дерева
func createAndFillNode(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, with key: Int) -> Int {
    let index = newNode(memory, &firstFree)
    memory[index].key = key
    memory[index].left = -1
    memory[index].right = -1
    return index
}

func add(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, start root: Int, with x: Int) {
    let key = memory[root].key
    if key > x {
        //влево
        let left = memory[root].left
        if left == -1 {
            memory[root].left = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: left, with: x)
        }
    } else if key < x {
        //вправо
        let right = memory[root].right
        if right == -1 {
            memory[root].right = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: right, with: x)
        }
    }
}

func traverseInOrder(from memory: [(key: Int, left: Int, right: Int)], start root: Int) {
    if memory[root].left != -1 {
        traverseInOrder(from: memory, start: memory[root].left)
    }
    if memory[root].left == -1, memory[root].right == -1 {
        print(memory[root].key)
    }
    if memory[root].right != -1 {
        traverseInOrder(from: memory, start: memory[root].right)
    }
}

let arrIn = readLine()!.split(separator: " ").map{Int($0)!}
// выделение памяти
var (memory, firstFree) = initMemory(arrIn.count)
// заполнение корня
let root = createAndFillNode(for: &memory, by: &firstFree, with: arrIn[0])
// формирование дерева если возможно
if memory[root].key != 0 {
    for i in arrIn.dropFirst() {
        if i == 0 {
            break
        }
        add(for: &memory, by: &firstFree, start: root, with: i)
    }
}
traverseInOrder(from: memory, start: root)
```

## F. Вывод развилок
https://contest.yandex.ru/contest/28069/problems/F/

```swift
func initMemory(_ maxN: Int) -> ([(key: Int, left: Int, right: Int)], Int) {
    var memory = [(key: Int, left: Int, right: Int)]()
    for i in 0..<maxN {
        memory.append((key: 0, left: i + 1, right: 0))
    }
    return (memory, 0)
}

func newNode(_ memory: [(key: Int, left: Int, right: Int)], _ firstFree: inout Int) -> Int {
    // выделение памяти
    let index = firstFree
    firstFree = memory[index].left
    return index
}

func delNode(_ memory: inout [(key: Int, left: Int, right: Int)], _ firstFree: inout Int, _ index: Int) {
    // для удаления по индексу, мы заменяем ссылку на следующий текущим первым свободным, и выдаем индекс как первый свободный
    memory[index].left = firstFree
    firstFree = index
}

// Бинарное дерево поиска
// Поиск
func find(from memory: [(key: Int, left: Int, right: Int)], start root: Int, for x: Int) -> Int {
    let key = memory[root].key
    if key == x {
        return root
    } else if key > x {
        // идем влево
        let left = memory[root].left
        if left == -1 {
            return -1
        } else {
            return find(from: memory, start: left, for: x)
        }
    } else {
        let right = memory[root].right
        if right == -1 {
            return -1
        } else {
            return find(from: memory, start: right, for: x)
        }
    }
}
// Добавление элемента и пример заполнения дерева
func createAndFillNode(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, with key: Int) -> Int {
    let index = newNode(memory, &firstFree)
    memory[index].key = key
    memory[index].left = -1
    memory[index].right = -1
    return index
}

func add(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, start root: Int, with x: Int) {
    let key = memory[root].key
    if key > x {
        //влево
        let left = memory[root].left
        if left == -1 {
            memory[root].left = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: left, with: x)
        }
    } else if key < x {
        //вправо
        let right = memory[root].right
        if right == -1 {
            memory[root].right = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: right, with: x)
        }
    }
}

func traverseInOrder(from memory: [(key: Int, left: Int, right: Int)], start root: Int) {
    if memory[root].left != -1 {
        traverseInOrder(from: memory, start: memory[root].left)
    }
    if memory[root].left != -1, memory[root].right != -1 {
        print(memory[root].key)
    }
    if memory[root].right != -1 {
        traverseInOrder(from: memory, start: memory[root].right)
    }
}

let arrIn = readLine()!.split(separator: " ").map{Int($0)!}
// выделение памяти
var (memory, firstFree) = initMemory(arrIn.count)
// заполнение корня
let root = createAndFillNode(for: &memory, by: &firstFree, with: arrIn[0])
// формирование дерева если возможно
if memory[root].key != 0 {
    for i in arrIn.dropFirst() {
        if i == 0 {
            break
        }
        add(for: &memory, by: &firstFree, start: root, with: i)
    }
}
traverseInOrder(from: memory, start: root)
```

## G. Вывод веток
https://contest.yandex.ru/contest/28069/problems/G/

```swift
func initMemory(_ maxN: Int) -> ([(key: Int, left: Int, right: Int)], Int) {
    var memory = [(key: Int, left: Int, right: Int)]()
    for i in 0..<maxN {
        memory.append((key: 0, left: i + 1, right: 0))
    }
    return (memory, 0)
}

func newNode(_ memory: [(key: Int, left: Int, right: Int)], _ firstFree: inout Int) -> Int {
    // выделение памяти
    let index = firstFree
    firstFree = memory[index].left
    return index
}

func delNode(_ memory: inout [(key: Int, left: Int, right: Int)], _ firstFree: inout Int, _ index: Int) {
    // для удаления по индексу, мы заменяем ссылку на следующий текущим первым свободным, и выдаем индекс как первый свободный
    memory[index].left = firstFree
    firstFree = index
}

// Бинарное дерево поиска
// Поиск
func find(from memory: [(key: Int, left: Int, right: Int)], start root: Int, for x: Int) -> Int {
    let key = memory[root].key
    if key == x {
        return root
    } else if key > x {
        // идем влево
        let left = memory[root].left
        if left == -1 {
            return -1
        } else {
            return find(from: memory, start: left, for: x)
        }
    } else {
        let right = memory[root].right
        if right == -1 {
            return -1
        } else {
            return find(from: memory, start: right, for: x)
        }
    }
}
// Добавление элемента и пример заполнения дерева
func createAndFillNode(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, with key: Int) -> Int {
    let index = newNode(memory, &firstFree)
    memory[index].key = key
    memory[index].left = -1
    memory[index].right = -1
    return index
}

func add(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, start root: Int, with x: Int) {
    let key = memory[root].key
    if key > x {
        //влево
        let left = memory[root].left
        if left == -1 {
            memory[root].left = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: left, with: x)
        }
    } else if key < x {
        //вправо
        let right = memory[root].right
        if right == -1 {
            memory[root].right = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: right, with: x)
        }
    }
}

func traverseInOrder(from memory: [(key: Int, left: Int, right: Int)], start root: Int) {
    if memory[root].left != -1 {
        traverseInOrder(from: memory, start: memory[root].left)
    }
    if memory[root].left != -1,  memory[root].right == -1 {
        print(memory[root].key)
    }
    if memory[root].left == -1,  memory[root].right != -1 {
        print(memory[root].key)
    }
    if memory[root].right != -1 {
        traverseInOrder(from: memory, start: memory[root].right)
    }
}

let arrIn = readLine()!.split(separator: " ").map{Int($0)!}
// выделение памяти
var (memory, firstFree) = initMemory(arrIn.count)
// заполнение корня
let root = createAndFillNode(for: &memory, by: &firstFree, with: arrIn[0])
// формирование дерева если возможно
if memory[root].key != 0 {
    for i in arrIn.dropFirst() {
        if i == 0 {
            break
        }
        add(for: &memory, by: &firstFree, start: root, with: i)
    }
}
traverseInOrder(from: memory, start: root)
```

## H. АВЛ-сбалансированность
https://contest.yandex.ru/contest/28069/problems/H/

```swift
func initMemory(_ maxN: Int) -> ([(key: Int, left: Int, right: Int)], Int) {
    var memory = [(key: Int, left: Int, right: Int)]()
    for i in 0..<maxN {
        memory.append((key: 0, left: i + 1, right: 0))
    }
    return (memory, 0)
}

func newNode(_ memory: [(key: Int, left: Int, right: Int)], _ firstFree: inout Int) -> Int {
    // выделение памяти
    let index = firstFree
    firstFree = memory[index].left
    return index
}

func delNode(_ memory: inout [(key: Int, left: Int, right: Int)], _ firstFree: inout Int, _ index: Int) {
    // для удаления по индексу, мы заменяем ссылку на следующий текущим первым свободным, и выдаем индекс как первый свободный
    memory[index].left = firstFree
    firstFree = index
}

// Бинарное дерево поиска
// Поиск
func find(from memory: [(key: Int, left: Int, right: Int)], start root: Int, for x: Int) -> Int {
    let key = memory[root].key
    if key == x {
        return root
    } else if key > x {
        // идем влево
        let left = memory[root].left
        if left == -1 {
            return -1
        } else {
            return find(from: memory, start: left, for: x)
        }
    } else {
        let right = memory[root].right
        if right == -1 {
            return -1
        } else {
            return find(from: memory, start: right, for: x)
        }
    }
}
// Добавление элемента и пример заполнения дерева
func createAndFillNode(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, with key: Int) -> Int {
    let index = newNode(memory, &firstFree)
    memory[index].key = key
    memory[index].left = -1
    memory[index].right = -1
    return index
}

func add(for memory: inout [(key: Int, left: Int, right: Int)], by firstFree: inout Int, start root: Int, with x: Int) {
    let key = memory[root].key
    if key > x {
        //влево
        let left = memory[root].left
        if left == -1 {
            memory[root].left = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: left, with: x)
        }
    } else if key < x {
        //вправо
        let right = memory[root].right
        if right == -1 {
            memory[root].right = createAndFillNode(for: &memory, by: &firstFree, with: x)
        } else {
            add(for: &memory, by: &firstFree, start: right, with: x)
        }
    }
}

func height(for memory: [(key: Int, left: Int, right: Int)], by root: Int) -> Int {
    if root == -1 {
        return 0
    } else {
        return 1 + max(height(for: memory, by: memory[root].left), height(for: memory, by: memory[root].right))
    }
}

let arrIn = readLine()!.split(separator: " ").map{Int($0)!}
// выделение памяти
var (memory, firstFree) = initMemory(arrIn.count)
// заполнение корня
let root = createAndFillNode(for: &memory, by: &firstFree, with: arrIn[0])

// формирование дерева если возможно
if memory[root].key != 0 {
    for i in arrIn.dropFirst() {
        if i == 0 {
            break
        }
        add(for: &memory, by: &firstFree, start: root, with: i)
    }
}

func heightMin(for memory: [(key: Int, left: Int, right: Int)], by root: Int) -> Int {
    if root == -1 {
        return 0
    } else {
        return 1 + min(height(for: memory, by: memory[root].left), height(for: memory, by: memory[root].right))
    }
}

var ans = "YES"
func traverseInOrderABL(from memory: [(key: Int, left: Int, right: Int)], start root: Int) {
    if memory[root].left != -1 {
        traverseInOrderABL(from: memory, start: memory[root].left)
    }
    if height(for: memory, by: root) - heightMin(for: memory, by: root) > 1 {
        ans = "NO"
    }
    //print(memory[root].key)
    if memory[root].right != -1 {
        traverseInOrderABL(from: memory, start: memory[root].right)
    }
}

traverseInOrderABL(from: memory, start: root)

print(ans)
```

## I. Родословная: число потомков
https://contest.yandex.ru/contest/28069/problems/I/

```swift
let n = Int(readLine()!)! - 1
var dic = [String: Int]() // предок - количество прямых потомков
var ans = [String: Int]() // предок - количество потомков
var childParent = [String: String]() // исходные данные

for _ in 0..<n {
    let str = readLine()!.split(separator: " ")
    let child = String(str[0])
    let parent = String(str[1])
    childParent[child] = parent

    if !dic.keys.contains(parent) {
        ans[parent] = 0
        dic[parent] = 0
    }
    dic[parent]! += 1
    
    if !dic.keys.contains(child) {
        dic[child] = 0
        ans[child] = 0
    }
}

var queue = Array(dic.filter{$0.value == 0}.keys)
var i = 0
while i < queue.count {
    let child = queue[i]
    if let parent = childParent[child] {
        ans[parent]! += ans[child]! + 1
        dic[parent]! -= 1
        if dic[parent]! == 0 {
            queue.append(parent)
        }
    }
    i += 1
}

for (key, value) in ans.sorted(by: {$0.key < $1.key}) {
    print(key, value)
}
```

## J. Родословная: подсчет уровней
https://contest.yandex.ru/contest/28069/problems/J/

```swift
let n = Int(readLine()!)! - 1
var dic = [String: [String]]() // предок - прямые потомки
var ans = [String: Int]() // предок - уровень
var children = [String]() // только потомки - для определения прародителя

for _ in 0..<n {
    let str = readLine()!.split(separator: " ")
    let child = String(str[0])
    children.append(child)
    let parent = String(str[1])
    
    if !dic.keys.contains(parent) {
        dic[parent] = [String]()
        ans[parent] = 0
    }
    dic[parent]!.append(child)
    
    if !dic.keys.contains(child) {
        dic[child] = [String]()
        ans[child] = 0
    }
}

let root = Set(dic.keys).subtracting(children).first! //прародитель
func dfs(for v: String, by level: Int) {
    ans[v] = level
    for child in dic[v]! {
        dfs(for: child, by: level + 1)
    }
}
dfs(for: root, by: 0)

for (key, value) in ans.sorted(by: {$0.key < $1.key}) {
    print(key, value)
}
```
