# Как справляться со сложностью

---

Более половины времени разработчики заняты доработкой и исправлением уже существующего кода

--- 

В идеале хочется иметь временные затраты О(1) для внесения доработок, а не О(n) или хуже
(где n - размер системы)

--- 

Компромисс. Что лучше:
1. Большее (но константное) время на разработку фичи в начале проекта. При этом в дальнейшем оно не сильно меняется.
2. Быстрая выдача фичи в начале проекта, с последующей деградацией при росте кодовой базы.

---

Управление сложностью:

1. Связанная (сфокусированная, cohesive) модульность
2. Сокрытие рисков и сложности на уровне слоя
3. Принцип открытости-закрытости
4. Наличие тестов, позволяющих вносить изменения без опасения регрессии
5. Формирование общего языка для общения с заказчиком

--- 

## Genezix

--- 

![Заказ](https://raw.githubusercontent.com/gilinykh/slides/master/genezix/order-page.png)
---
![Управление](https://raw.githubusercontent.com/gilinykh/slides/master/genezix/manage-page.png)
---
![Настройки](https://raw.githubusercontent.com/gilinykh/slides/master/genezix/profile-page.png)

---

Стандартный подход:

1. Сущность (что хранить -> таблица БД -> Java-класс). Получили `Equipment.java`, представляющий структуру для хранения.
2. Сервис (операции с сущностью). Получили `EquipmentService.java`, для операций с `Equipment.java`
3. ДАО и прочее

---

На другой странице другие данные? Делаем новую таблицу, сущность, сервис.

Очень удобно?

---

Что не так со стандартным подходом?

---

Дизайн завязывается на пользовательское представление (что увидели, то и воспроизвели в БД).

* Поменялось представление?
* Добавились поля?
* Изменился сценарий?

---

Как можно сделать иначе?

---

Контексты в предметной области

---

![Заказ - контексты](https://raw.githubusercontent.com/gilinykh/slides/master/genezix/order-page-contexts.png)
---
![Управление - контексты](https://raw.githubusercontent.com/gilinykh/slides/master/genezix/manage-page-contexts.png)
---
![Настройки - контексты](https://raw.githubusercontent.com/gilinykh/slides/master/genezix/profile-page-context.png)

---

![Контексты](https://github.com/gilinykh/slides/raw/master/genezix/components.svg?sanitize=true)

---

![Идентификация и авторизация](https://github.com/gilinykh/slides/raw/master/genezix/auth-classes.svg?sanitize=true)
![Каталог](https://github.com/gilinykh/slides/raw/master/genezix/catalog-classes.svg?sanitize=true)
![Запасы](https://github.com/gilinykh/slides/raw/master/genezix/inventory-classes.svg?sanitize=true)
![Заказы](https://github.com/gilinykh/slides/raw/master/genezix/orders-classes.svg?sanitize=true)
![Хостинг](https://github.com/gilinykh/slides/raw/master/genezix/vms-classes.svg?sanitize=true)

---

Архитектурные принципы

---

- Дешёвая модуляризация за счет выделения в отдельные пакеты ограниченных контекстов.
- Для объединения данных с различных контекстов реализуется модуль шлюза. 
- Взаимодействие с контекстом только через интерфейс.
- Только ссылочные связи между классами доменной модели разных модулей (например, UserId)
- Разделение каждого контекста на слои, где каждый слой скрывает свою часть сложности
+++
- В контроллере только биндинг входных параметров, передача управления сервису и представление вернувшегося результата.
- Репозитории вместо ДАО
- Для разработки UI не должен требоваться бэкенд. Использовать json-server / in-memory-web-api

---

Слои

1. Адаптеры
2. Сценарии
3. Модель

---

Вопросы?
