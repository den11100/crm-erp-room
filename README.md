## Аутентификация  
Все методы требуют передачи заголовка:  
```
X-Api-Key: {your_api_key}
```  
При отсутствии или некорректном ключе возвращается ошибка **400 Bad Request**:  
```json
{
  "name": "Bad Request",
  "message": "Bad API key",
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```  


## Получить информацию о Враче по его ID из ERP.  
## GET /api/erp/room/{erp_employee_doctor_id}  

### Пример запроса  
```http
GET /api/erp/room/491
Host: api.example.com
X-Api-Key: your_api_key
```

### Пример успешного ответа 200  
```json
{
  "status": 200,
  "data": {
    "id": 45,
    "office_id": 7,   
    "name": "Иванов Иван Иванович",
    "fio": "Иванов Иван Иванович",
    "specialty_id": 12,
    "comment": null,
    "min_time": "09:00:00",
    "max_time": "18:00:00",
    "grid_pitch": 30,
    "grid_pitch_alternative": null,
    "bot_enable": 1,
    "user_id_attached": 34,
    "user_id": null,
    "created_by": 2,
    "created_at": "2024-08-15 14:23:00",
    "is_deleted": 0,
    "erp_employee_id": 491,
    "erp_medoffice_id": 10,
    "title": "Иванов Иван Иванович, Врач-кардиолог",
    "achievements": "Кандидат медицинских наук",
    "education": "Первый МГМУ им. Сеченова",
    "experience": "10 лет",
    "hide_profession_in_title": 1,
    "erp_employee_doctor_id": 2,
    "is_khabarovsk": 0,
    "erp_analysis_ids": null,
    "accept_children": 0,
    "med_doctor_id": 0
  }
}
```

### Ошибки  
**404 Not Found**, если кабинет/врач не найден:  
```json
{
  "name": "Not Found",
  "message": "Кабинет/врач не найден",
  "status": 404,
  "type": "yii\\web\\NotFoundHttpException"
}
```  

## Создать нового Врача.
## POST /api/erp/room

### Тело запроса (JSON)

| Поле                      | Обязат. | Тип    | Описание                                                             |
|---------------------------|---------|--------|-----------------------------------------------------------------------|
| `erp_employee_doctor_id`  | Да      | int    | ERP  erp.employee_doctor.id                                           |
| `erp_employee_id`         | Да      | int    | ERP  erp.employee.id                                                  |
| `name`                    | Да      | string | Название кабинета или ФИО врача                                       |
| `specialty_id`            | Да      | int    | ID специальности (для ProDoctorov)                                    |
| `erp_medoffice_id`        | Да      | int    | ID медофиса в ERP (для вычисления `office_id`)                        |
| `title`                   | Нет     | string | Заголовок (ФИО, Должность, Специализация, Регалии) (из ERP)           |
| `achievements`            | Нет     | string | Достижения (из ERP)                                                   |
| `education`               | Нет     | string | Образование (из ERP)                                                  |
| `experience`              | Нет     | string | Опыт (из ERP)                                                         |
| `hide_profession_in_title`| Нет     | int    | Скрыть профессию в заголовке (из ERP)                                 |
| `is_khabarovsk`           | Нет     | int    | Для Хабаровска ?                                                      |
| `erp_analysis_ids`        | Нет     | array  | erp тесты id                                                          |
| `accept_children`         | Нет     | int    | Принимает ли детей ?                                                   |
| `med_doctor_id`           | Нет     | int    | Id врача из Медангел                                                  |


### Пример запроса
```http
POST /api/erp/room
Content-Type: application/json
X-Api-Key: your_api_key
```

```json
{
  "erp_employee_doctor_id": 3,
  "erp_employee_id": 321,
  "name": "Иванов Иван Иванович",
  "specialty_id": 5,
  "erp_medoffice_id": 10,
  "title": "Иванов Иван Иванович, Врач-кардиолог",
  "achievements": "Кандидат медицинских наук",
  "education": "Первый МГМУ им. Сеченова",
  "experience": "10 лет",
  "hide_profession_in_title": 1
}
```


### Пример успешного ответа 200  
```json
{
  "status": 200,
  "data": {
    "id": 46,
    "office_id": 3,
    "erp_employee_id": 321,
    "name": "Иванов Иван Иванович",
    "fio": "Иванов Иван Иванович",
    "specialty_id": 5,
    "comment": "",
    "min_time": "09:00:00",
    "max_time": "18:00:00",
    "grid_pitch": 30,
    "grid_pitch_alternative": null,
    "bot_enable": 1,
    "user_id_attached": null,
    "user_id": null,
    "created_by": 2,
    "created_at": "2025-04-17 10:05:12",
    "is_deleted": 0, 
    "erp_medoffice_id": 10,
    "title": "Иванов Иван Иванович, Врач-кардиолог",
    "achievements": "Кандидат медицинских наук",
    "education": "Первый МГМУ им. Сеченова",
    "experience": "10 лет",
    "hide_profession_in_title": 1,
    "erp_employee_doctor_id": 2,
    "is_khabarovsk": 0,
    "erp_analysis_ids": null,
    "accept_children": 0,
    "med_doctor_id": null,
  }
}
```

### Ошибки  
**400 Bad Request**, если пропущены обязательные поля:  
```json
{
  "name": "Bad Request",
  "message": "Не переданы обязательные параметры или пустые значения: erp_employee_id, name",
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```  
**422 Unprocessable Entity**, при ошибках валидации модели:  
```json
{
  "name": "Error",
  "message": "Ошибка валидации: specialty_id must be an integer",
  "status": 422,
  "type": "yii\\web\\HttpException"
}
```  
**400 Bad Request**, если тело не JSON или пустое:  
```json
{
  "name": "Bad Request",
  "message": "Пустое тело запроса",
  "status": 400,
  "type": "yii\\web\\BadRequestHttpException"
}
```  

## Обновить данные существующего Врача по его ID из ERP.  
## PUT /api/erp/room/{erp_employee_doctor_id}  


### Тело запроса (JSON)  
| Поле                      | Обязат. | Тип     | Описание                                       |
|---------------------------|---------|---------|------------------------------------------------|
| `erp_employee_id`         | Нет     | int     | ERP  erp.employee.id                           |
| `name`                    | Нет     | string  | ФИО врача                                      |
| `specialty_id`            | Нет     | int     | Новый ID специальности                          |
| `erp_medoffice_id`        | Нет     | int     | Новый ID медофиса в ERP (пересчёт `office_id`)  |
| `title`                   | Нет     | string  | Заголовок (ФИО, Должность, Специализация, Регалии) (из ERP)          |
| `achievements`            | Нет     | string  | Достижения (из ERP)                                                   |
| `education`               | Нет     | string  | Образование (из ERP)                                                  |
| `experience`              | Нет     | string  | Опыт (из ERP)                                                         |
| `hide_profession_in_title`| Нет     | int     | Скрыть профессию в заголовке (из ERP)                                |
| `is_khabarovsk`           | Нет     | int    | Для Хабаровска ?                                                      |
| `erp_analysis_ids`        | Нет     | array  | erp тесты id                                                         |
| `accept_children`         | Нет     | int   | Принимает ли детей ?                                                 |
| `med_doctor_id`           | Нет     | int   | Id врача из Медангел                                                  |

med_doctor_id

### Пример запроса  
```http
PUT /api/erp/room/321
Content-Type: application/json
X-Api-Key: your_api_key

{
  "name": "Иванов Иван Иванович",
  "specialty_id": 8,
  "experience": "10 лет"
}
```

### Пример успешного ответа 200  
```json
{
  "status": 200,
  "data": {
    "id": 46,
    "office_id": 3,
    "erp_employee_id": 321,
    "name": "Иванов Иван Иванович",
    "fio": "Иванов Иван Иванович",
    "specialty_id": 8,
    "comment": "коммент",
    "min_time": "09:00:00",
    "max_time": "18:00:00",
    "grid_pitch": 30,
    "grid_pitch_alternative": null,
    "bot_enable": 1,
    "user_id_attached": null,
    "user_id": null,
    "created_by": 2,
    "created_at": "2025-04-17 10:05:12",
    "is_deleted": 0,
    "erp_medoffice_id": 10,
    "title": "Иванов Иван Иванович, Врач-кардиолог",
    "achievements": "Кандидат медицинских наук",
    "education": "Первый МГМУ им. Сеченова",
    "experience": "10 лет",
    "hide_profession_in_title": 1,
    "erp_employee_doctor_id": 2,
    "is_khabarovsk": 0,
    "erp_analysis_ids": null,
    "accept_children": 0,
    "med_doctor_id": null,
  }
}
```

### Ошибки  
**404 Not Found**, если запись не найдена:  
```json
{
  "name": "Not Found",
  "message": "Кабинет/врач не найден",
  "status": 404,
  "type": "yii\\web\\NotFoundHttpException"
}
```  
**422 Unprocessable Entity**, при ошибках валидации:  
```json
{
  "name": "Error",
  "message": "Ошибка валидации: specialty_id must be positive integer",
  "status": 422,
  "type": "yii\\web\\HttpException"
}
```


## Изменение активности Врача

## PUT `/api/erp/room/{erp_employee_doctor_id}/change-active`

Изменяет статус активности врача, используя `erp_employee_doctor_id`.

Обязательный параметр:

- `is_deleted` (integer)    0 — врач активен; 1 — врач деактивирован

Тело запроса (JSON):

    {
      "is_deleted": 1
    }

Ответ:
```json
    {
  "status": 200,
  "data": {
    "id": 46,
    "office_id": 3,
    "erp_employee_id": 321,
    "name": "Иванов Иван Иванович",
    "fio": "Иванов Иван Иванович",
    "specialty_id": 8,
    "comment": "коммент",
    "min_time": "09:00:00",
    "max_time": "18:00:00",
    "grid_pitch": 30,
    "grid_pitch_alternative": null,
    "bot_enable": 1,
    "user_id_attached": null,
    "user_id": null,
    "created_by": 2,
    "created_at": "2025-04-17 10:05:12",
    "is_deleted": 1,
    "erp_medoffice_id": 10,
    "title": "Иванов Иван Иванович, Врач-кардиолог",
    "achievements": "Кандидат медицинских наук",
    "education": "Первый МГМУ им. Сеченова",
    "experience": "10 лет",
    "hide_profession_in_title": 1,
    "erp_employee_doctor_id": 2,
    "is_khabarovsk": 0,
    "erp_analysis_ids": null,
    "accept_children": 0,
    "med_doctor_id": null,
  }
}
```
Ошибки:

- `400 Bad Request` — если не передан параметр `is_deleted`.
- `404 Not Found` — если Врач не найден по `erp_employee_doctor_id`.



