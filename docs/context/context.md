# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()


Person(customer3, "Модератор")

Person(customer1, "Докладчик")

Person(customer2, "Слушатель")
Person(customer4, "Организатор")


System_Boundary(a, "Registration") {
    System(reg, "Регистрация", "Регистрация всех пользователей")
    System(upload, "Загрузка материалов", "Загрузка материалов докладчиков")
}

System_Boundary(b, "Moderation") {
    System(es0, "Подтверждение материалов", "Система подтверждения материалов")
    System_Ext(es2, "Антиплагиат", "Система проверки на плагиат")
    System_Ext(es4, "Bad words", "Система проверки на плохие слова")
    System(es3, "Следование структуры", "Система проверки струтуры доклада")

}

System_Boundary(c, "Conference") {
    System(conf, "Конференция", "Система проведения конференций")
    System_Ext(stream, "Online", "Стриминоговый сервис")
    System_Ext(es1, "E-mail", "Система уведомлений на почту")
    System(sch, "Расписание", "Система составления расписания")
}

Rel(customer1, reg, "Регистрация")
Rel(customer1, upload, "Загрузка материалов")
Rel(customer2, reg, "Регистрация")
Rel(customer3, reg, "Регистрация")

Rel(customer3, es0, "Валидация материалов")
Rel(es0, es2, "Проверка материалов")
Rel(es0, es4, "Проверка материалов")
Rel(es0, es3, "Проверка материалов")

Rel(customer4, sch, "Составление расписания")
Rel(es1, customer2, "Отправка уведомлений/писем с обратной связью")
Rel(customer4, conf, "Организация конференции")

Rel(conf, stream, "Онлайн трансляция")
Rel(conf, es1, "Отправка уведомлений")


@enduml
```
