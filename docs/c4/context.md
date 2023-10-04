# Диаграмма контекста
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

LAYOUT_WITH_LEGEND()

title Диаграмма контекста системы Поколение-М

Person(guest, "Гость", "Потенциальный пользователь системы Поколение-М")
Person(student, "Студент", "Проходит обучение используя ресурсы и возможности системы")
Person(teacher, "Преподаватель", "Использует систему для осуществления преподавательской деятельности")
Person(support, "СотрудникПоддержки", "Обеспечивает функционирование, техническую поддержку системы")
Person(admin, "Администратор", "Управляет правами пользователей, размещением и доступностью материалов, подтверждать корректность загруженных материалов")

System(sys, "Система Поколение-М", "Предоставляет образовательные программы и курсы для детей и подростков")

System_Ext(emailsys, "Система эл.почты", "Коммунальный сервер МТС для рассылок электронной почты")
System_Ext(mtsa, "МТС Аналитика", "Система веб-, мобильной-, рекламной аналитики")
System_Ext(webinar, "Webinar", "Система проведения онлайн мероприятий (встреч, уроков, конференций и тп)")

Rel(guest, sys, "Просматривает доступные курсы, регистрируется")
Rel(student, sys, "Проходит обучение по курсам, сдает дом.работы, оценивает курс")
Rel(teacher, sys, "Формирует образовательную программу, проверяет дом.работу")
Rel(support, sys, "Управляет учетными записями пользователей, назначением курсов, формированием образовательной программы")
Rel(admin, sys, "Управляет учетными записями пользователей, назначением курсов, корректностью образовательной программы")

Rel(sys, emailsys, "Рассылает уведомления пользователям")
Rel(sys, mtsa, "Журналирует события пользователей на сайте и мобильном приложении")
Rel(sys, webinar, "Создает трансляции онлайн активностей: воркшопов, уроков, маркетинговых событий и тп")

@enduml
```