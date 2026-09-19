# MITTEL Platform — Smoke Test

## Назначение
Быстрая проверка основных функций MITTEL Platform: подбор инструмента и отправка заявки.

## Окружение
- Frontend: http://127.0.0.1:8000
- API: http://127.0.0.1:5000
- Инструмент: OpenCode + Playwright MCP

## Сценарий 1 — Подбор инструмента
1. Открыть http://127.0.0.1:8000
2. Заполнить фильтры:
   - #f-diameter = 72
   - #f-type = Сухое с микроударом
   - #f-mount = М16
   - #f-length = 70
3. Нажать кнопку «Найти» (#filterForm submit)
4. Дождаться появления .result-card
5. Проверить артикулы: должны включать DB-SB72, DH-DE400.72, MH-SB72
6. Скриншот: screenshots/podbor.png

## Сценарий 2 — Отправка заявки
1. На карточке MH-SB72 нажать «Добавить в подбор»
2. Проверить: #podborCount = 1, #podborTotal = 3 900 ₽
3. Нажать #orderBtn — открывается модалка #orderModal
4. Заполнить:
   - #m-name = Playwright Test
   - #m-phone = +79991234567
5. Нажать submit формы #orderForm
6. Ожидание: #modalStatus класс is-ok, текст «Заявка отправлена! Номер: #N»
7. Проверка через API: GET http://127.0.0.1:5000/api/requests — последняя заявка с name=Playwright Test
8. Скриншот: screenshots/request.png

## Критерии успеха
- Оба сценария прошли без ошибок
- Скриншоты сохранены в screenshots/
- Заявка появилась в mittel.db

## Известные расхождения
- backend/app.py не содержит поля source при INSERT в requests (миграция урока 8 не применена)
- При быстром переключении фильтров клик по .result-card__add может не сработать до перерисовки карточек
