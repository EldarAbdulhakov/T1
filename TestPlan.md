# План тестирования

## 1. Введение
### 1.1 Цель тестирования
Проверить корректность работы конечной точки API для получения статуса продукта.

Проверить корректную обработку:
* параметров запроса (обязательных и необязательных)
* авторизации (authToken)
* обработки несуществующих и некорректных данных
* формата и содержимого ответа (JSON)
* кодов ответов HTTP

### 1.2 Персонал
Один тестировщик (100% загруженность за все время проекта). Время: 14 рабочих дней (112 рабочих часов)

## 2. Объект тестирования
Метод GET /products/{productid}/status?authToken=&recalculate=&owner=&region=

## 3. Аппаратные и программные требования
* Программное обеспечение: Windows 11
* Аппаратное обеспечение: Процессор Intel(R) Core(TM) i3-2310M CPU @ 2.10GHz, 16 Gb RAM

## 4. Ресурсы тестирования
* Коммуникация (Telegramm)
* Документация (GitHub)
* Тестирование (Postman)
* Скриншоты (Icecream Screen Recorder 7)

## 5. Типы проверок
* Позитивные тесты
* Негативные тесты
* Тестирование границ
* Тестирование безопасности

## 6. Критерии завершенности
* Тестовое покрытие требований: 90%
* Выполнение тестов: 100%
* Успешно пройденные тесты: 80%

## 7. Риски
* Время: Заказчик указал крайний срок 17.10. Рекомендуется приложить все усилия, чтобы завершить проект к 15.10, чтобы один день оставался доступным для любых неожиданных проблем
* Отключение электричества
* Заболевание тестировщика

## 8. Сроки тестирования
* Срок согласования предложений по совершенствованию требований - 08.10.2025
* Срок написания тест-плана - 09.10.2025
* Выполнение тест-кейсов - 11.10.2025
* Срок составления отчетов - 15.10.2025
* Срок сдачи проекта - 17.10.2025


# Описания тест-кейсов

1. Запрос с корректными и всеми необязательными параметрами (recalculate=true, owner=Создатель, region=Северо-Запад):

       /products/123/status?authToken=a1d5f8gh7h42y35j&recalculate=true&owner=Создатель&region=Северо-Запад
       ОР: Код 200 OK, JSON{"productStatus":1} или {"productStatus":0}
2. Запрос с корректными и всеми необязательными параметрами (recalculate=false, owner=Пользователь, region=Сибирь):

       /products/13/status?authToken=a1d5f8gh7h42y35j&recalculate=false&owner=Пользователь&region=Сибирь
       ОР: Код 200 OK, JSON{"productStatus":1} или {"productStatus":0}
3. Запрос с корректными и всеми необязательными параметрами (recalculate=null, owner=null, region=Поволжье):

       /products/9/status?authToken=a1d5f8gh7h42y35j&recalculate=null&owner=null&region=Поволжье
       ОР: Код 200 OK, JSON{"productStatus":1} или {"productStatus":0}
4. Запрос с корректными обязательными параметрами и всеми необязательными параметрами без указания значений):

       /products/9/status?authToken=a1d5f8gh7h42y35j&recalculate=&owner=&region=
       ОР: Код 200 OK, JSON{"productStatus":1} или {"productStatus":0}
4. Запрос с корректными обязательными и без необязательных параметров (только authToken и productId)

       /products/547/status?authToken=a1d5f8gh7h42y35j
       ОР: Код 200 OK, JSON{"productStatus":1} или {"productStatus":0}
5. Запрос с productId на границе (минимальный номер productId)

       /products/1/status?authToken=a1d5f8gh7h42y35j&recalculate=true&owner=Создатель&region=Северо-Запад
       ОР: Код 200 OK, JSON{"productStatus":1} или {"productStatus":0}
6. Запрос с productId на границе (максимальный номер productId)

       /products/100500/status?authToken=a1d5f8gh7h42y35j
       ОР: Код 200 OK, JSON{"productStatus":1} или {"productStatus":0}
7. Запрос с отсутствующим authToken

       /products/100/status
       ОР: Код 401 Unauthorized
8. Запрос с невалидным authToken (меньше 16 символов, например, a1d5f8gh7h42y35)

       /products/174/status?authToken=a1d5f8gh7h42y35&recalculate=false&owner=Пользователь&region=Сибирь
       ОР: Код 401 Unauthorized
9. Запрос с невалидным authToken (больше 16 символов, например, a1d5f8g47h7h42y35)

       /products/174/status?authToken=a1d5f8g47h7h42y35&recalculate=false&owner=Пользователь&region=Сибирь
       ОР: Код 401 Unauthorized
10. Запрос с невалидным authToken (с недопустимыми символами, например, a1d5f8!"7h7h42y35)

        /products/174/status?authToken=a1d5f8!"7h7h42y35&recalculate=false&owner=Пользователь&region=Сибирь
        ОР: Код 401 Unauthorized
11. Запрос с несуществующим productId (например, 999)

        /products/999/status?authToken=a1d5f8gh7h42y35j
        ОР: Код 404 Not Found
12. Запрос с невалидным productId (не число, например, фыв)

        /products/фыв/status?authToken=a1d5f8gh7h42y35j
        ОР: Код 400 Bad Request или 404 Not Found
13. Запрос с невалидным productId (отрицательное число, например, -1)

        /products/-1/status?authToken=a1d5f8gh7h42y35j
        ОР: Код 400 Bad Request или 404 Not Found
14. Запрос с невалидным значением recalculate (например, invalid)

        /products/1/status?authToken=a1d5f8gh7h42y35j&recalculate=invalid&owner=Создатель&region=Северо-Запад
        ОР: Код 400 Bad Request 
15. Запрос с невалидным значением owner (например, Незнакомец)

        /products/9/status?authToken=a1d5f8gh7h42y35j&recalculate=null&owner=Незнакомец&region=Поволжье
        ОР: Код 400 Bad Request 
16. Запрос с невалидным значением region (например, Юг)

        /products/9/status?authToken=a1d5f8gh7h42y35j&recalculate=null&owner=null&region=Юг
        ОР: Код 400 Bad Request 
17. Запрос с симуляцией внутренней ошибки (например, отключить базу данных)

        /products/123/status?authToken=a1d5f8gh7h42y35j&recalculate=true&owner=Создатель&region=Северо-Запад
        ОР: Код 500, JSON {"errorMessage": "Описание ошибки"}
18. Запрос с невалидным authToken (ровно 16 символов только цифры, например, 5236985145265237) 

        /products/174/status?authToken=5236985145265237&recalculate=false&owner=Пользователь&region=Сибирь
        ОР: Код 401 Unauthorized
19. Запрос с невалидным authToken (ровно 16 символов только буквы, например, jnhytgvbhjnadprf) 

        /products/174/status?authToken=jnhytgvbhjnadprf
        ОР: Код 401 Unauthorized
20. Запрос с проверкой на инъекции в authToken (authToken с SQL-кодом, например, 1';DROP TABLE users';)

        /products/174/status?authToken=1';DROP TABLE users';
        ОР: Код 401 Unauthorized, без выполнения инъекции
21. Запрос с проверкой на инъекции в необязательных параметрах (например, owner со скриптом <script>alert(1)</script>)

        /products/123/status?authToken=a1d5f8gh7h42y35j&recalculate=true&owner=<script>alert(1)</script>&region=Северо-Запад
        ОР: Код 400 Bad Request, без выполнения скрипта 































