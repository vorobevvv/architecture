# Компонентная архитектура
<!-- Состав и взаимосвязи компонентов системы между собой и внешними системами с указанием протоколов, ключевые технологии, используемые для реализации компонентов.
Диаграмма контейнеров C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783368
-->
## Контейнерная диаграмма

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(customer1, "Докладчик")
Person(customer2, "Слушатель")
Person(customer3, "Модератор")
Person(customer4, "Организатор")

System_Boundary(c, "MTS conference") {
   Container(app, "веб-приложение", "html, JavaScript, react", "Приложение проведения онлайн конференций")
   Container(registration_service, "Registration Service", "JavaScript, nodeJS", "Сервис регистрации и загрузки материалов", $tags = "microService")   
   Container(moderatin_service, "Moderation Service", "JavaScript, nodeJS", "Сервис модерации", $tags = "microService") 
   Container(schedule_service, "Schedule Service", "JavaScript, nodeJS", "Сервис составления расписания", $tags = "microService") 
   Container(conference_service, "Conference Service", "JavaScript, nodeJS", "Сервис конференции", $tags = "microService") 

   ContainerDb(user_db, "user Catalog", "mongoDB", "Хранение пользователей", $tags = "storage") 
   ContainerDb(material_db, "material Catalog", "mongoDB", "Хранение материалов", $tags = "storage") 
}

System_Ext(moderation_system1, "Антиплагиат", "Система проверки на плагиат.")  
System_Ext(moderation_system2, "bad words", "Система проверки на запрещенные слова.")  
System_Ext(streamig_service, "stream", "Система стримингово сервиса.")  

Rel(customer1, app, "регистрация, загрузка доклада", "HTTPS")
Rel(app, registration_service, "Регистрация пользователя:user", "JSON, HTTPS")
Rel(registration_service, user_db, "Сохранение пользователя", "NOSQL")
Rel(registration_service, material_db, "Сохранение материалов", "NOSQL")

Rel(customer3, app, "регистрация, проверка материалов", "HTTPS")
Rel(app, moderatin_service, "Модерация материалов:materials", "JSON, HTTPS")
Rel(moderatin_service, moderation_system1, "Модерация материалов:materials", "JSON, HTTPS")
Rel(moderatin_service, moderation_system2, "Модерация материалов:materials", "JSON, HTTPS")
Rel(moderatin_service, material_db, "Получение материалов докладчиков", "NOSQL")
Rel(moderatin_service, user_db, "Получение докладчиков", "NOSQL")

Rel(customer4, app, "регистрация, составление расписания", "HTTPS")
Rel(app, schedule_service, "Составление расписания:users", "JSON, HTTPS")
Rel(schedule_service, user_db, "Получение докладчиков", "NOSQL")

Rel(customer2, app, "регистрация, просмотр конференции", "HTTPS")
Rel(app, conference_service, "онлайн трансляция", "JSON, HTTPS")
Rel(conference_service, streamig_service, "онлайн трансляция", "JSON, HTTPS")
Rel(conference_service, user_db, "Получение докладчиков", "NOSQL")
Rel(conference_service, material_db, "Получение материалов", "NOSQL")


SHOW_LEGEND()
@enduml
```

## Список компонентов
| Компонент             | Роль/назначение                  |
|:----------------------|:---------------------------------|
| *Название компонента* | *Описание назначения компонента* |