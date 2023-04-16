# Модель предметной области
<!-- Логическая модель, содержащая бизнес-сущности предметной области, атрибуты и связи между ними. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375782602

Используется диаграмма классов UML. Документация: https://plantuml.com/class-diagram 
-->

```plantuml
@startuml
' Логическая модель данных в варианте UML Class Diagram (альтернатива ER-диаграмме).

namespace Registration {
 class Speaker
 {
  id : string
  fio: string
  state: stirng
  moderation: boolean
  material: File[]
 }

 class User
 {
  id : string
  fio: string
 }

 Registration.Speaker ..> Moderation.File : ref
}

namespace Moderation {
class File
 {
  id : string
  createDate : datetime
  updateDate : datetime
  title: string
  version: int
  moderation1: boolean
  moderation2: boolean
  moderation3: boolean
  moderatorId: Moderator
 }

  class Moderator
 {
  id : string
  fio: string
 }

 Moderator *-- File
 
}

namespace Schedule {
class Schedule
 {
    id : string
    date: datetime
    sequence: Sequence[]
 }

 class Sequence 
 {
    id: string
    title: string
    speakerId: Speaker
    time: datetime
 }

 Schedule *-- Sequence
 Schedule.Sequence ..> Registration.Speaker : ref
}

namespace Conference {
 class Conference 
 {
    id: string
    title: string
    schedule: Schedule
    users: User[]
 }

 Conference.Conference ..> Schedule.Schedule : ref
 Conference.Conference ..> Registration.User : ref
}

@enduml
```