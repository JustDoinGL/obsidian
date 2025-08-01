---
links:
  - https://youtu.be/q0u4yRUSDzI?si=B7ymXYZlvHI0aWL0
---
Сервер не может давать любые данные всем подряд, поэтому клиент сначала должен представиться. Стоит различать: 
- Авторизацию
- Аутентификацию 
- Идентификацию.

## Идентификация 
— это процедура распознавания субъекта по его идентификатору (проще говоря, это определение имени, логина или номера).

## Аутентификация 
— это процедура проверки подлинности (пользователя проверяют с помощью пароля, письмо проверяют по электронной подписи и т.д.).

## Авторизация 
— это предоставление доступа к какому-либо ресурсу (например, к электронной почте).

![[Screenshot 2024-03-10 at 13.56.24.png]]

## Виды авторизации. 
### Сессии
- ﻿﻿Хранится в [[Cookies]]  
    Сервер кладёт `id` сессии в `cookies`, которые отправляет клиенту при успешной авторизации
- ﻿﻿При каждом запросе отправляется на сервер
- ﻿﻿Server-side (в основном)
- ﻿﻿Не для публичного АРІ
#### Клиент-серверное взаимодействии при сессии
![[Screenshot 2024-03-10 at 14.00.07.png]]
### Токены
[[JWT]]
- ﻿﻿Публичное API
- ﻿﻿Микросервисная архитектура
- ﻿﻿Обычно отправляется в заголовках запроса
- ﻿﻿В браузере хранится либо в DOM-хранилищах  
    (sessionStorage/localStorage), либо в тех же Cookies
- ﻿﻿Короткое время жизни
#### Клиент-серверное взаимодействие при токенах
![[Screenshot 2024-03-10 at 14.04.18.png]]