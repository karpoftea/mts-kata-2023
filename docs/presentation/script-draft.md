```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

title Диаграмма контейнеров: Course Manager and Exam Manager

System_Boundary(sys, "Система Поколение-М") {
    Container(cm, "CourseManager", "Java, SpringBoot", "Предоставляет API для создания/управления курсами/уроками и тп, а также записи на них")
    Container(em, "ExamManager", "Java, SpringBoot", "Предоставляет API для создания/управления тестами/домашними заданиями/дипломами. Реализует оценочную систему")
    ContainerDb(dbms, "Реляционная СУБД", "PostgreSQL", "Хранит информацию о пользователях, курсах и тп")
}

System_Ext(webinar, "Webinar", "Система проведения онлайн мероприятий (встреч, уроков, конференций и тп)")


Rel(cm, dbms, "Управляет данными курсов/уроков в", "tcp/jdbc")
Rel(cm, webinar, "Создает комнаты для воркшопов в", "http/rest")

Rel(em, dbms, "Управляет данными тестов/дз/дипломов/оценок в", "tcp/jdbc")

@enduml
```


```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

title Диаграмма контейнеров: API Gateway

Person(guest, "Гость", "Потенциальный пользователь системы Поколение-М")
Person(student, "Студент", "Проходит обучение используя ресурсы и возможности системы")
Person(teacher, "Преподаватель", "Осуществляет преподавательскую деятельность с помощью системы")
Person(support, "СотрудникПоддержки", "Обеспечивает функционирование, техническую поддержку системы")
Person(admin, "Администратор", "Управляет пользователями, учебными материалами, мониторинг вовлеченности аудитории")

System_Boundary(sys, "Система Поколение-М") {
    Container(userwebapp, "Образовательная Платформа", "JavaScript, ReactJS, SPA", "Адаптивный веб-интерфейс для обучающихся")
    Container(adminwebapp, "Панель Управления", "JavaScript, ReactJS, SPA", "Веб-интерфейс управления контентом образовательной платформы для персонала")
    Container(gw, "API Gateway", "Java, SpringBoot/Cloud", "Аутентифицирует и хранит профиль пользователей; Марштутизирует запросы в нижележащие сервисы")
    Container(cm, "CourseManager", "Java, SpringBoot", "Предоставляет API для создания/управления курсами/уроками и тп, а также записи на них")
    Container(em, "ExamManager", "Java, SpringBoot", "Предоставляет API для создания/управления тестами/домашними заданиями/дипломами. Реализует оценочную систему")
    ContainerDb(dbms, "Реляционная СУБД", "PostgreSQL", "Хранит информацию о пользователях, курсах и тп")
}

System_Ext(emailsys, "Система отправки эл.почты", "Коммунальный сервер МТС для рассылок электронной почты")
System_Ext(mtsid, "МТС ID", "Система идентификации пользователей")

Rel(admin, adminwebapp, "Управляет правами, создает курсы/уроки в")

Rel(guest, userwebapp, "Просматривает доступные курсы, регистрируется в")
Rel(student, userwebapp, "Проходит обучение по курсам, сдает дом.работы, оценивает курс в")
Rel(teacher, adminwebapp, "Проверяет домашние работы или задания по конкурсам в")
Rel(support, adminwebapp, "Управляет учетными записями пользователей, назначением курсов, формированием образовательной программы в")

Rel(userwebapp, gw, "Использует API", "http/rest")
Rel(adminwebapp, gw, "Использует API", "http/rest")

Rel(gw, dbms, "Управляет данными пользователей в", "tcp/jdbc")
Rel(gw, emailsys, "Отправляет уведомление о регистрации в", "smtp")
Rel(gw, mtsid, "Аутентифицирует пользователей в")
Rel(gw, cm, "Вызывает API", "http/rest")
Rel(gw, em, "Вызывает API", "http/rest")


@enduml
```


```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

title Диаграмма контейнеров: Migration Tool

Person(admin, "Администратор", "Управляет пользователями, учебными материалами, мониторинг вовлеченности аудитории")

System_Boundary(sys, "Система Поколение-М") {
    Container(gw, "API Gateway", "Java, SpringBoot/Cloud", "Аутентифицирует и хранит профиль пользователей; Марштутизирует запросы в нижележащие сервисы")
    Container(cm, "CourseManager", "Java, SpringBoot", "Предоставляет API для создания/управления курсами/уроками и тп, а также записи на них")
    Container(fm, "FileManager", "Java, SpringBoot", "Предоставляет API для создания/управления файлами пользователей и учебными материалами")
    Container(em, "ExamManager", "Java, SpringBoot", "Предоставляет API для создания/управления тестами/домашними заданиями/дипломами. Реализует оценочную систему")
    Container(mt, "MigrationTool", "Java, SpringBoot", "Консольное приложение мигрирующее данные пользователей их прогресс, и материалы (видео, текст) в систему")
}

System_Ext(legacy, "Старая система Поколение-М", "Информационный портал с неструктурированными материалами")

Rel(admin, mt, "Запускает миграцию через")

Rel(gw, cm, "Вызывает API", "http/rest")
Rel(gw, fm, "Вызывает API", "http/rest")
Rel(gw, em, "Вызывает API", "http/rest")


Rel(mt, legacy, "Загружает информацию о пользователях/курсах/уроках", "tcp/jdbc+http")
Rel(mt, gw, "Сохраняет информацию о пользователях/курсах/уроках/домашних заданиях/учебных материалах", "http")

@enduml
```