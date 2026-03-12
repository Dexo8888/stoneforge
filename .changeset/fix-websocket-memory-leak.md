---
"@stoneforge/ui": patch
---

fix: устранить утечку памяти в WebSocket клиенте

При длительном подключении WebSocket клиент не освобождал буферы событий из-за двух проблем:

1. `disconnect()` вызывал `socket.close()`, что синхронно триггерило `onclose` → `handleDisconnect()` → `scheduleReconnect()` — уже после того как `cancelReconnect()` был вызван. Клиент переподключался снова через reconnectDelay даже после явного disconnect.

2. При переподключении обработчики (`onopen`, `onmessage`, `onclose`, `onerror`) старого сокета не сбрасывались, что удерживало ссылки через замыкания и предотвращало сборку мусора.

Исправление: добавлен флаг `shouldReconnect` и метод `clearSocketHandlers` для явной очистки обработчиков.
