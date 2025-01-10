# Задание 1. Анализ и планирование

### 1. Описание функциональности монолитного приложения

**Управление отоплением:**

- Пользователи могут управлять отоплением в доме (удалённо включать/выключать отопление)
- Система поддерживает работу с модулем управления отоплением

**Мониторинг температуры:**

- Пользователи могут проверять температуру в доме
- Система поддерживает возможность предоставления о текущей температуре в доме через веб-интерфейс
- Система получает данные о температуре с датчиков, установленных в домах

### 2. Анализ архитектуры монолитного приложения

- Язык программирования: Java
- База данных: PostgreSQL
- Архитектура: Монолитная, все компоненты системы (обработка запросов, бизнес-логика, работа с данными) находятся в рамках одного приложения.
- Взаимодействие: Синхронное, запросы обрабатываются последовательно.
- Масштабируемость: Ограничена, так как монолит сложно масштабировать по частям.
- Развёртывание: Требует остановки всего приложения.
- Всё управление идёт от сервера к датчику. Данные о температуре также получаются через запрос от сервера к датчику.
- Каждая установка сопровождается выездом специалиста по подключению системы отопления в доме к текущей версии системы.

### 3. Определение доменов и границы контекстов

1. Домен: Умный дом (Smart Home)
Поддомены:
1.1. Управление отоплением (Heating Management):
- Управление температурой в помещениях.
- Взаимодействие с датчиками температуры и исполнительными механизмами (например, обогревателями).
1.2. Управление освещением (Lighting Management):
- Включение/выключение освещения.
- Настройка яркости, цветовой температуры.
1.3. Управление воротами (Gate Management):
- Открытие/закрытие автоматических ворот.
- Реагирование на внешние команды.
1.4. Управление видеонаблюдением (Video Surveillance Management):
- Управление камерами наблюдения.
- Потоковое видео, записи, обработка видео-потока.

2. Домен: Аналитика и отчеты (Analytics and Reporting)
Поддомены:
2.1. Обработка данных (Data Processing):
- Обработка данных, полученных с устройств, для создания аналитических отчетов.
2.2. Хранение данных (Data Storage):
- Использование OLAP-хранилища (например, Clickhouse или ScyllaDB) для долговременного хранения и анализа больших объемов данных.
2.3. Генерация отчетов (Report Generation):
- Формирование отчетов и аналитических выводов на основе собранных данных.

3. Домен: Взаимодействие с пользователями (User Interaction)
Поддомены:
3.1. Интерфейс для пользователей (User Interface):
- Веб-интерфейс для взаимодействия с системой (управление устройствами, просмотр отчетов и состояния).
3.2. Уведомления для пользователей (User Notifications):
- Отправка уведомлений о событиях в системе (например, изменение состояния устройства или системы).

4. Домен: Интеграция с устройствами (Device Integration)
Поддомены:
4.1. Датчики (Sensors):
- Все устройства, собирающие информацию о состоянии (температура, влажность и т.д.).
4.2. Устройства управления (Actuators):
- Устройства, такие как приводы для ворот, освещения и отопления, которые выполняют действия по запросу.
4.3. Видеонаблюдение (Surveillance):
- Видеокамеры, системы записи и потокового видео.

5. Домен: Асинхронная коммуникация (Asynchronous Communication)
Поддомены:
5.1. Межсервисная коммуникация (Inter-service Communication):
- Обработка событий и сообщений между сервисами через Kafka.
5.2. Публикация и подписка на события (Event Publishing and Subscribing):
- Отправка и получение событий в реальном времени для синхронизации между микросервисами.

6. Домен: Легаси-система (Legacy System)
Поддомены:
6.1. Бизнес-логика (Business Logic):
- Основная логика работы с пользователями, платежами и другими функциями, которые были реализованы в старой версии системы.
6.2. Интеграция с новыми сервисами (Integration with New Services):
- Логика и механизмы, которые обеспечивают плавный переход и взаимодействие между старой системой и новыми микросервисами через Антикоррупционный слой (ACL).

### **4. Проблемы монолитного решения**

Монолитная архитектура, используемая в текущем приложении для управления отоплением, имеет несколько значительных проблем, которые могут негативно сказаться на его развитии и эксплуатации:

1. Ограниченная масштабируемость
- Масштабируется всё приложение, а не отдельные компоненты.
- Неэффективное использование ресурсов и увеличение инфраструктурных затрат.

2. Сложности развертывания
- Обновления требуют остановки всей системы.
- Возможны простои, особенно критичные в отопительный сезон.

3. Затруднения в развитии
- Невозможность масштабировать команду разработки.
- Ограниченная независимая работа над новыми функциями.

4. Низкая гибкость
- Внедрение новых технологий и функций требует значительных усилий.
- Изменения в одной части системы могут нарушить работу других.

5. Проблемы производительности
- Синхронная обработка запросов увеличивает задержки.
- С ростом числа пользователей ухудшается пользовательский опыт.

6. Усложнённое тестирование и сопровождение
- Сложная взаимосвязь компонентов затрудняет внесение изменений.
- Медленный темп разработки и исправления ошибок.

7. Слабая интеграция
- Нет возможности легко интегрировать новые сервисы.
- Снижается ценность системы, например, для подключения пользовательских датчиков.


### 5. Визуализация контекста системы — диаграмма С4

[диаграмму контекста в модели C4](https://www.plantuml.com/plantuml/uml/lLFDRjD06BpxARO-fHAjBprnQecG2Yh2rE4SEV7IMl9FPAzfAuIK7wGYfQ8IGe830Y5U8AaQahQnymgxRyJiOkEWMcqvG0zrTh_vPhxvFDwCcHsnnFUq5OU-S0DAwXicsMUi4zytZCW-MDzpsNxIIc8QjSE0qO2jjqFVw7Xs8DlMOkPuRikeRwoPykhvant3jsD68st53TfUuj0ayYGf8CswP3XawnqbM5stCKJq2w6PD8h3e2R5xn6TV-KPln8dV8hd6H-9Ff_8iovht_b2TFc8d-7cb4z4d_0CW8ml-1LWAP_X_vpm6L50403KpQ6AQGuehYIl6A0qq5SrwmXM_EQ3C0aSEpgg7T1Mq-vqcgtyGYg_H-zuJR7Ee9Nn7uMUlafUQGH_ltr9Wh97z33mXUXB0xlKRZDZku7zrIrBB-nECMTMMbfKRk0T4ODb-p1mGa0-4xtSMSOlY3YMzd2S4ZwMO3HDvHLy_Zca7xPSZyeHFgDpp6TACyNl2Qdq2QZxr3As0YSANUSV3u_Q7DrT0D-WbcByr_cBgiNg8VM_BH6CbYIAD8ZJoe3eXUjK3WjO_mRV18yHUMo_j34KCSEOxKNKUhPJ1kVX8Ql5D8X3bTyFAbl7QBsiZjnGoNz1xLgOpt5CUIg6KZQhR9U1qxHTwkKVJ9wrJw9NhbuC9DqDMK37TgDXMg0gcSf0t4OsltDWbbNKfEMxgrp09GZcQWddOgY-uobE2YRUWXI9khQfGeTicOzogSBA_r4hLCeCVAoWkTlhBGKASaLwgvM_fTwaWPFutay0)

```
@startuml
!define C4P https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master
!includeurl C4P/C4_Context.puml
!includeurl C4P/C4_Container.puml

Person(user, "Пользователь", "Управляет отоплением и проверяет температуру")
System_Boundary(web_app_boundary, "Веб-приложение") {
    Container(web, "Веб-сайт", "React/HTML/CSS", "Позволяет пользователю управлять системой через браузер")
    Container(mobile, "Мобильное приложение", "iOS/Android", "Позволяет пользователю управлять системой через смартфон")
}
System(system, "Система управления отоплением", "Монолитное приложение на Java с PostgreSQL")
System_Ext(sensor, "Датчик температуры", "Отправляет данные о температуре")

Rel(user, web, "Использует через браузер", "HTTPS")
Rel(user, mobile, "Использует через мобильное приложение", "HTTPS")
Rel(web, system, "Отправляет команды и запрашивает данные", "HTTP API")
Rel(mobile, system, "Отправляет команды и запрашивает данные", "HTTP API")
Rel(system, sensor, "Запрашивает данные о температуре", "HTTP")
Rel(sensor, system, "Отправляет данные о температуре", "HTTP")
@enduml
```

# Задание 2. Проектирование микросервисной архитектуры

**Диаграмма контейнеров (Containers)**

[диаграмму контейнеров в модели C4](https://www.plantuml.com/plantuml/uml/p5TVRzjK57_FfxZhKoUIXg4zyKIN8fW88f31w-GaTuarnuvifoL2I4rA2LFHAgOz8SG428d7XZQh-wVELxZ-HfpVSMAysHTJ5H3RmutllUUSt-_yEVVq7SytN7_OjqgtElAHQKjHkziGFTyVU6zNgwwniz4r_TwmDVIastPiNzh-HjlfLmUMOKEiulaTosnLQtSh3Mnz-c6zsZSyNxgbMwRTjeOTENGjAAKh3skamI0ZxiO09alD4TtiFtptSt_TaxRdk6MnffwhesWIJLMWpaKqKNDrgHPg7ktozbwq6ns8DmHjXkeasbCNqRU2bWNErU_gWkvVgI0wN8Eg7qa-EOaEISj4g1Fwl59NqO6Q2nNcci198B3ItMCLaD4z5YR18vXBAOsUHay8wvc8nhGSuuQwMBkTvcOw_fRSDjjogzPnpMr9qEux3mHP6fEzNM9oXb-axqXK12iFw8C8QbdW_XBjc5cC8G0c-1doSKXtoHSg-7V8rCrkpo-WSa7ovDleu7z8f6tqfMi0r_UCSiwfnJTdfEAfEaddqit1buKw9Ua5QHnJqe9DcD9vJXne2TXiMA5xE0snNxuvxuPqFSTUHnc3p0iLed88K8XGeGRmpKIVCSm05GOigu2oJL1crRmtnntOcj8Hu9qI0rfc5HA_mWM2-bMi0dL7-79hDJTzI3VYJRTeMG0ltPec8h10kaCIkYG2NyF3204TxhyI5pKd-gkuttE6TiTmH-iUVuFkpwHoGSeFY0Zrdf1Yj_tMnfjhjyKN9K7_afwrRWpCzmnVxXWZajjit1VBB_RQBvda89VjSvCwv90m7Ku0y2HSpB0i2ESUNCKE2uWIzRsO41n7vpE443qBVeSiaeNz3eqWlnjzWvo0F5b8P87h_GSF6iJ_LI8n-S_kPEYVnFJl47sS_OG08YSvw402_LUqEwuGf13964CfvyGLJNIHcsU7CNFONUwzJE8jblwPmcLQ1Zrf-ARTRKeNdUXrhClAMRps6lm0j4ld9vTp4pAZGStrOBPQApXRBPZvJZtN86WudjzrPVFZUeQGXMQgfOE-GXjFRa0dhQ683eD9o6Ia2ybqgIuqN6W96VFasXmIoJXNzFZPVWf-8bW0UzliIASvTBUbQT4iqzPoIBgqzh6u3g_prUjJo8i19iGAS1IGnKlGS9skQ_HrD1VvZmHHEkSk2p9e0alabzTadf7_imVWBU3dw_Zl-2ND9V5EHdk1FnuRZnvZ6FW0gwXPGyompEWt5S2_gS4HtEFNZBmond6q3sCuvSQyd2Rd444dusc0_i-I0kJcpBOoIFgERVgEgszlU7NPXHCaDgCxLaVBZdq1gpZZnoOxXc0sIJ_WyMX6tkjBloU7Nbdae8ANKxWI9skW5BZQi0rhv9jjJyVlA1vRM6s0P8i7C3NZMCxuPJz0IcO6-YGk8LsPuXYvkqeTFRS4Ka9DW-8lQDcERpuosuPlEhQ66T8Zyye7-l5o3sqtVfNJTQDzLOk9fa4cNFDndHNmTInZqAI4CBhW-L5zio5G1kmt7jkGxCpjrzLkVhGqhgvPPljnpw7YhZRR8-h8M_ToKiatQuxh3WSWL3T6FDvkrkfizqLSXV4OnZb1imqrN1hmq6LGa9Xbk5gdoT0I3uyOTk1ak0OxsWQ4WcsPbjmS3EB-5wP5GH9XfGtwUBaoieIdxwyOuV-X8LgBUqUsbg2VyT2EcPH49_ybkFJV3d5za_WfjbAmlooLFlcZDJD6gHW8W_2HtXLiBAtU2GGVA4GRyG998mHixRIqLR4GyH9x8vsVZTd9_s-8Ew_S2D_EY_yiCvj3ztT5G3ayDewfnV669Lv_UYXuWFbE7cBhTGy3z0LdslH-fD5jG3tvoFZSNhTWsN7L8hcwNIwEDEchFP7-hzzRFU5tn8p-TxjtdcR2bcRbL_SuLxCqXaFC9DX7nO13jR9iPi-K8LwfFCAKB8SmN8z465SJ2SN5G275MLt6-bGi0ZdCwRmYExNWm8k3YkLVewK1LS9Y_B0IGye02se6L5CGhc_kSsGpNMzfJFsI11IrUz8MdfhCYbKnCHPKXx03T3N6QzH25jAb0DUef6v1NPPZDseCxAUB8sYBAZOnks-ww0pApG8JLdZzrn6rpR_-zotwrKlgvpl2JxeNfveDibMF-hbLFXJWqQJCZmImEyom5f5yVjXAmF6HWeK2gplN_gAWeyaidYbeFx7KRbkmu4RzL0jvdgUdlTXOqwUY9_v65QQ1V5TmTtkvsv_jTwVNvrVTB_laS7E1xxm_)

```
@startuml
!define C4P https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master
!includeurl C4P/C4_Container.puml

System_Ext(sensor, "Датчик температуры + модуль управления", "Отправляет данные о температуре и реагирует на управляющий сигнал")
System_Ext(iotDeviceDCdrive, "IoT устройства - привод", "Отправляет статус о своем состоянии и реагирует на управляющий сигнал")
System_Ext(iotDeviceLight, "IoT устройства - лампы", "Отправляет статус о своем состоянии и реагирует на управляющий сигнал")
System_Ext(camera, "Камеры наблюдения", "Отправляет статус о своем состоянии, запись и отправка видео-потока, и реагирует на управляющий сигнал")
Person(user, "Пользователь", "Взаимодействует с системой через веб-интерфейс для управления устройствами или получения информации о их статусе")

System_Boundary(system, "Теплый Дом v2.0") {
    Container(apiGateway, "API Gateway", "Управляет входящими запросами, аутентификацией, балансировкой и маршрутизацией", "HTTP")
    Container(systemV1, "Теплый дом v1.0", "Сервис с бизнес-логикой, управление пользователями, платежами и прочим", "HTTP")
    Container(heatingService, "Сервис отопления", "Микросервис управления отоплением", "HTTP")
    ContainerDb(heatingDb, "БД отопления", "PostgreSQL")
    Container(lightService, "Сервис освещения", "Микросервис управления освещением", "HTTP")
    ContainerDb(lightDb, "БД освещения", "PostgreSQL")
    Container(gateService, "Сервис ворот", "Микросервис управления воротами", "HTTP")
    ContainerDb(gateDb, "БД ворот", "PostgreSQL")
    Container(videoSurveillanceService, "Сервис видеонаблюдения", "Микросервис управления камерами", "HTTP")
    ContainerDb(videoDb, "БД видеонаблюдения", "PostgreSQL")
    Container(userProgramService, "Сервис пользовательских программ", "Микросервис управления программами и триггерами", "HTTP")
    ContainerDb(userProgramDb, "БД программ", "PostgreSQL")
    Container(kafka, "Kafka Cluster", "Шина данных для асинхронной передачи сообщений", "Kafka")
    Container(monitoringService, "Сервис мониторинга", "Следит за состоянием системы", "Prometheus, Kafka")
    ContainerDb(monitoringDb, "БД мониторинга", "PostgreSQL")
    Container(analyticsService, "Сервис аналитики", "Обрабатывает данные для отчетов", "HTTP, Kafka")
    ContainerDb(analyticsDb, "БД аналитики", "PostgreSQL")
    Container(notificationService, "Сервис нотификации", "Отправляет уведомления пользователям", "HTTP, Kafka")
    ContainerDb(notificationDb, "БД нотификаций", "PostgreSQL")
    Container(olapStorage, "OLAP хранилище", "Хранит данные для аналитики", "Clickhouse/ScyllaDB")
    Container(antiCorruptionLayer, "ACL", "Переход от v1 к v2 и обеспечение стабильности", "HTTP")
    Container(mobileApp, "Мобильное приложение", "Пользовательский интерфейс для управления устройствами", "HTTP")
    Container(webApp, "Веб-сайт", "Пользовательский интерфейс для управления устройствами через браузер", "HTTP")
}

Rel(sensor, apiGateway, "Отправляет телеметрию", "HTTP")
Rel(iotDeviceDCdrive, apiGateway, "Отправляет статус и команды", "HTTP")
Rel(iotDeviceLight, apiGateway, "Отправляет статус и команды", "HTTP")
Rel(camera, apiGateway, "Отправляет данные и реагирует на команды", "HTTP")
Rel(user, mobileApp, "Взаимодействует через мобильное приложение", "HTTP")
Rel(user, webApp, "Взаимодействует через веб-сайт", "HTTP")
Rel(mobileApp, apiGateway, "Запросы управления", "HTTP")
Rel(webApp, apiGateway, "Запросы управления", "HTTP")

Rel(apiGateway, systemV1, "Передача запросов", "HTTP")
Rel(apiGateway, heatingService, "Передача запросов", "HTTP")
Rel(apiGateway, lightService, "Передача запросов", "HTTP")
Rel(apiGateway, gateService, "Передача запросов", "HTTP")
Rel(apiGateway, videoSurveillanceService, "Передача запросов", "HTTP")
Rel(apiGateway, userProgramService, "Передача запросов", "HTTP")
Rel(apiGateway, monitoringService, "Передача запросов", "HTTP")
Rel(apiGateway, analyticsService, "Передача запросов", "HTTP")

Rel(notificationService, kafka, "Публикует уведомления", "Kafka")
Rel(kafka, notificationService, "Слушает события", "Kafka")

Rel(heatingService, kafka, "Отправляет события", "Kafka")
Rel(kafka, heatingService, "Слушает события", "Kafka")
Rel(lightService, kafka, "Отправляет события", "Kafka")
Rel(kafka, lightService, "Слушает события", "Kafka")
Rel(gateService, kafka, "Отправляет события", "Kafka")
Rel(kafka, gateService, "Слушает события", "Kafka")
Rel(videoSurveillanceService, kafka, "Отправляет события", "Kafka")
Rel(kafka, videoSurveillanceService, "Слушает события", "Kafka")
Rel(monitoringService, kafka, "Отправляет события", "Kafka")
Rel(kafka, monitoringService, "Слушает события", "Kafka")
Rel(analyticsService, kafka, "Получает данные", "Kafka")
Rel(userProgramService, kafka, "Отправляет события", "Kafka")
Rel(kafka, userProgramService, "Слушает события", "Kafka")

Rel(analyticsService, analyticsDb, "Чтение и запись данных", "SQL")
Rel(notificationService, notificationDb, "Чтение и запись данных", "SQL")
Rel(monitoringService, monitoringDb, "Чтение и запись данных", "SQL")
Rel(heatingService, heatingDb, "Чтение и запись данных", "SQL")
Rel(lightService, lightDb, "Чтение и запись данных", "SQL")
Rel(gateService, gateDb, "Чтение и запись данных", "SQL")
Rel(videoSurveillanceService, videoDb, "Чтение и запись данных", "SQL")
Rel(userProgramService, userProgramDb, "Чтение и запись данных", "SQL")
Rel(analyticsService, olapStorage, "Чтение и запись данных", "SQL")

Rel(antiCorruptionLayer, systemV1, "Передача запросов", "HTTP")
Rel(antiCorruptionLayer, heatingService, "Передача запросов", "HTTP")
Rel(notificationService, user, "Отправка уведомлений", "Push/SMS/Email")
@enduml
```

**Диаграмма компонентов (Components)**

[сильно упрощенная диаграмма компонентов в модели C4](https://www.plantuml.com/plantuml/uml/hLPRJzj867tthnZoiY01MQrusYUM8B6h1Ia5zHaDYKai73lo1OGg999Ue0ebAkfJLO7QgkehkKIO9ax-mim_wfalCH3PfK60X1D_bpDdpZV7pgBhSSVpIwOsbTUt3KjdcOKiAtfUsVqxdNRutdp1y8h-bk_gJiwsFDtovdDsALqskQNQvbmlRnfRwSp2N5Q5dg-jfalSzNH7cpAidEddTTyn5IXADZDsgMnR2gEibjGoGEHOrDbSidqhpvtzQNSVpQLPbX9VH4SCH5VMn3KJ7qHVzDZkN_D_fcRO2utXxmPicfUD5UxfUtmVVVza_sEZknHkDygEOHNOacrxwbPy4fVoK1xZ_wMiYa1SuVwD26ITgQzY80z5NrQmMdEM8OKYq0XLgNo5yXzHER8Dr7Vn7w8-G1zwL9w94C6MMa5S8H2aPeWlNH9sNDIv1ufhTjutTQ88f3QQk_A4uKDHkWHqLn4Xfc1OHSqW2k7Qkv5bJ9e8V4DtTeqSeNyT0wJkP40KhzYasNkb4-8TMPaJNHIs4A2Tja6o_X2IlwMup3s-nLr5zTzXWeqoB4efJbdRzGgEll5ibHX-e-N3smsrwJQKDVaQJ4GZJYUOW4o6vuf9J5ISQAphUSCb1X_bEtby7sHcRLaDtx5DKtVkO2BERXDtL3yN3TeLZfcimWLDVE_8AeFoy0BjhorhJoM-vTgE4ZYHup1D0tWWrq24y4qVoTx8EQT43XEYqYCwIN1EZBAi9nBUuTix74l-Zq-GCNtN0vCe4971QW1XPEG-kBsDC9681d-W7MZQ7qny5at4hqJ0G3jKZWVRW8tPKblNpR77JDnNapzOC4467f79JnyqGX29cVgN3XBVPJNgJWzf0BiYJyOCCWagMU7HaD7mn3b0eYsh1ma9qq2dKkuQUwS8OAkureT-LQ60Gb12rkvb7NERE9Cri8NwO_vvEkaJ_ICk0DF5WJXAVZ9DxWGwgMUaTp0yEyXTanfDmfKL0D4xfqKLGRJF0QxXy1s6vbvCnuIZdQpWXUX0gOwY2xANOpg2O0pcCSgCyDIlZgnZ5nnUkd_0yICruPWPwOCtxV1mg4aWZtjP0K2WPaJE5Irgxz51MDIjlFhzyXC0)

```
@startuml
!define C4P https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master
!includeurl C4P/C4_Component.puml

Container_Boundary(system, "Теплый Дом v2.0") {
    Component(apiGateway, "API Gateway", "Spring Boot", "Маршрутизация запросов, аутентификация, балансировка нагрузки")
    
    Container_Boundary(heatingModule, "Модуль управления отоплением") {
        Component(heatingService, "Управление отоплением", "Go", "Бизнес-логика для управления отоплением")
        Component(heatingServiceDatabase, "Heating Service Database", "PostgreSQL", "Хранение данных об отоплении")
        Component(heatingServiceCache, "Heating Service Cache", "Redis", "Кэш")
        Component(heatingCMDController, "Heating CMD Controller", "Go", "Обрабатывает команды управления отоплением")
        Component(heatingSensorDataController, "Heating Sensor Data Controller", "Go", "Обрабатывает информацию от датчиков отопления")
        Component(kafka, "Kafka Cluster", "Kafka", "Обмен сообщениями")
    }
}

' Связи внутри heatingModule
Rel(apiGateway, heatingService, "Маршрутизация запросов к модулю управления отоплением")
Rel(heatingService, heatingServiceDatabase, "Чтение/запись данных")
Rel(heatingService, heatingServiceCache, "Чтение/запись данных")
Rel(heatingService, kafka, "Отправка событий о состоянии системы")
Rel(heatingService, heatingCMDController, "Вызов команд управления отоплением")
Rel(heatingService, heatingSensorDataController, "Получение данных от датчиков отопления")

' Взаимодействие с другими компонентами системы
Rel(apiGateway, heatingCMDController, "Передача команд")
Rel(apiGateway, heatingSensorDataController, "Получение данных")
Rel(userProgramService, heatingCMDController, "Отправка команд управления от пользовательских программ")
@enduml
```

**Диаграмма кода (Code)**

[упрощенная диаграмма кода (последовательности) в модели C4 -  отправка данных температуры сенсором и их обработка](https://www.plantuml.com/plantuml/uml/TLBBIiD05DtFLroozmTSI7s0YXiX_i0GHmrgMjkfuguFr48L4LoAq3yOYiLGJV8Bz_wH9ucXfDP5ClSnvznpJzBeMDsstSiBfGFRR9DleWuEtI5VjT09Wx2b8qlUvk4-xMfhRlIjbxUHro_mX6VIuod7qYSPSeR48VtY6ISeFXEdCk2Kiwg4ztV1jMUq3QDJtxIlmy3KQBTrS5Qlx6ofgwxf6ZhG9-SQy38uQhR2G2cVrZPoRUl4xvXMYOXb88_47qWIJcJCCq8NypahL3lqXzAJiG8M54kImFR63lqHdY6GCNyYlo_NGbWCIgNFJjMHu2ft0CK4D-uAGKbb0C08d3miJ9eIp-L9Hd-9iFf_B02zyEiAoCAR9KVcx7BOxnU9UKuUpZIl5mkLQbQRvAc6YOxnq_m0)

```
@startuml
actor Sensor as S
participant "API Gateway" as API
participant "Сервис управления отоплением" as HeatingService
participant Cache
participant DB
queue Kafka as KafkaTopic

S -> API: Отправить данные температуры
API -> HeatingService: Маршрутизация запроса
HeatingService -> Cache: Сохранение значения в кэш
HeatingService -> DB: Сохранение в БД
HeatingService -> KafkaTopic: Отправка в топик "показания температуры"
@enduml
```

[упрощенная диаграмма (последовательности) в модели C4 - обработка события сенсора температуры модулем пользовательских программ](https://www.plantuml.com/plantuml/uml/hLJBojf05DxFKmnPjT2-W8iYT5Ce1Udb0O9E9TIBZKdNNXPjKHIwBYrz0uq6ez7u2kUyKR_3Q9kHBEJdHynavinybsyEgGyZFupYRaSDC0RDOE3t8lrXQC96s6-7VlUT2Ry4JQFzONbLVOvDRFHh_-Cd7oNxyhu_29eEZaT_AEKH9PJnMFEKpfYiCCqffHCMbveAdb11v-iIdaTF85yPFmfwxGl_3UnF7Da2B-Vv_R2Q4arRh1ufBLEywRVYZb6gv4y_Vr7oLDFqMmW11nzqe9Mc4nwM0k07xM4if48x_Aq6bC6AGYhoUC9J7l7CK_-1XQR4Uk79iV7hLRO6OO7Vch__C_T4sCKqPS07-hDABf69JIWaFEDl677iwQrDD-UQRhzjlDHoWdQWJMesocl26Q3To7d92sZBQ0-f63R2GZac6db8s9A7nBvNBXVOtIi0rwzRBWU4fvMthHoKAbFq0o0xIcmuq0OQczAOLjRjMP87deFfQ2q5zpY8Jeivrj58S8gNr3QiqzcqleUkwf9Isb21muB1Cq1WdziF3zDakNbkMqQ4KOhvmegj579qP52cwgRNmdVeBm00)

```
@startuml
participant UserProgramService as UserProgram
queue Kafka as KafkaTopic
participant "Сервис управления отоплением" as HeatingService
participant "Модуль управления отоплением в доме" as HeatingModule

UserProgram -> KafkaTopic: Подписка на топик "показания температуры"
KafkaTopic -> UserProgram: Получение сообщения (например, температура превышена)
UserProgram -> HeatingService: gRPC вызов: отправить управляющий сигнал с командой
HeatingService -> HeatingModule: Отправить команду управления
HeatingModule -> HeatingService: Возврат статуса выполнения
HeatingService -> UserProgram: Возврат статуса выполнения
HeatingService -> KafkaTopic: Отправка уведомления об статуса выполнения управляющего события
UserProgram -> KafkaTopic: Отправка уведомления пользователю об управляющем событии
@enduml
```

[упрощенная диаграмма (последовательности) в модели C4 - просмотр пользователем информации об отоплениии](https://www.plantuml.com/plantuml/uml/dPFFJl9G4CNtzoachBvlmGkuCAW9wix4z06kzQfDj99obRX3Y7-C1XCtRep6DsWQ6uX0UOMPD_9ScagAii05X9cU-SsPGrfhMXtPivDZfNrx6f7ND17f9dcgWNxW1mqTMggixMpJfhQcfIygxZ7gic2zNYUvv5JQdF00lsAIBazGncWCEUKnse_4cNGuuqdHmlTeIWIWJEJt6Mr9rfRHDWWFTqrq7lYcXDtG_f5HogFggZjYXVWPLCVEi8O_K8nvBFToHG1dFtP8Js4CqyumfbyubhRSujE5SC1zrDUvk_uCpB6275Dprlwolam0Cxy9g3SeX_1nKyH1m7rA1-75UF2GWqjRb9i5v3TdJy3rEN6nozqIo7c3trczufQ4K-bGwLOUyM3HjNbinN-ro59MW94pXDC1Rx5yFcLt27GP634OAUeU_UE4McQl6eJMH0VQD_zLpOJxU5E-0000)

```
@startuml
actor User as U
participant "API Gateway" as API
participant "BFF сервиса управления отоплением" as BFF
participant "Сервис управления отоплением" as HeatingService
participant Cache

U -> API: Запрос данных по отоплению
API -> BFF: Маршрутизация запроса
BFF -> HeatingService: Запрос данных по отоплению
HeatingService -> Cache: Получение актуальных данных
Cache --> HeatingService: Возвращение актуальных данных
HeatingService --> BFF: Ответ с данными по отоплению
BFF --> API: Возвращение ответа
API --> U: Возвращение данных пользователю (в web-interface)
@enduml
```

# Задание 3. Разработка ER-диаграммы

[ER-диаграмма](https://www.plantuml.com/plantuml/uml/jLCnRiCm3Dpz2i5Z0N-WKuOCxT2foHom5fi8i2H3f5iOIVzUcHmS2Huwf1jvTpWT2Mf738adFpGD1dOyc_P8c5e3P9R2N1jZdeopvjaSZwzxwG9upFqx9nVEWg07DvJG24JVCTbxjEk4wXC2epq1PtAVouFpACmqsWtcPmtt4YT2IIVLPhoCfrC9WRK9YOg4EiP3q-tno_KQJylDV3oFMEMxv65gZ30v64vXyX-OokJ4m1FzH_R3F6h-jVtZ2LYaFhcHNGmhSUV_5kqqgBUM3_8zu80UESrvA13x0jbynSfmrRxPUIG6FzQhiy1GF2qVutRhkdZLCyniUm5g6-sKqXKAryV0u48MAAQAsxfaKJaoyLEkezBfmaIJxIkNQpLl5fqnO09UbCqEWrs-zoy0)

```
@startuml
entity "User" as User {
  * id : UUID
  * name : String
  * email : String
  * password : String
  --
  * created_at : DateTime
  * updated_at : DateTime
}

entity "House" as House {
  * id : UUID
  * user_id : UUID
  * address : String
  * name : String
  --
  * created_at : DateTime
  * updated_at : DateTime
}

entity "Device" as Device {
  * id : UUID
  * type_id : UUID
  * house_id : UUID
  * serial_number : String
  * status : String
  --
  * created_at : DateTime
  * updated_at : DateTime
}

entity "DeviceType" as DeviceType {
  * id : UUID
  * name : String
  * description : String
}

entity "Module" as Module {
  * id : UUID
  * name : String
  * description : String
}

entity "TelemetryData" as TelemetryData {
  * id : UUID
  * device_id : UUID
  * timestamp : DateTime
  * data : String
}

User ||--o{ House : "has"
House ||--o{ Device : "contains"
Device ||--o| DeviceType : "is of type"
Device ||--o| Module : "uses"
Device ||--o{ TelemetryData : "generates"
@enduml
```