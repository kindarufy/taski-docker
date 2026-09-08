# Taski frontend

React-интерфейс задач. Полный запуск описан в [корневом README](../README.md).
Нужен Node.js 22. Из корня репозитория:

```bash
cd frontend
npm ci
npm start
```

Откройте http://localhost:3000/. Backend должен работать на http://127.0.0.1:8000;
dev-server проксирует запросы API по этому адресу. Остановка: Ctrl+C.

Сборка: `npm run build`. Тесты без watch: `npm test -- --watchAll=false`.
