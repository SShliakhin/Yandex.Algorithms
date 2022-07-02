# ДЗ B1 Сложность, тестирование, особые случаи
https://contest.yandex.ru/contest/28730/enter/

## A. Interactor
https://contest.yandex.ru/contest/28730/problems/A/

```swift
let r = Int(readLine()!)! // код завершения задачи -127 - 127
let i = Int(readLine()!)! // вердикт интерактора 0 - 7
let c = Int(readLine()!)! // вердикт чекера 0 - 7

var res = i
switch i {
case 0: res = r != 0 ? 3 : c
case 1: res = c
case 4: res = r != 0 ? 3 : 4
case 6: res = 0
case 7: res = 1
default: res = i
}

print(res)
```

## B. Кольцевая линия метро
https://contest.yandex.ru/contest/28730/problems/B/

```swift
let arr = readLine()!.split(separator: " ").map { Int($0)! }
let n = arr[0]
let i = arr[1]
let j = arr[2]
let res = min((max(i,j) - min(i,j) - 1),
              (min(i,j) + n - max(i,j) - 1))
print(res)
```

## C. Даты
https://contest.yandex.ru/contest/28730/problems/C/

```swift
let arr = readLine()!.split(separator: " ").map{Int($0)!}
let x = arr[0]
let y = arr[1]
let z = arr[2]
let res = x > 12 || y > 12 || x == y ? 1 : 0
print(res)
```

## D. Строительство школы
https://contest.yandex.ru/contest/28730/problems/D/

```swift
let n = Int(readLine()!)! // количество учеников
let arr = readLine()!.split(separator: " ").map { Int($0)! } // координаты учеников
if n % 2 == 0 {
    //четное
    print(arr[(n - 1)/2 + 1])
} else {
    // нечетное
    print(arr[(n - 1)/2])
}
```

## E. Точка и треугольник
https://contest.yandex.ru/contest/28730/problems/E/

- пусть твоя точка имеет координаты (х,у), а вершины треугольника - (х1,у1), (х2,у2), (х3,у3).
- точка будет принадлежать треугольнику, если будет принадлежать одновременно трем полуплоскостям, пересечение которых и есть треугольник.
- точка с координатами (х,у) принадлежит полуплоскости, когда находится по одну из сторон некоторой прямой, а именно y-(kx+b)>=0 , где k, b - параметры данной прямой.
- далее нужно найти уравнения прямых, которые содержат стороны треугольника, составить 3 неравенства, и решить систему из этих 3х неравенств.
- составить уравнение прямых можно с помощью уравнения прямой, проходящей через 2 точки на плоскости

- Уже было - https://www.cyberforum.ru/showthread.php?t=8234

Математическая часть - векторное и псевдоскалярное произведения.

Реализация - считаются произведения (1, 2, 3 - вершины треугольника, 0 - точка):

* (x1 - x0) * (y2 - y1) - (x2 - x1) * (y1 - y0)
* (x2 - x0) * (y3 - y2) - (x3 - x2) * (y2 - y0)
* (x3 - x0) * (y1 - y3) - (x1 - x3) * (y3 - y0)

Если они одинакового знака, то точка внутри треугольника, если что-то из этого - ноль, то точка лежит на стороне, иначе точка вне треугольника.

- уровнение прямой проходящей через 2 точки https://upload.wikimedia.org/math/d/3/8/d38db0a192f94ce9fb7902481a22cd00.png 
- расстояние между двумя точками d = корень из суммы квадратов (x2 - x1) и (y2 - y1)

```swift
let d = Int(readLine()!)! // катет
let arr = readLine()!.split(separator: " ").map { Int($0)! } // координаты точки

let x1 = 0
let y1 = 0
let x2 = d
let y2 = 0
let x3 = 0
let y3 = d
let x = arr[0]
let y = arr[1]

func getP(x0: Int, y0: Int, x1: Int, y1: Int, x2: Int, y2: Int) -> Int {
    return (x1 - x0) * (y2 - y1) - (x2 - x1) * (y1 - y0)
}

func getR(x0: Int, y0: Int, x1: Int, y1: Int) -> Int {
    return (x1 - x0) * (x1 - x0) + (y1 - y0) * (y1 - y0)
}

let pr1 = getP(x0: x, y0: y, x1: x1, y1: y1, x2: x2, y2: y2)
let pr2 = getP(x0: x, y0: y, x1: x3, y1: y3, x2: x1, y2: y1)
let pr3 = getP(x0: x, y0: y, x1: x2, y1: y2, x2: x3, y2: y3)

if (pr1 == 0 || pr2 == 0 || pr3 == 0) ||
    (pr1 > 0 && pr2 > 0 && pr3 > 0) ||
    (pr1 < 0 && pr2 < 0 && pr3 < 0) {
    print(0)
} else {
    let r1 = getR(x0: x, y0: y, x1: x1, y1: y1)
    let r2 = getR(x0: x, y0: y, x1: x2, y1: y2)
    let r3 = getR(x0: x, y0: y, x1: x3, y1: y3)
    switch min(r1, r2, r3) {
    case r1: print(1)
    case r2: print(2)
    case r3: print(3)
    default: fatalError()
    }
}
```
