### Лекция: Работа с файлами в C#  

#### Введение  
Файлы играют ключевую роль в разработке программного обеспечения. Они позволяют сохранять данные между запусками программы, передавать их другим системам и организовывать долговременное хранение.  

В C# для работы с файлами предоставляется мощный набор инструментов, включая классы, методы и интерфейсы, находящиеся в пространстве имен **`System.IO`**.  

---

#### Основные понятия  

1. **Файловая система**  
   - Иерархическая структура хранения данных.
   - Состоит из файлов и директорий (папок).

2. **Типы файлов**  
   - **Текстовые файлы**: данные представлены в виде строк (например, `.txt`, `.csv`, `.json`, `.xml`).  
   - **Бинарные файлы**: данные хранятся в двоичном формате (например, `.dat`, `.exe`, `.jpg`).

---

#### Основные классы для работы с файлами  

1. **`File` и `FileInfo`**  
   - Предназначены для работы с файлами (создание, копирование, удаление, получение информации).
   - Различие:
     - `File` содержит статические методы.
     - `FileInfo` предоставляет методы экземпляра.  

2. **`Directory` и `DirectoryInfo`**  
   - Управляют папками (создание, удаление, получение списка файлов).
   - Различие аналогично классам `File` и `FileInfo`.

3. **`Path`**  
   - Предоставляет методы для работы с путями к файлам и папкам (например, извлечение расширения или имени файла).  

4. **`Stream` и его производные**  
   - Для чтения и записи данных в потоковом режиме:
     - `FileStream`: работа с файлами на низком уровне.  
     - `StreamReader` и `StreamWriter`: чтение и запись текстовых данных.  
     - `BinaryReader` и `BinaryWriter`: чтение и запись бинарных данных.  

---

#### Работа с текстовыми файлами  

1. **Запись в файл**  
```csharp
using System.IO;

class Program
{
    static void Main()
    {
        string path = "example.txt";
        string content = "Пример записи в файл.";

        // Метод File.WriteAllText
        File.WriteAllText(path, content);
        Console.WriteLine("Данные записаны.");
    }
}
```

2. **Чтение из файла**  
```csharp
class Program
{
    static void Main()
    {
        string path = "example.txt";

        // Метод File.ReadAllText
        if (File.Exists(path))
        {
            string content = File.ReadAllText(path);
            Console.WriteLine("Содержимое файла: ");
            Console.WriteLine(content);
        }
        else
        {
            Console.WriteLine("Файл не найден.");
        }
    }
}
```

3. **Запись строк в файл (построчно)**  
```csharp
class Program
{
    static void Main()
    {
        string path = "example_lines.txt";
        string[] lines = { "Строка 1", "Строка 2", "Строка 3" };

        File.WriteAllLines(path, lines);
        Console.WriteLine("Данные записаны построчно.");
    }
}
```

4. **Чтение строк из файла (построчно)**  
```csharp
class Program
{
    static void Main()
    {
        string path = "example_lines.txt";

        if (File.Exists(path))
        {
            string[] lines = File.ReadAllLines(path);
            Console.WriteLine("Содержимое файла:");
            foreach (var line in lines)
            {
                Console.WriteLine(line);
            }
        }
        else
        {
            Console.WriteLine("Файл не найден.");
        }
    }
}
```

---

#### Работа с бинарными файлами  

1. **Запись данных**  
```csharp
class Program
{
    static void Main()
    {
        string path = "example.dat";

        using (BinaryWriter writer = new BinaryWriter(File.Open(path, FileMode.Create)))
        {
            writer.Write(42);               // Целое число
            writer.Write(3.14);             // Число с плавающей точкой
            writer.Write("Привет, мир!");   // Строка
        }

        Console.WriteLine("Данные записаны в бинарный файл.");
    }
}
```

2. **Чтение данных**  
```csharp
class Program
{
    static void Main()
    {
        string path = "example.dat";

        if (File.Exists(path))
        {
            using (BinaryReader reader = new BinaryReader(File.Open(path, FileMode.Open)))
            {
                int intValue = reader.ReadInt32();
                double doubleValue = reader.ReadDouble();
                string stringValue = reader.ReadString();

                Console.WriteLine($"Целое: {intValue}, Число: {doubleValue}, Строка: {stringValue}");
            }
        }
        else
        {
            Console.WriteLine("Файл не найден.");
        }
    }
}
```

---

#### Работа с потоками  

1. **Использование `FileStream`**  
```csharp
class Program
{
    static void Main()
    {
        string path = "stream_example.txt";
        string content = "Работа с потоками в C#";

        // Запись в файл
        using (FileStream fs = new FileStream(path, FileMode.Create))
        {
            byte[] data = System.Text.Encoding.UTF8.GetBytes(content);
            fs.Write(data, 0, data.Length);
        }

        Console.WriteLine("Данные записаны в поток.");
    }
}
```

2. **Чтение из файла через поток**  
```csharp
class Program
{
    static void Main()
    {
        string path = "stream_example.txt";

        if (File.Exists(path))
        {
            using (FileStream fs = new FileStream(path, FileMode.Open))
            {
                byte[] data = new byte[fs.Length];
                fs.Read(data, 0, data.Length);
                string content = System.Text.Encoding.UTF8.GetString(data);

                Console.WriteLine("Содержимое файла:");
                Console.WriteLine(content);
            }
        }
        else
        {
            Console.WriteLine("Файл не найден.");
        }
    }
}
```

---

#### Работа с путями  

Класс **`Path`** упрощает работу с именами файлов и директорий:  
```csharp
string path = @"C:\example\file.txt";

// Получение информации о пути
Console.WriteLine(Path.GetDirectoryName(path)); // Папка
Console.WriteLine(Path.GetFileName(path));      // Имя файла
Console.WriteLine(Path.GetExtension(path));    // Расширение
```

---

#### Полезные советы  

1. **Исключения и обработка ошибок**  
   - Используйте `try-catch` для обработки ошибок ввода/вывода:
     ```csharp
     try
     {
         File.ReadAllText("nonexistent.txt");
     }
     catch (IOException ex)
     {
         Console.WriteLine($"Ошибка: {ex.Message}");
     }
     ```

2. **Использование `using`**  
   - Позволяет автоматически закрывать файлы после использования.

3. **Проверка существования файлов/папок**  
   - Используйте методы `File.Exists` и `Directory.Exists`.

---

#### Заключение  

Работа с файлами в C# позволяет эффективно хранить и обрабатывать данные. Знание основных классов, таких как `File`, `FileStream`, и `Path`, значительно упрощает разработку приложений, взаимодействующих с файловой системой.  
