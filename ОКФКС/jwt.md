# Лекция: JSON Web Token (JWT) 

## Введение 

JSON Web Token (JWT) — это компактный, URL-безопасный формат для передачи информации между сторонами как JSON-объект. JWT широко используется для авторизации и аутентификации в веб-приложениях благодаря своей простоте, безопасности и способности легко интегрироваться с другими технологиями.

---

## Зачем нужен JWT?  

JWT решает проблему безопасной передачи данных между клиентом и сервером, позволяя:  
1. **Аутентифицировать пользователя** — сервер создает токен, который подтверждает личность пользователя.  
2. **Передавать данные между сторонами** — благодаря подписанию токена, данные внутри него нельзя подделать.  
3. **Упрощать масштабирование** — серверу не нужно хранить состояние пользователя (например, сессии) между запросами.

---

## Структура JWT  

JWT состоит из трех частей:  
1. **Header** (заголовок): метаданные о токене.  
2. **Payload** (полезная нагрузка): данные, которые передаются.  
3. **Signature** (подпись): защищает от изменений токена.  

Все три части кодируются в Base64URL и соединяются точкой (`.`):  

```plaintext
header.payload.signature
```

### Пример  
```plaintext
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
.
eyJ1c2VybmFtZSI6ImpvaG4iLCJyb2xlIjoiYWRtaW4iLCJpYXQiOjE2ODc4MzIwMDB9
.
h_Xk9E9aRkzlYI8YMe5J2MFT7d5dyfnTlL_dCfpwTWo
```

---

## 1. **Header**  

Header содержит информацию о типе токена и алгоритме подписи. Пример:  

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### Поля:  
- **alg**: алгоритм подписи (например, HS256, RS256).  
- **typ**: тип токена (обычно JWT).  

---

## 2. **Payload**  

Payload содержит данные (клеймы). Пример:  

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true,
  "iat": 1687832000
}
```

### Типы клеймов:  
1. **Registered Claims**: стандартизированные ключи (например, `iss`, `exp`, `sub`).  
2. **Public Claims**: пользовательские данные, доступные всем.  
3. **Private Claims**: данные, согласованные между сторонами.  

---

## 3. **Signature**  

Signature обеспечивает целостность токена. Она создается следующим образом:  

1. Берутся Header и Payload.  
2. Кодируются в Base64URL.  
3. Объединяются с помощью точки (`.`).  
4. Применяется алгоритм подписи с использованием секретного ключа:  

```plaintext
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

---

## Преимущества JWT  

1. **Самодостаточность**: JWT содержит всю необходимую информацию.  
2. **Масштабируемость**: сервер не хранит состояние сессии.  
3. **Безопасность**: благодаря подписи, данные невозможно подделать.  
4. **Удобство использования**: формат JSON легко обрабатывать на клиенте и сервере.  

---

## Недостатки JWT  

1. **Размер токена**: токены могут быть длинными, что увеличивает объем трафика.  
2. **Невозможность отзыва**: подписанные токены остаются валидными до истечения срока действия.  
3. **Безопасность ключа**: утечка секретного ключа ставит под угрозу всю систему.  

---

## Использование JWT  

1. **Авторизация**:  
   - После входа пользователь получает JWT.  
   - При каждом запросе клиент отправляет токен в заголовке `Authorization`.  

   ```http
   GET /protected HTTP/1.1
   Host: example.com
   Authorization: Bearer <JWT>
   ```

2. **Передача данных**:  
   - JWT может быть использован для обмена информацией между микросервисами.  

---

## Практика: Создание и проверка JWT  

### 1. Генерация JWT  

На сервере:  

```csharp
var header = new { alg = "HS256", typ = "JWT" };
var payload = new { sub = "1234567890", name = "John Doe", iat = DateTimeOffset.UtcNow.ToUnixTimeSeconds() };

string secret = "your-256-bit-secret";

string token = GenerateJwt(header, payload, secret);

string GenerateJwt(object header, object payload, string secret)
{
    string headerBase64 = Base64UrlEncode(JsonConvert.SerializeObject(header));
    string payloadBase64 = Base64UrlEncode(JsonConvert.SerializeObject(payload));
    string signature = HmacSha256($"{headerBase64}.{payloadBase64}", secret);

    return $"{headerBase64}.{payloadBase64}.{signature}";
}
```

```csharp
//Microsoft.AspNetCore.Authentication.JwtBearer;

var claims = new List<Claim> { new Claim(ClaimTypes.Name, username) };
var jwt = new JwtSecurityToken(
        issuer: app.Configuration["Issuer"],
        audience: app.Configuration["Audience"],
        claims: claims,
        expires: DateTime.UtcNow.Add(TimeSpan.FromMinutes(15)), // время действия 15 минут
        signingCredentials: new SigningCredentials(new SymmetricSecurityKey(Encoding.UTF8.GetBytes(builder.Configuration["SigningKey"])), SecurityAlgorithms.HmacSha256));

return new JwtSecurityTokenHandler().WriteToken(jwt);
```


### 2. Проверка JWT  

1. Распаковать Header и Payload.  
2. Проверить подпись.  
3. Убедиться в валидности клеймов (например, `exp`).  

---

## Лучшие практики  

1. **Храните секретный ключ в защищенном месте**.  
2. **Используйте HTTPS для передачи токенов**.  
3. **Ограничивайте срок действия токена (`exp`)**.  
4. **Используйте алгоритмы с асимметричной криптографией (например, RS256)**.  
5. **Храните токен в безопасном месте (например, в `HttpOnly` куки)**.  

---

## Заключение  

JWT — мощный инструмент для авторизации и аутентификации в современных веб-приложениях. Он прост в использовании, обеспечивает безопасность данных и идеально подходит для распределенных систем. Однако важно учитывать его ограничения и следовать лучшим практикам для защиты вашей системы.  