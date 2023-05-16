# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()



System_Boundary(helloconf, "Helloconf") {
    System_Boundary(a, "Registration") {
        System(reg, "Регистрация", "Регистрация всех пользователей")
        System(upload, "Загрузка материалов", "Загрузка материалов докладчиков")
    }

    System_Boundary(b, "Moderation") {
        System(es0, "Подтверждение материалов", "Система подтверждения материалов")
        System(es3, "Следование структуры", "Система проверки струтуры доклада")
    }

    System_Boundary(c, "Conference") {
        System(conf, "Конференция", "Система проведения конференций")
        System_Ext(stream, "Online", "Стриминоговый сервис")
        System_Ext(es1, "E-mail", "Система уведомлений на почту")
        System(sch, "Расписание", "Система составления расписания")
    }
}



Rel(es0, es3, "Проверка материалов")


Rel(conf, stream, "Онлайн трансляция")
Rel(conf, es1, "Отправка уведомлений")
Rel(sch, conf, "")


Rel(a, b, "Получение материалов")
Rel_R(a, c, "Докладчики и материалы")


@enduml
```
