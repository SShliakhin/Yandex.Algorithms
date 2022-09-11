# Input / Output

Ссылка с образцами ввода-вывода для всех языков тут: https://yandex.ru/support/contest/examples-stdin-stdout.html  
Значения ошибок, возвращаемых проверяющей системой: https://contest.yandex.ru/errors/

## Прочитать:
1. строку: `let string = readLine()!`
2. число: `let number = Int(readLine()!)!`
3. массив строк: `let stringArray = readLine()!.components(separatedBy: " ")`
4. массив чисел: `let numberArray = readLine()!.components(separatedBy: " ").map{Int($0)!}`

***Замечание***: 
1. использование `.components` требует `import Foundation`
2. бывает "не чистый" ввод со служебными символами, которые не нужны и приводят к **RE**, тогда вместо `readLine()!` пишем `readLine()!.trimmingCharacters(in: .whitespacesAndNewlines)`. Используем аккуратно, тоже использует время и может привести к **TL**.

## Прочитать, когда не известно количество строк:  
### Использование ввода/вывода через файл:  
Чтение и запись в файл: https://yandex.ru/support/contest/examples-file.html

## Вывести:
## Замер производительности:
