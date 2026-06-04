## Запуск стенда
```
docker compose up -d
```

## Протокол тестирования
1. Прямое обращение к приложению (без nginx)
```
curl http://localhost:8080
```
2. Через nginx3 -> app
```
curl http://localhost:8083
```
3. Через nginx2 -> nginx3 -> app
```
curl http://localhost:8082
```
4. Через полную цепочку: nginx1 -> nginx2 -> nginx3 -> app
```
curl http://localhost:8081
```
Ожидаемый результат ответа echo сервера, где первый IP — реальный IP клиента
```
  "headers": {
    "x-forwarded-for": "172.20.0.1, 172.20.0.5, 172.20.0.4",
    ...
  },
```
5. Тест на защиту от спуфинга
```
curl -H "X-Forwarded-For: 1.2.3.4, 5.6.7.8" http://localhost:8081
```
Результат:
```
"headers": {
    "x-forwarded-for": "172.20.0.1, 172.20.0.5, 172.20.0.4",
    ...
  },
```
