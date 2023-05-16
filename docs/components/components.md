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



System_Boundary(helloconf, "helloconf") {


Container(app, "Веб-приложение", "html, JavaScript, react", "Приложение проведения онлайн конференций")
System_Boundary(reg, "Registration") {
   Container(registration_service, "Registration Service", "JavaScript, nodeJS", "Сервис регистрации", $tags = "microService")   
   Container(upload_service, "Upload Service", "JavaScript, nodeJS", "Сервис загрузки материалов", $tags = "microService") 
   ContainerDb(user_db, "user Catalog", "postgresql", "Хранение пользователей", $tags = "storage") 
   ContainerDb(material_db, "material Catalog", "mongoDB", "Хранение материалов", $tags = "storage")
}

Rel(customer1, app, "регистрация, загрузка доклада", "HTTPS")
Rel(app, registration_service, "Регистрация пользователя:user", "JSON, HTTPS")
Rel(app, upload_service, "Загрузка материалов:materials", "base64, HTTPS")
Rel(registration_service, user_db, "Сохранение пользователя", "SQL")
Rel(upload_service, material_db, "Сохранение материалов", "NOSQL")



System_Boundary(mod, "Moderation") {
   Container(moderatin_service, "Moderation Service", "JavaScript, nodeJS", "Сервис модерации", $tags = "microService") 
   System_Ext(moderation_system1, "Антиплагиат", "Система проверки на плагиат.")  
   System_Ext(moderation_system2, "bad words", "Система проверки на запрещенные слова.")  
   Container(moderation_system3, "Structure Service", "JavaScript, nodeJS", "Сервис проверки структуры доклада", $tags = "microService") 
}

Rel(customer3, app, "регистрация, проверка материалов", "HTTPS")
Rel(app, moderatin_service, "Модерация материалов:materials", "HTTPS")
Rel(moderatin_service, moderation_system1, "Модерация материалов:materials", "base64, HTTPS")
Rel(moderatin_service, moderation_system2, "Модерация материалов:materials", "base64, HTTPS")
Rel(moderatin_service, moderation_system3, "Модерация материалов:materials", "base64, HTTPS")



System_Boundary(conf, "Conference") {
   Container(conference_service, "Conference Service", "JavaScript, nodeJS", "Сервис конференции", $tags = "microService") 
   Container(schedule_service, "Schedule Service", "JavaScript, nodeJS", "Сервис составления расписания", $tags = "microService") 
   System_Ext(streamig_service, "stream", "Система стримингово сервиса.")  
   System_Ext(email_service, "email", "Система отправки уведомлений.") 
}

}
   
Rel(customer4, app, "Составление расписания, Организация конференции", "HTTPS")
Rel(app, schedule_service, "Составление расписания:users", "JSON, HTTPS")
Rel(app, conference_service, "Организация конференции:users", "HTTPS")
Rel(conference_service, streamig_service, "онлайн трансляция")
Rel(conference_service, email_service, "отправка уведомления")


Rel(customer2, app, "регистрация, просмотр конференции", "HTTPS")


Rel(app, user_db, "Получение пользователей", "SQL")
Rel(app, material_db, "Получение материалов материалов:materials", "NOSQL")

SHOW_LEGEND()
@enduml
```

## Список компонентов
| Компонент             | Роль/назначение                  |
|:----------------------|:---------------------------------|
| *Название компонента* | *Описание назначения компонента* |