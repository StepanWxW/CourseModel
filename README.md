### Задание: 
Тестовое задание для кандидатов

1. Создать клон данного репозитория в своем профиле github. Исправить файл readme. Указать ФИО автора, контактную информацию.
2. Разработать схему отношений на на основании модели, представленной в courses.gif
3. Разработать микросервис, реализующий REST-API с CRUD-сервисами для этой модели
4. Реализовать сервис, позволяющий построить отчет по загрузке препадавательского состава. Поля отчета: ФИО профессора, Суммарное количество студентов по всем курсам, Средння успеваемость студентов по всем курсам. Отчет сформировать в формате xlsx.
5. Разработать web-интерфейс для редактирования списка профессоров. (Дополнительное задание)
6. Прислать merge request

### Решение

<br>Выполнил: Поляков Степан Сергеевич.
<br>почта: polyakovs@list.ru
<br>Flyway сразу наполняет базу данных.
<br>Для понимания отношений сущностей:
<br>Смотреть диаграмму отношений Dia.jpg

- Отношение "Профессор"(Professor) - "Курс"(Course)
<br>Один профессор может вести несколько курсов или вообще не одного. Курс может иметь только одного профессора или вообще не иметь.
<br>oneToMany
***
- Отношения "Курс" - "Прохождение курса" (CourseStage)
<br> Каждая запись в таблице является одной оценкой по определенному курсу, определенного студента.
<br> oneToMany
***
- Отношения "Студент" (Student) - "Прохождение курса" 
<br> oneToMany
***
- Для решения задачи нам понадобиться еще одна сущность! (CourseStage)
<br> Она будет хранить в себе информацию о записанном студенте на определенный курс, а также информацию о том завершен или нет курс.
<br> Студент записывается на курс по номеру зачетки, т.к. номер зачетки является уникальным.
<br> Один студент не может быть записан два раза на один и тот же курс или уже на пройденный курс.

***
Согласно заданию реализован RESP API для получения информации.

- Записать студента на курс 
<br> Для этого по post запросу /student нужно в параметрах запроса отправить два значения:
<br> Номер зачетки студента и номер курса. Пример:
<br> /course?bookGrade=3339&numberCourse=4458
- Добавить студента на курс 
<br> Работает аналогично
<br> Пример:
<br> /student/addToCourse?bookGrade=3339&numberCourse=4458
- Удалить студента с курса
<br> delete запрос на /course с параметрами
<br>Пример:
<br> /course/addToCourse?bookGrade=5&numberCourse=777
- Получить список прослушанных курсов: (возврат в формате JSON)
<br> post запрос на /student/courseFinished/{gradeBook} 
<br> вместо gradeBook нужно указать номер зачетки студента. Номер зачетки можно узнать по запросу /student (получив всех студентов)
<br> Пример:
<br> /student/courseFinished/1236


- Получить текущий средний бал: (возврат в формате JSON)
<br> post запрос на адрес /coursestage/getGrade с параметрами
<br> Указать Id студента и Id курса
<br> Пример
<br> /coursestage/getGrade?studentId=1&courseId=3
<br> В ответ прейдет JSON файл с тремя параметрами: средняя оценка и id курса и id студента.

- Получить финальную отметку: (возврат в формате JSON)
<br> post запрос на адрес /coursestage/getFinalGrade с параметрами
<br> Указать Id студента и Id курса
<br> Пример
<br> /coursestage/getFinalGrade?studentId=6&courseId=2
<br> Если студент еще не окончил курс или если даже не поступил на него, то отметка не прейдет. Будет Json с сообщением, о том что курс еще не окончен. (Проверка на то, что студент поступил или нет не производиться)
<br> В ответ прейдет JSON файл с тремя параметрами: средняя оценка по курсу и id курса и id студента.
***
Также сделал все пять CRUD методов для студента.
***
<br>Задание 4.
<br>Получить xlsx файл в виде потока байтов можно по адресу http://localhost:8080/report/get
<br>Post запросом.
<br>Данную задачу реализовал с помощью нативного запроса SQL
***

<br>Задание 5.
<br>Реализовал с помощью библиотеки Thymeleaf. Здесь конечно слабенько, но как умею.
<br> Переходим по ссылке /professor
***

