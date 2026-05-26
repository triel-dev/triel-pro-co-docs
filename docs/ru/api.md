# Документация API Triel Pro Co

- [Аутентификация](#аутентификация)
  - [Получение токена доступа](#получение-токена-доступа)
  - [Использование токена](#использование-токена)
- [Управление пользователями](#управление-пользователями)
  - [Список пользователей](#список-пользователей)
  - [Список рабочих](#список-рабочих)
  - [Получение пользователя](#получение-пользователя)
  - [Создание пользователя](#создание-пользователя)
  - [Обновление пользователя](#обновление-пользователя)
  - [Удаление пользователя](#удаление-пользователя)
- [Управление категориями](#управление-категориями)
  - [Наборы категорий](#наборы-категорий)
    - [Список наборов категорий](#список-наборов-категорий)
    - [Получение набора категорий](#получение-набора-категорий)
    - [Создание набора категорий](#создание-набора-категорий)
    - [Обновление набора категорий](#обновление-набора-категорий)
    - [Удаление набора категорий](#удаление-набора-категорий)
  - [Категории](#категории)
    - [Список категорий в наборе](#список-категорий-в-наборе)
    - [Получение категории](#получение-категории)
    - [Создание категории](#создание-категории)
    - [Обновление категории](#обновление-категории)
    - [Удаление категории](#удаление-категории)
  - [Уникальные коды](#уникальные-коды-продукта)
    - [Загрузка уникальных кодов (JSON)](#загрузка-уникальных-кодов-json)
    - [Загрузка уникальных кодов (Файл)](#загрузка-уникальных-кодов-файл)
    - [Список уникальных кодов](#список-уникальных-кодов)
    - [Резервирование уникальных кодов](#резервирование-уникальных-кодов)
    - [Скачивание уникальных кодов (CSV)](#скачивание-уникальных-кодов-csv)
    - [Статистика уникальных кодов](#статистика-уникальных-кодов)
    - [Удаление уникальных кодов](#удаление-уникальных-кодов)
    - [Обновление напечатанных кодов](#обновление-напечатанных-кодов)
    - [Список напечатанных кодов](#список-напечатанных-кодов)
    - [Скачивание напечатанных кодов (CSV)](#скачивание-напечатанных-кодов-csv)
- [Управление этикетками и шаблонами](#управление-этикетками-и-шаблонами)
  - [Шаблоны этикеток](#шаблоны-этикеток)
    - [Список шаблонов](#список-шаблонов)
    - [Поиск шаблона](#поиск-шаблона)
    - [Получение шаблона](#получение-шаблона)
    - [Создание шаблона](#создание-шаблона)
    - [Обновление шаблона](#обновление-шаблона)
    - [Удаление шаблона](#удаление-шаблона)
    - [Установка шаблона по умолчанию](#установка-шаблона-по-умолчанию)
    - [Рендеринг шаблона](#рендеринг-шаблона)
  - [Варианты шаблонов](#варианты-шаблонов)
    - [Список вариантов](#список-вариантов)
    - [Получение варианта](#получение-варианта)
    - [Создание варианта](#создание-варианта)
    - [Обновление варианта](#обновление-варианта)
    - [Удаление варианта](#удаление-варианта)
  - [Размеры этикеток](#размеры-этикеток)
    - [Список размеров](#список-размеров)
    - [Получение размера](#получение-размера)
    - [Создание размера](#создание-размера)
    - [Обновление размера](#обновление-размера)
    - [Удаление размера](#удаление-размера)
- [Управление переменными](#управление-переменными)
  - [Список переменных](#список-переменных)
  - [Создание переменной](#создание-переменной)
  - [Обновление переменной](#обновление-переменной)
  - [Групповое обновление переменных](#групповое-обновление-переменных)
  - [Удаление переменной](#удаление-переменной)
  - [Список плейсхолдеров](#список-плейсхолдеров)
- [Управление производственными партиями](#управление-производственными-партиями)
  - [Список партий](#список-партий)
  - [Получение активной партии](#получение-активной-партии)
  - [Открытие партии](#открытие-партии)
  - [Закрытие партии](#закрытие-партии)
  - [Отчет по партии](#отчет-по-партии)
- [Счетчики](#счетчики)
  - [Список счетчиков](#список-счетчиков)
  - [Создание счетчика](#создание-счетчика)
  - [Обновление счетчика](#обновление-счетчика)
  - [Удаление счетчика](#удаление-счетчика)
  - [Список ключей счетчиков](#список-ключей-счетчиков)
  - [Поиск счетчиков](#поиск-счетчиков)
  - [Получение значений счетчиков](#получение-значений-счетчиков)
  - [Сброс значения счетчика](#сброс-значения-счетчика)
- [Оборудование](#оборудование)
  - [Список оборудования](#список-оборудования)
- [Печать и рендеринг](#печать-и-рендеринг)
  - [Отправка задания на печать](#отправка-задания-на-печать)
  - [Логи заданий на печать](#логи-заданий-на-печать)
  - [Рендеринг штрих-кода](#рендеринг-штрих-кода)

## Аутентификация

API использует JWT (JSON Web Token) для аутентификации. Все эндпоинты (кроме `/login`) требуют наличия валидного токена в заголовке `Authorization`.

### Получение токена доступа

Для получения токена доступа используйте эндпоинт логина.

**Эндпоинт**: `POST /api/v1/auth/login`  
**Описание**: Аутентифицирует пользователя и возвращает JWT.

**Тело запроса**:
```json
{
  "username": "admin",
  "password": "password"
}
```

**Ответ**:
```json
{
  "jwt": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImlhdCI6MTYy..."
}
```

### Использование токена

После получения JWT, включайте его в заголовок `Authorization` каждого запроса:

```text
Authorization: Bearer <your_access_token>
```

---

## Управление пользователями

Эндпоинты для управления пользователями системы и рабочими.

### Список пользователей
`GET /api/v1/users`  
Возвращает список всех пользователей.

**Ответ**:
```json
[
  {
    "id": 1,
    "username": "admin",
    "firstName": "Admin",
    "lastName": "System",
    "role": "ADMIN",
    "userType": "HUMAN"
  },
  {
    "id": 2,
    "username": "worker1",
    "firstName": "John",
    "lastName": "Doe",
    "role": "WORKER",
    "userType": "HUMAN"
  }
]
```

### Список рабочих
`GET /api/v1/users/workers`  
Возвращает список пользователей с ролью `WORKER` и типом `HUMAN`.

**Ответ**:
```json
[
  {
    "id": 2,
    "username": "worker1",
    "firstName": "John",
    "lastName": "Doe",
    "role": "WORKER",
    "userType": "HUMAN"
  }
]
```

### Получение пользователя
`GET /api/v1/users/{id}`  
Возвращает детали конкретного пользователя по ID.

**Ответ**:
```json
{
  "id": 1,
  "username": "admin",
  "firstName": "Admin",
  "lastName": "System",
  "role": "ADMIN",
  "userType": "HUMAN"
}
```

### Создание пользователя
`POST /api/v1/users`  
Создает нового пользователя.

**Тело запроса**:
```json
{
  "username": "jdoe",
  "firstName": "John",
  "lastName": "Doe",
  "password": "securepassword",
  "role": "WORKER",
  "userType": "HUMAN"
}
```

**Ответ**:
```json
{
  "id": 3,
  "username": "jdoe",
  "firstName": "John",
  "lastName": "Doe",
  "role": "WORKER",
  "userType": "HUMAN"
}
```

### Обновление пользователя
`PUT /api/v1/users/{id}`  
Обновляет существующего пользователя.

**Тело запроса**:
```json
{
  "username": "jdoe_updated",
  "firstName": "John",
  "lastName": "Smith",
  "password": "newpassword",
  "role": "WORKER",
  "userType": "HUMAN"
}
```

**Ответ**:
```json
{
  "id": 3,
  "username": "jdoe_updated",
  "firstName": "John",
  "lastName": "Smith",
  "role": "WORKER",
  "userType": "HUMAN"
}
```

### Удаление пользователя
`DELETE /api/v1/users/{id}`  
Удаляет пользователя из системы. Пользователи не могут удалять самих себя.

---

## Управление категориями

Управление наборами категорий и отдельными категориями. Категории используются для определения типов продуктов, их веса тары и связанных изображений.

### Наборы категорий

#### Список наборов категорий
`GET /api/v1/category-sets`  
Возвращает список всех наборов категорий.

**Ответ**:
```json
[
  {
    "id": "set_1",
    "name": "Dairy Products",
    "deleted": false
  }
]
```

#### Получение набора категорий
`GET /api/v1/category-sets/{id}`  
Возвращает конкретный набор категорий.

**Ответ**:
```json
{
  "id": "set_1",
  "name": "Dairy Products",
  "deleted": false
}
```

#### Создание набора категорий
`POST /api/v1/category-sets`  
Создает новый набор категорий.

**Тело запроса**:
```json
{
  "id": "set_1",
  "name": "Dairy Products"
}
```

**Ответ**:
```json
{
  "id": "set_1",
  "name": "Dairy Products",
  "deleted": false
}
```

#### Обновление набора категорий
`PUT /api/v1/category-sets/{id}`  
Обновляет существующий набор категорий.

**Тело запроса**:
```json
{
  "name": "Updated Dairy Products"
}
```

**Ответ**:
```json
{
  "id": "set_1",
  "name": "Updated Dairy Products",
  "deleted": false
}
```

#### Удаление набора категорий
`DELETE /api/v1/category-sets/{id}`  
Удаляет набор категорий.

**Ответ**:
```json
{
  "id": "set_1",
  "name": "Updated Dairy Products",
  "deleted": true
}
```

### Категории

#### Список категорий в наборе
`GET /api/v1/category-sets/{categorySetId}/categories`  
Возвращает все категории, принадлежащие конкретному набору.

**Ответ**:
```json
[
  {
    "id": "cat_1",
    "category_set": {
      "id": "set_1",
      "name": "Dairy Products",
      "deleted": false
    },
    "name": "Milk 3.2%",
    "external_id": "EXT-001",
    "tare_weight": 50,
    "pack_tare_weight": 200,
    "pallet_tare_weight": 5000,
    "has_image": true,
    "image_url": "/api/v1/categories/cat_1/image"
  }
]
```

#### Получение категории
`GET /api/v1/categories/{id}`  
Возвращает детали конкретной категории.

**Ответ**:
```json
{
  "id": "cat_1",
  "category_set": {
    "id": "set_1",
    "name": "Dairy Products",
    "deleted": false
  },
  "name": "Milk 3.2%",
  "external_id": "EXT-001",
  "tare_weight": 50,
  "pack_tare_weight": 200,
  "pallet_tare_weight": 5000,
  "has_image": true,
  "image_url": "/api/v1/categories/cat_1/image"
}
```

#### Создание категории
`POST /api/v1/category-sets/{categorySetId}/categories`  
Создает новую категорию внутри набора. Тип контента: `multipart/form-data`.

**Параметры**:
- `id` (string): Уникальный идентификатор.
- `name` (string): Отображаемое имя.
- `externalId` (string): Ссылка на внешнюю систему.
- `tareWeight` (int): Вес тары единичного продукта.
- `packTareWeight` (int): Вес тары упаковки.
- `palletTareWeight` (int): Вес тары паллеты.
- `image` (file): Необязательный файл изображения.

**Ответ**:
```json
{
  "id": "cat_1",
  "category_set": {
    "id": "set_1",
    "name": "Dairy Products",
    "deleted": false
  },
  "name": "Milk 3.2%",
  "external_id": "EXT-001",
  "tare_weight": 50,
  "pack_tare_weight": 200,
  "pallet_tare_weight": 5000,
  "has_image": true,
  "image_url": "/api/v1/categories/cat_1/image"
}
```

#### Обновление категории
`PUT /api/v1/categories/{id}`  
Обновляет существующую категорию. Тип контента: `multipart/form-data`.

**Ответ**:
```json
{
  "id": "cat_1",
  "category_set": {
    "id": "set_1",
    "name": "Dairy Products",
    "deleted": false
  },
  "name": "Milk 3.2% Updated",
  "external_id": "EXT-001",
  "tare_weight": 55,
  "pack_tare_weight": 210,
  "pallet_tare_weight": 5100,
  "has_image": true,
  "image_url": "/api/v1/categories/cat_1/image"
}
```

#### Удаление категории
`DELETE /api/v1/categories/{id}`  
Удаляет категорию.

### Уникальные коды продукта

Данный функционал используется для печати кодов выданных в государственных системах маркировки и прослеживания продукции:
- Честный знак (РФ)
- Электронный знак (РБ)
- Аналогичные системы иных государств

#### Загрузка уникальных кодов продукта в формате JSON
`POST /api/v1/categories/{categoryId}/unique-codes/upload/json`  
Загружает список уникальных кодов в формате JSON.

**Тело запроса**:
```json
["CODE1", "CODE2", "CODE3"]
```

**Ответ**:
```json
{
  "uploaded_count": 3,
  "ignored_count": 0
}
```

#### Загрузка уникальных кодов продукта в файлом
`POST /api/v1/categories/{categoryId}/unique-codes/upload/file`  
Загружает уникальные коды из файла. Тип контента: `multipart/form-data`.

**Параметры**:
- `file` (file): Файл, содержащий уникальные коды (по одному на строку).

**Ответ**:
```json
{
  "uploaded_count": 100,
  "ignored_count": 2
}
```

#### Список неиспользованных уникальных кодов
`GET /api/v1/categories/{categoryId}/unique-codes`  
Возвращает постраничный список всех доступных уникальных кодов для конкретной категории.

**Параметры запроса**:
- `page`, `size`: Параметры пагинации.

**Ответ**:
```json
{
  "content": [
    {
      "id": "code_1",
      "categoryId": "cat_1",
      "code": "010460...21...",
      "parts": {
        "01": "0460...",
        "21": "..."
      }
    }
  ],
  "pageable": {
    "sort": {
      "sorted": false,
      "unsorted": true,
      "empty": true
    },
    "offset": 0,
    "pageNumber": 0,
    "pageSize": 20,
    "paged": true,
    "unpaged": false
  },
  "totalPages": 1,
  "totalElements": 1,
  "last": true,
  "size": 20,
  "number": 0,
  "sort": {
    "sorted": false,
    "unsorted": true,
    "empty": true
  },
  "numberOfElements": 1,
  "first": true,
  "empty": false
}
```

#### Скачивание уникальных кодов (CSV)
`GET /api/v1/categories/{categoryId}/unique-codes/download/csv`  
Скачивает доступные уникальные коды в виде CSV файла.

**Ответ**:
Загрузка CSV файла.

#### Статистика уникальных кодов
`GET /api/v1/categories/{categoryId}/unique-codes/stats`  
Возвращает статистику по уникальным кодам для конкретной категории.

**Ответ**:
```json
{
  "totalCount": 1000,
  "reservedCount": 50,
  "printedCount": 25000
}
```

#### Удаление уникальных кодов
`DELETE /api/v1/categories/{categoryId}/unique-codes`  
Удаляет все доступные уникальные коды для конкретной категории.

**Ответ**:
```json
{
  "deletedCount": 100
}
```

#### Обновление напечатанных кодов
`POST /api/v1/categories/{categoryId}/printed/upsert`  
Обновляет или вставляет напечатанные уникальные коды.

**Тело запроса**:
Список объектов `PrintedUniqueCodeDto`.
```json
[
  {
    "id": "code-id-1",
    "code": "010460...21...",
    "categoryId": "cat_1",
    "machineId": "MACHINE-01",
    "printedAt": "2023-10-27T10:00:00",
    "validated": true
  }
]
```

**Ответ**:
200 OK

#### Список напечатанных кодов
`GET /api/v1/categories/{categoryId}/unique-codes/printed`  
Возвращает постраничный список напечатанных уникальных кодов.

**Параметры запроса**:
- `startDate` (ISO Date Time): Необязательный фильтр по дате начала.
- `endDate` (ISO Date Time): Необязательный фильтр по дате окончания.
- `page`, `size`, `sort`: Параметры пагинации.

**Ответ**:
Стандартный объект Page из Spring Data, содержащий элементы `PrintedUniqueCodeDto`.

#### Скачивание напечатанных кодов (CSV)
`GET /api/v1/categories/{categoryId}/unique-codes/printed/download/csv`  
Скачивает напечатанные уникальные коды в виде CSV файла.

**Параметры запроса**:
- `startDate` (ISO Date Time): Необязательный фильтр по дате начала.
- `endDate` (ISO Date Time): Необязательный фильтр по дате окончания.

**Ответ**:
Загрузка CSV файла.

---

## Управление этикетками и шаблонами

Эндпоинты для управления шаблонами этикеток и их конфигурациями.

### Шаблоны этикеток

#### Список шаблонов
`GET /api/v1/label-templates/list`  
Возвращает список шаблонов. Поддерживает параметр запроса `searchCriteria`.

**Ответ**:
```json
[
  {
    "id": "tmpl_1",
    "name": "Single Product Label",
    "content": "<xml>...</xml>",
    "category_id": "cat_1",
    "product_packaging_type": "SINGLE_PRODUCT",
    "dimension": {
      "id": 1,
      "width": 100,
      "height": 50
    },
    "is_fallback": false
  }
]
```

#### Поиск шаблона
`GET /api/v1/label-templates/find`  
Ищет шаблон на основе категории, набора и типа упаковки.
Параметры запроса: `categoryId`, `categorySetId`, `packagingType`.

**Ответ**:
```json
{
  "id": "tmpl_1",
  "name": "Single Product Label",
  "content": "<xml>...</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "id": 1,
    "width": 100,
    "height": 50
  },
  "is_fallback": false
}
```

#### Получение шаблона
`GET /api/v1/label-templates/{id}`  
Возвращает конкретный шаблон.

**Ответ**:
```json
{
  "id": "tmpl_1",
  "name": "Single Product Label",
  "content": "<xml>...</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "id": 1,
    "width": 100,
    "height": 50
  },
  "is_fallback": false
}
```

#### Создание шаблона
`POST /api/v1/label-templates`  
Создает новый шаблон этикетки.

**Тело запроса**:
```json
{
  "name": "Single Product Label",
  "content": "<xml>...</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "width": 100,
    "height": 50
  }
}
```

**Ответ**:
```json
{
  "id": "tmpl_2",
  "name": "Single Product Label",
  "content": "<xml>...</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "id": 2,
    "width": 100,
    "height": 50
  },
  "is_fallback": false
}
```

#### Обновление шаблона
`PUT /api/v1/label-templates/{id}`  
Обновляет существующий шаблон.

**Тело запроса**:
```json
{
  "name": "Updated Label",
  "content": "<xml>new content</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "width": 100,
    "height": 60
  }
}
```

**Ответ**:
```json
{
  "id": "tmpl_2",
  "name": "Updated Label",
  "content": "<xml>new content</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "id": 3,
    "width": 100,
    "height": 60
  },
  "is_fallback": false
}
```

#### Удаление шаблона
`DELETE /api/v1/label-templates/{id}`  
Удаляет шаблон.

#### Установка шаблона по умолчанию
`POST /api/v1/label-templates/{id}/fallback`  
Устанавливает указанный шаблон как шаблон по умолчанию (fallback).

#### Рендеринг шаблона
`GET /api/v1/label-templates/{id}/render`  
Рендерит шаблон в виде изображения JPEG.

**Ответ**:
Бинарные данные изображения (JPEG).

### Варианты шаблонов

#### Список вариантов
`GET /api/v1/label-template-variants`  
Возвращает все варианты шаблонов этикеток.

**Ответ**:
```json
[
  {
    "id": "var_1",
    "name": "Variant A"
  }
]
```

#### Получение варианта
`GET /api/v1/label-template-variants/{id}`  
Возвращает конкретный вариант.

**Ответ**:
```json
{
  "id": "var_1",
  "name": "Variant A"
}
```

#### Создание варианта
`POST /api/v1/label-template-variants`  
Создает новый вариант.

**Тело запроса**:
```json
{
  "id": "var_2",
  "name": "Variant B"
}
```

**Ответ**:
```json
{
  "id": "var_2",
  "name": "Variant B"
}
```

#### Обновление варианта
`PUT /api/v1/label-template-variants/{id}`  
Обновляет вариант.

**Тело запроса**:
```json
{
  "name": "Variant B Updated"
}
```

**Ответ**:
```json
{
  "id": "var_2",
  "name": "Variant B Updated"
}
```

#### Удаление варианта
`DELETE /api/v1/label-template-variants/{id}`  
Удаляет вариант.

### Размеры этикеток

#### Список размеров
`GET /api/v1/label-templates/dimension`  
Возвращает все определенные размеры этикеток.

**Ответ**:
```json
[
  {
    "id": 1,
    "width": 100,
    "height": 50
  }
]
```

#### Получение размера
`GET /api/v1/label-templates/dimension/{id}`  
Возвращает конкретный размер.

**Ответ**:
```json
{
  "id": 1,
  "width": 100,
  "height": 50
}
```

#### Создание размера
`POST /api/v1/label-templates/dimension`  
Создает новый размер.

**Тело запроса**:
```json
{
  "width": 80,
  "height": 40
}
```

**Ответ**:
```json
{
  "id": 2,
  "width": 80,
  "height": 40
}
```

#### Обновление размера
`PUT /api/v1/label-templates/dimension/{id}`  
Обновляет существующий размер.

**Тело запроса**:
```json
{
  "width": 85,
  "height": 45
}
```

**Ответ**:
```json
{
  "id": 2,
  "width": 85,
  "height": 45
}
```

#### Удаление размера
`DELETE /api/v1/label-templates/dimension/{id}`  
Удаляет размер.

---

## Управление переменными

Управление динамическими переменными и плейсхолдерами, используемыми в шаблонах этикеток.

### Список переменных
`GET /api/v1/variables/list`  
Возвращает список переменных шаблонов. Можно фильтровать по `searchCriteria`, `hot` (boolean) или `categoryId`.

**Ответ**:
```json
[
  {
    "id": "var_123",
    "category_set_id": "set_1",
    "category_set_name": "Dairy Products",
    "category_id": "cat_1",
    "category_name": "Milk 3.2%",
    "key": "expiration_days",
    "value": "7",
    "hot": true
  }
]
```

### Создание переменной
`POST /api/v1/variables`  
Создает новую переменную шаблона.

**Тело запроса**:
```json
{
  "category_set_id": "set_1",
  "category_id": "cat_1",
  "key": "expiration_days",
  "value": "7",
  "hot": true
}
```

**Ответ**:
```json
{
  "id": "var_124",
  "category_set_id": "set_1",
  "category_set_name": "Dairy Products",
  "category_id": "cat_1",
  "category_name": "Milk 3.2%",
  "key": "expiration_days",
  "value": "7",
  "hot": true
}
```

### Обновление переменной
`PUT /api/v1/variables/{id}`  
Обновляет переменную.

**Тело запроса**:
```json
{
  "value": "10",
  "hot": false
}
```

**Ответ**:
```json
{
  "id": "var_124",
  "category_set_id": "set_1",
  "category_set_name": "Dairy Products",
  "category_id": "cat_1",
  "category_name": "Milk 3.2%",
  "key": "expiration_days",
  "value": "10",
  "hot": false
}
```

### Групповое обновление переменных
`PATCH /api/v1/variables`  
Массово создает или обновляет несколько переменных.

**Тело запроса**:
```json
[
  {
    "category_id": "cat_1",
    "key": "line_id",
    "value": "Line 1"
  },
  {
    "id": "var_124",
    "value": "14"
  }
]
```

**Ответ**:
```json
[
  {
    "id": "var_125",
    "category_id": "cat_1",
    "key": "line_id",
    "value": "Line 1",
    "hot": false
  },
  {
    "id": "var_124",
    "category_id": "cat_1",
    "key": "expiration_days",
    "value": "14",
    "hot": false
  }
]
```

### Удаление переменной
`DELETE /api/v1/variables/{id}`  
Удаляет переменную.

### Список плейсхолдеров
`GET /api/v1/variables/placeholders`  
Возвращает все доступные плейсхолдеры (ключи), которые могут быть использованы в шаблонах.

**Ответ**:
```json
[
  {
    "variable": "expiration_days",
    "variable_type": "STRING",
    "type": "VARIABLE"
  },
  {
    "variable": "current_date",
    "variable_type": "DATE",
    "type": "SYSTEM"
  }
]
```

---

## Управление производственными партиями

Управление производственными партиями. Партия отслеживает производство конкретного продукта во времени, записывая количество и вес.

### Список партий
`GET /api/v1/product-batches`  
Возвращает список всех производственных партий.

**Ответ**:
```json
[
  {
    "id": 1,
    "name": "Batch 2023-01",
    "external_id": "EXT-B-01",
    "start": "2023-01-01T08:00:00",
    "end": "2023-01-01T17:00:00",
    "active": false,
    "products_count": 500,
    "products_net_weight": 250000,
    "products_gross_weight": 275000,
    "products_tare_weight": 25000,
    "created_at": "2023-01-01T08:00:00",
    "updated_at": "2023-01-01T17:00:00"
  }
]
```

### Получение активной партии
`GET /api/v1/product-batches/active`  
Возвращает детали текущей активной партии.

**Ответ**:
```json
{
  "id": 2,
  "name": "Batch 2023-02",
  "external_id": "EXT-B-02",
  "start": "2023-01-02T08:00:00",
  "active": true,
  "products_count": 50,
  "created_at": "2023-01-02T08:00:00"
}
```

### Открытие партии
`POST /api/v1/product-batches/open`  
Открывает новую производственную партию.
Параметры запроса: `name` (необязательно), `externalId` (необязательно).

**Ответ**:
```json
{
  "id": 2,
  "name": "Batch 2023-02",
  "external_id": "EXT-B-02",
  "start": "2023-01-02T08:00:00",
  "active": true,
  "products_count": 0,
  "created_at": "2023-01-02T08:00:00"
}
```

### Закрытие партии
`POST /api/v1/product-batches/close`  
Закрывает текущую активную партию.

### Отчет по партии
`GET /api/v1/product-batches/{id}/report`  
Генерирует производственный отчет для партии.
Параметры запроса: `type` (например, `PDF`, `EXCEL`).

**Ответ**:
Бинарный файл отчета.

---

## Счетчики

Управление производственными счетчиками для отслеживания количества на различных машинах и категориях.

### Список счетчиков
`GET /api/v1/counters/list`  
Возвращает все зарегистрированные счетчики.

**Ответ**:
```json
[
  {
    "id": "cnt_1",
    "name": "Main Production Counter",
    "key": "prod_total",
    "reset_value": 0,
    "category_id": "cat_1",
    "category_set_id": "set_1",
    "product_packaging_type": "SINGLE_PRODUCT"
  }
]
```

### Создание счетчика
`POST /api/v1/counters`  
Создает новый счетчик.

**Тело запроса**:
```json
{
  "name": "Line 2 Counter",
  "key": "line2_total",
  "reset_value": 0,
  "category_id": "cat_1",
  "category_set_id": "set_1",
  "product_packaging_type": "SINGLE_PRODUCT"
}
```

**Ответ**:
```json
{
  "id": "cnt_2",
  "name": "Line 2 Counter",
  "key": "line2_total",
  "reset_value": 0,
  "category_id": "cat_1",
  "category_set_id": "set_1",
  "product_packaging_type": "SINGLE_PRODUCT"
}
```

### Обновление счетчика
`PUT /api/v1/counters/{id}`  
Обновляет существующий счетчик.

**Тело запроса**:
```json
{
  "name": "Line 2 Counter Updated",
  "reset_value": 10
}
```

**Ответ**:
```json
{
  "id": "cnt_2",
  "name": "Line 2 Counter Updated",
  "key": "line2_total",
  "reset_value": 10,
  "category_id": "cat_1",
  "category_set_id": "set_1",
  "product_packaging_type": "SINGLE_PRODUCT"
}
```

### Удаление счетчика
`DELETE /api/v1/counters/{id}`  
Удаляет счетчик.

### Список ключей счетчиков
`GET /api/v1/counters/keys`  
Возвращает список всех уникальных ключей счетчиков.

**Ответ**:
```json
["prod_total", "line2_total"]
```

### Поиск счетчиков
`GET /api/v1/counters/search`  
Находит счетчики, соответствующие определенным критериям.
Параметры запроса: `categoryId`, `categorySetId`, `machineId`, `packagingType` (необязательно).

**Ответ**:
```json
[
  {
    "id": "cnt_1",
    "name": "Main Production Counter",
    "key": "prod_total",
    "value": 150
  }
]
```

### Получение значений счетчиков
`GET /api/v1/counters/{counterId}/values`  
Возвращает значения конкретного счетчика для всех машин.

**Ответ**:
```json
[
  {
    "counter_id": "cnt_1",
    "machine_id": "machine_A",
    "value": 150
  },
  {
    "counter_id": "cnt_1",
    "machine_id": "machine_B",
    "value": 45
  }
]
```

### Сброс значения счетчика
`POST /api/v1/counters/{counterId}/values/{machineId}/reset`  
Сбрасывает значение конкретного счетчика на конкретной машине.

---

## Оборудование

Управление аппаратным обеспечением и мониторинг состояния оборудования.

#### Список оборудования
`GET /api/v1/equipment`  
Возвращает список всего зарегистрированного оборудования и его текущие статусы.

**Ответ**:
```json
[
  {
    "id": 1,
    "eurekaInstanceId": "applicator:8081",
    "status": "UP",
    "instanceType": "APPLICATOR",
    "internalId": "APP-01",
    "machineId": "LINE-1",
    "hostName": "applicator-1.local",
    "ipAddr": "192.168.1.50",
    "port": 8081,
    "createdAt": "2023-01-01T10:00:00"
  }
]
```

---

## Печать и рендеринг

Эндпоинты для отправки заданий на печать и рендеринга штрих-кодов.

### Отправка задания на печать
`POST /api/v1/tasks/print-task`  
Отправляет задание на печать на один или несколько экземпляров оборудования.

**Тело запроса**:
```json
{
  "equipmentIds": [1, 2],
  "printTaskRequest": {
    "productCategoryId": "cat_123",
    "productTemplateId": "tmpl_456",
    "productPack": {
      "enabled": true,
      "itemsCountUnlimited": false,
      "maxProductCount": 12,
      "maxWeightUnlimited": true,
      "maxWeight": 0,
      "templateId": "tmpl_pack_789"
    },
    "productPallet": {
      "enabled": false,
      "itemsCountUnlimited": true,
      "maxProductCount": 0,
      "maxWeightUnlimited": true,
      "maxWeight": 0,
      "templateId": "tmpl_pallet_012"
    }
  }
}
```

**Ответ**:
```json
{
  "results": {
    "1": {
      "success": true,
      "statusCode": 200
    },
    "2": {
      "success": false,
      "errorMessage": "Equipment status is DOWN"
    }
  }
}
```

### Логи заданий на печать
`GET /api/v1/tasks/log`  
Возвращает историю отправленных заданий на печать.

**Ответ**:
```json
[
  {
    "id": 101,
    "overallStatus": "PARTIAL_SUCCESS",
    "request": {
      "equipmentIds": [1, 2],
      "printTaskRequest": {
        "productCategoryId": "cat_123",
        "productTemplateId": "tmpl_456",
        "productPack": {
          "enabled": true,
          "itemsCountUnlimited": false,
          "maxProductCount": 12,
          "maxWeightUnlimited": true,
          "maxWeight": 0,
          "templateId": "tmpl_pack_789"
        },
        "productPallet": {
          "enabled": false,
          "itemsCountUnlimited": true,
          "maxProductCount": 0,
          "maxWeightUnlimited": true,
          "maxWeight": 0,
          "templateId": "tmpl_pallet_012"
        }
      }
    },
    "response": {
      "results": {
        "1": { "success": true, "statusCode": 200 },
        "2": { "success": false, "errorMessage": "..." }
      }
    },
    "createdAt": "2023-01-01T12:00:00",
    "createdBy": {
      "id": 1,
      "username": "admin",
      "firstName": "Admin",
      "lastName": "System",
      "role": "ADMIN",
      "userType": "HUMAN"
    }
  }
]
```

### Рендеринг штрих-кода
`POST /api/v1/render/barcode`  
Рендерит штрих-код в виде строки, закодированной в Base64, на основе предоставленных свойств.

**Тело запроса**:
```json
{
  "content": "123456789012",
  "type": "EAN13",
  "width": "200",
  "height": "100"
}
```

**Ответ**:
```text
"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA..."
```
