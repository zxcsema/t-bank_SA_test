# Тестовые сценарии

| ID | Сценарий | Предусловия | Входные данные | Ожидаемый результат | Источник |
|---|---|---|---|---|---|
| TC-01 | Успешный ответ ML | ML доступен, `confidence >= threshold` | `recommendationType = HOTEL`, `limit = 10` | `200`, список рекомендаций, `source = ML` | ML |
| TC-02 | Ошибка 500 от ML | ML возвращает HTTP 500 | Валидный запрос | Запускается fallback, лог `ML_HTTP_5XX`, ответ `200` при успешном fallback | FALLBACK |
| TC-03 | Сетевая ошибка ML | Соединение с ML невозможно | Валидный запрос | Запускается fallback, лог `ML_NETWORK_ERROR`, ответ `200` при успешном fallback | FALLBACK |
| TC-04 | Тайм-аут ML | ML отвечает дольше 3 секунд | Валидный запрос | Ожидание прекращается, лог `ML_TIMEOUT`, ответ `200` при успешном fallback | FALLBACK |
| TC-05 | Низкий confidence | ML возвращает `confidence < threshold` | Валидный запрос | ML-ответ не возвращается, лог `LOW_CONFIDENCE`, запускается fallback | FALLBACK |
| TC-06 | Пустой ответ ML | ML возвращает `items = []` | Валидный запрос | Лог `ML_EMPTY_RESULT`, запускается fallback | FALLBACK |
| TC-07 | Отсутствует история | `User History Service` вернул 404 или пустую историю | Валидный запрос | Fallback использует профиль, параметры поездки и популярность | FALLBACK |
| TC-08 | Отсутствует профиль | `User Profile Service` вернул 404 или пустой профиль | Валидный запрос | Fallback использует историю, параметры поездки и популярность | FALLBACK |
| TC-09 | Новый пользователь | Нет профиля или история пустая | Валидный запрос | Fallback использует параметры поездки и популярность | FALLBACK |
| TC-10 | Недоступен Popularity Service | `Popularity Service` возвращает 5xx | Валидный запрос | Fallback использует профиль и историю; если результата нет, `503` | FALLBACK или ошибка |
| TC-11 | Все внутренние источники недоступны | Профиль, история и популярность недоступны | Валидный запрос, ML тоже недоступен | `503 RECOMMENDATIONS_UNAVAILABLE`, технические причины ML не раскрываются | Нет |
| TC-12 | Некорректный запрос | Не передан `requestId` или неверный `recommendationType` | Невалидный JSON или поле | `400 INVALID_REQUEST` | Нет |
| TC-13 | Запрос рекомендаций отелей | Доступны объекты типа `HOTEL` | `recommendationType = HOTEL` | В `items[].entityType` только `HOTEL` | ML или FALLBACK |
| TC-14 | Запрос рекомендаций активностей | Доступны объекты типа `ACTIVITY` | `recommendationType = ACTIVITY` | В `items[].entityType` только `ACTIVITY` | ML или FALLBACK |
| TC-15 | Запрос рекомендаций маршрутов | Доступны объекты типа `ROUTE` | `recommendationType = ROUTE` | В `items[].entityType` только `ROUTE` | ML или FALLBACK |

