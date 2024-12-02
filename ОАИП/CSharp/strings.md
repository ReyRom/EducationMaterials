# Строки. Основные методы работы со строками в C#

Строки — один из ключевых типов данных в любом языке программирования. В C# строки представлены типом `string`, который является неизменяемым (immutable) объектом. Это означает, что при изменении строки создаётся новый объект, а старый остаётся неизменным.

**Примеры использования строк**:
- Хранение текста (имена, адреса, сообщения).
- Анализ текстовой информации.
- Форматирование и вывод данных.

## Создание строк

### Инициализация строк
```csharp
string str1 = "Привет, мир!";
string str2 = string.Empty; // Пустая строка
string str3 = null; // Строка без значения
```

### Интерполяция строк (с C# 6.0)
```csharp
int age = 20;
string message = $"Мне {age} лет.";
Console.WriteLine(message);
```

### Конкатенация строк
```csharp
string firstName = "Иван";
string lastName = "Иванов";
string fullName = firstName + " " + lastName;
Console.WriteLine(fullName);
```

## Основные методы класса `string`

### 1. **Извлечение символов**
```csharp
string text = "Программирование";
char firstChar = text[0];
Console.WriteLine(firstChar); // П
```

### 2. **Длина строки**
```csharp
string text = "Программирование";
int length = text.Length;
Console.WriteLine(length); // 16
```

### 3. **Изменение регистра**
```csharp
string text = "Программирование";
string upper = text.ToUpper();
string lower = text.ToLower();
Console.WriteLine(upper); // ПРОГРАММИРОВАНИЕ
Console.WriteLine(lower); // программирование
```

### 4. **Удаление пробелов**
```csharp
string text = "   Программирование   ";
string trimmed = text.Trim();
Console.WriteLine(trimmed); // "Программирование"
```

### 5. **Проверка наличия подстроки**
```csharp
string text = "Программирование";
bool contains = text.Contains("грамм");
Console.WriteLine(contains); // True
```

### 6. **Поиск символа или подстроки**
```csharp
string text = "Программирование";
int index = text.IndexOf("грамм");
Console.WriteLine(index); // 3
```

### 7. **Извлечение подстроки**
```csharp
string text = "Программирование";
string substring = text.Substring(3, 5);
Console.WriteLine(substring); // "грамм"
```

### 8. **Разделение строки**
```csharp
string text = "яблоко,банан,груша";
string[] fruits = text.Split(',');
foreach (string fruit in fruits)
{
    Console.WriteLine(fruit);
}
```

### 9. **Замена символов или подстрок**
```csharp
string text = "Программирование";
string replaced = text.Replace("грамм", "код");
Console.WriteLine(replaced); // "Прокодирование"
```

### 10. **Сравнение строк**
```csharp
string str1 = "Привет";
string str2 = "привет";
bool areEqual = str1.Equals(str2, StringComparison.OrdinalIgnoreCase);
Console.WriteLine(areEqual); // True
```

## Полезные методы

- **`StartsWith` и `EndsWith`** — проверяют, начинается или заканчивается строка определённой подстрокой:
```csharp
string text = "Программирование";
Console.WriteLine(text.StartsWith("Про")); // True
Console.WriteLine(text.EndsWith("ние")); // True
```

- **`IsNullOrEmpty` и `IsNullOrWhiteSpace`** — проверяют, является ли строка пустой или содержит только пробелы:
```csharp
string text = " ";
bool isEmpty = string.IsNullOrWhiteSpace(text);
Console.WriteLine(isEmpty); // True
```

- **`Join`** — объединяет элементы массива в строку:
```csharp
string[] words = { "Программирование", "это", "искусство" };
string sentence = string.Join(" ", words);
Console.WriteLine(sentence); // "Программирование это искусство"
```

- **`PadLeft` и `PadRight`** — добавляют пробелы или другие символы слева или справа:
```csharp
string text = "123";
string padded = text.PadLeft(5, '0');
Console.WriteLine(padded); // "00123"
```

## Неизменяемость строк и использование `StringBuilder`

При частом изменении строк рекомендуется использовать класс `StringBuilder`, так как он работает быстрее, создавая изменяемый буфер текста:
```csharp
using System.Text;

StringBuilder sb = new StringBuilder("Программирование");
sb.Append(" — это");
sb.Append(" круто!");
Console.WriteLine(sb.ToString()); // "Программирование — это круто!"
```

## Итоги

- `string` — неизменяемый тип данных.
- Методы класса `string` помогают эффективно работать с текстом.
- Для больших объёмов текстовых изменений рекомендуется использовать `StringBuilder`.