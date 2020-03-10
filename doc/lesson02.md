# Онлайн-проект <a href="https://github.com/JavaWebinar/topjava">Topjava</a>

## <a href="https://drive.google.com/drive/folders/0B9Ye2auQ_NsFfkpsWE1uX19zV19IVHd0bTlDclc5QmhMMm4xa0Npek9DT18tdkwyLTBZdXM">Материалы занятия</a> (скачать все патчи можно через Download папки patch)

## ![hw](https://cloud.githubusercontent.com/assets/13649199/13672719/09593080-e6e7-11e5-81d1-5cb629c438ca.png) Разбор домашнего задания HW1:

- **Перед сборкой проекта (или запуском Tomcat) откройте вкладку Maven Projects и сделайте `clean`**
- **Если страничка в браузере работает неверно, очистите кэш (`Ctrl+F5` в хроме)**

### ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 1. <a href="https://drive.google.com/open?id=0B9Ye2auQ_NsFXzByNVF3VV9zM1k">Отображения списка еды в JSP</a>
#### Apply 2_1_HW1.patch

> - Переименовал `TimeUtil` в `DateTimeUtil`
> - Переименовал `mealList.jsp` в `meals.jsp`
> - Изменения в `MealsUtil`:
>    - Сделал константу `List<Meal> MEALS`
>    - Для фильтрации по времени и без нее в метод `filteredByStreams` передаю реализацию `Predicate` (см. паттерн [Стратегия](https://refactoring.guru/ru/design-patterns/strategy))
> - Форматирование даты сделал на основе <a href="http://stackoverflow.com/questions/35606551/jstl-localdatetime-format#35607225"> Custom EL function</a>
>    - [Create a custom Function for JSTL через tag library descriptor (TLD)](http://findnerd.com/list/view/How-to-create-a-custom-Function-for-JSTL/2869/)
> - Добавил еще один способ вывести `dateTime` через стандартную JSTL функцию `replace`  (префикс `fn` в шапке также надо поменять)

- [jsp:useBean](http://java-online.ru/jsp-actions.xhtml#useBean)
- [MVC - Model View Controller](http://design-pattern.ru/patterns/mvc.html)

### ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 2. <a href="https://drive.google.com/open?id=0B9Ye2auQ_NsFQndBeWFOa3phRTg">Optional: реализация CRUD</a>
#### Apply 2_2_HW1_optional.patch
Про использование паттерна Repository будет подробно рассказано в видео "Слои приложения"
> - Согласно ответам на [Java Interfaces/Implementation naming convention](https://stackoverflow.com/questions/2814805/java-interfaces-implementation-naming-convention) 
убрал `Impl` в `InMemory` (и всех последующих) реализациях репозиториев. Они не нужны.
> - Поправил `InMemoryMealRepository.save()`. Если обновляется еда, которой нет в хранилище (c несуществующим id), вставка не происходит.
> - В `MealServlet.doGet()` сделал выбор через `switch`
> - В местах, где требуется `int`, заменил `Integer.valueOf()` на `Integer.parseInt()`
> - В `meal.jsp` использую <a href="http://stackoverflow.com/questions/1890438/how-to-get-parameters-from-the-url-with-jsp#1890462">параметр запроса `param.action`</a>, который не кладется в атрибуты.
> - Переименовал `mealEdit.jsp` в `mealForm.jsp`. Поля ввода формы добавил `required`
> - Пофиксил багу c `history.back()` в `mealForm.jsp` для **FireFox** (коммит формы при Cancel, сделал `type="button"`).

Дополнительно:
  - <a href="http://stackoverflow.com/questions/246859/http-1-0-vs-1-1">HTTP 1.0 vs 1.1</a>

### ![question](https://cloud.githubusercontent.com/assets/13649199/13672858/9cd58692-e6e7-11e5-905d-c295d2a456f1.png) Вопросы по HW1

> Зачем в `InMemoryMealRepository` наполнять map с помощью нестатического блока инициализации, а не в конструкторе?

Разницы нет. Сделал чтобы напомнить вам про эту конструкцию. [Малоизвестные особенности Java](https://habrahabr.ru/post/133237/)

> Почему `InMemoryMealRepository` не singleton?

Начиная с Servlet API 2.3 пул сервлетов не создается, [создается только один инстанс сервлетов](https://stackoverflow.com/questions/6298309). Те. `InMemoryMealRepository` в нашем случае создается тоже только один раз. Далее все наши классы слоев приложения будут создаваться через Spring, бины которого по умолчанию являются синглтонами (в его контексте).  

> `Objects.requireNonNull` в `MealServlet.getId(request)` если у нас нет `id` в запросе бросает NPE (`NullPointerException`). Но оно вылетит и без этого метода. Зачем он нужен и почему мы его не обрабатываем?

`Objects.requireNonNull` - это проверка предусловия (будет подробно на 4-м занятии). Означает что в метод пришел неверный аргумент (должен быть не null) и приложение сообщает об ошибке сразу на входе (а не "может быть где-то потом"). См. [What is the purpose of Objects#requireNonNull](https://stackoverflow.com/a/27511204/548473). Если ее проглатывать или замазывать, то приложение возможно где-то работает неверно (приходят неверные аргументы), а мы об этом не узнаем. Красиво обрабатывать ошибки будем на последних занятиях (Spring Exception Handling).

## Занятие 2:
### ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 3. <a href="https://drive.google.com/open?id=0B9Ye2auQ_NsFVDJZVTktQzRYTWc">Библиотека vs Фреймворк. Стандартные библиотеки Apache Commons, Guava</a>
- <a href="http://commons.apache.org/">Apache Commons</a>, <a href="https://github.com/google/guava/wiki">Guava</a>
  - Guava используется на проекте [Многомодульный maven. Многопоточность. XML (JAXB/StAX). Веб сервисы (JAX-RS/SOAP). Удаленное взаимодействие (JMS/AKKA)](http://javaops.ru/reg/masterjava)  
   
### ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 4. <a href="https://drive.google.com/open?id=1Ve2vLlZAHwOKBfpuOL9X_jbSpQMGsLVP">Многоуровневая(многослойная) архитектура</a>
![Слои приложения](http://javaops.ru/static/images/topjava/top-scheme.jpg)
-  <a href="https://metanit.com/sharp/mvc5/23.5.php">Многоуровневая(многослойная) архитектура</a>
-  <a href="https://ru.wikipedia.org/wiki/Data_Access_Object">Data Access Object</a>
-  <a href="http://martinfowler.com/eaaCatalog/dataTransferObject.html">Паттерн DTO</a>
-  <a href="http://stackoverflow.com/questions/21554977/should-services-always-return-dtos-or-can-they-also-return-domain-models">Should services always return DTOs, or can they also return domain models?</a>
- [Mapping Entity->DTO goes in which application layer: Controller or Service?](http://stackoverflow.com/questions/31644131/spring-dto-dao-resource-entity-mapping-goes-in-which-application-layer-cont/35798539#35798539)
-  Дополнительно:
   -  <a href="http://stackoverflow.com/questions/1612334/difference-between-dto-vo-pojo-javabeans">Value Object и Data Transfer Object</a>
   -  <a href="http://codehelper.ru/questions/205/new/repository-и-dao-отличия-преимущества-недостатки">Паттерны Repository и DAO</a>
   - <a href="http://habrahabr.ru/post/263033/">Забудьте о DAO, используйте Repository</a>
   - <a href="http://stackoverflow.com/questions/6640784/difference-between-active-record-and-dao">Difference between Active Record and DAO</a>

### ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 5. <a href="https://drive.google.com/open?id=1pWsm_fNfxqPB8yygWJsMUpvWMVyRxIqt">Создание каркаса приложения для пользователей</a>

#### Apply 2_3_app_layers.patch
> - Убрал интерфейсы к сервисам. Я всегда предпочитаю писать меньше кода и в случае с одной реализацией можно обходится без них. 
    По поводу инкапсуляции и отделения API от реализации - мне кажется сейчас все стремится к упрощению, в том числе и в жизни разработчиков.
> - Переименовал `ExceptionUtil` в `ValidationUtil`
> - Поменял `LoggedUser` на `SecurityUtil`. Это класс, из которого приложение будет получать данные авторизированного пользователя (пока авторизации нет, он реализован как заглушка). Находится в пакете `web`, т.к. авторизация происходит на слое контроллеров и остальные слои приложения про нее знать не должны.
> - Добавил проверку id пользователя, пришедшего в контроллер ([treat IDs in REST body](https://stackoverflow.com/a/32728226/548473), "If it is a public API you should be conservative when you reply, but accept liberally")

## ![question](https://cloud.githubusercontent.com/assets/13649199/13672858/9cd58692-e6e7-11e5-905d-c295d2a456f1.png) Ваши вопросы

> Почему у `User.registered` тип `Date`, а `Meal.dateTime` `LocalDateTime `?

Время регистрации - абсолютное (конкретный момент), а время еды по бизнес логике относительно (те не зависит от часового пояса)

>  Какова цель деления приложения на слои?

Управляемость проекта (особенно большого) повышается на порядок:
- Обеспечивается меньшая связываемость. Допустим если мы меняем что-то в контроллере, то сервис эти изменения не задевают.
- Облегчается тестирование (мы будем тестировать слои сервисов и контроллеров отдельно)

> DTO используются когда есть избыточность запросов, которую мы уменьшаем, собрав данные из разных бинов в один? Когда DTO необходимо использовать?

(D)TO может быть как частью одного entity  (набор полей) так и набором нескольких entities.
В нашем проекте для данных, которые надо отдавать наружу и отличающихся от Entiy (хранимый бин), мы будем делать (Data) Transfer Object и класть в отдельный пакет to. Например `MealsTo` мы отдаем наружу и он является Transfer Object, его пернесем в пакет `to`.
На многих проектах (и собеседованиях) практикуют разделение на уровне maven модулей entity слоя от логики и соответствующей конвертацией ВСЕХ Entity в TO, даже если у них те же самые поля.
Хороший ответ когда TO обязательны есть на <a href="http://stackoverflow.com/questions/21554977/should-services-always-return-dtos-or-can-they-also-return-domain-models#21569720">stackoverflow: When to Use</a>.

> Почему контроллеры положили в папку web, а не в controllers?

То же самое что `domain/model` - просто разные названия.

> Зачем мы наследуем `NotFoundException` от `RuntimeException`?

Так с ним удобнее работать. И у нас нет никаких действий по восстановлению состояния приложения (no recoverable conditions): <a href="http://stackoverflow.com/questions/6115896/java-checked-vs-unchecked-exception-explanation">checked vs unchecked exception</a>. По последним данным checked exception вообще depricated: <a href="http://blog.takipi.com/ignore-checked-exceptions-all-the-cool-devs-are-doing-it-based-on-600000-java-projects">Ignore Checked Exceptions</a>

> Что такое `ProfileRestController`?

Контроллер, где авторизованный пользователь будет работать со своими данными

> Зачем в `AdminRestController` переопределяются методы родителя с вызовом тех же родительских?

Сделано на будущее, мы будем работать с `AdminRestController`.

> Что лучше возвращать из API: `Collection` или `List`

Вообще, как правило, возвращают `List`, если не просится по коду более общий случай (например возможный `Set` или `Collection`, возвращаемый `Map.values()`). Если возвращается отсортированный список, то `List` будет адекватнее.

###  ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 6. <a href="https://drive.google.com/open?id=14XPypWzxYdjxir6dGTjqTOCBZUu_Jy3C">Обзор  Spring Framework. Spring Context.</a>
>  [Видео про миграцию на Spring 5](http://javaops.ru/view/resources/spring5) у нас будет позже. Изменения в логгировании - [Spring Framework 5.0 comes with its own Commons Logging bridge out of the box](https://github.com/spring-projects/spring-framework/wiki/What's-New-in-Spring-Framework-5.x):
>  - spring-jcl instead of standard Commons Logging
>  - Autodetecting Log4j 2.x, SLF4J, JUL (java.util.logging) without any extra bridges. 

**[Что такое Spring Framework? От внедрения зависимостей до Web MVC](https://habr.com/ru/post/490586)**

#### Apply 2_4_add_spring_context.patch
-  <a href="http://en.wikipedia.org/wiki/Spring_Framework">Spring Framework</a>
-  <a href="http://spring.io/projects">Проекты Spring</a>.
-  <a href=https://docs.spring.io/spring/docs/current/spring-framework-reference/html/overview.html>Обзор Spring Framework</a>

#### Apply 2_5_add_dependency_injection.patch
-  <a href="https://ru.wikipedia.org/wiki/Инверсия_управления">Инверсия управления.</a>
-  <a href="http://habrahabr.ru/post/131993/">IoC, DI, IoC-контейнер — Просто о простом</a>

#### Apply 2_6_add_annotation_processing.patch
> - Закомментировал ненужный `context:annotation-config`: сканирование аннотаций подключаются при `context:component-scan`

-  <a href="http://stackoverflow.com/questions/6827752/whats-the-difference-between-component-repository-service-annotations-in">Difference
   between @Component, @Repository & @Service annotations in Spring</a>
-  <a href="http://www.mkyong.com/spring/spring-auto-scanning-components/">Spring Auto Scanning Components</a>
-  <a href="http://www.seostella.com/ru/article/2012/02/12/ispolzovanie-annotacii-autowired-v-spring-3.html">Использование аннотации @Autowired</a>
-  Дополнительное:
   -  <a href="https://docs.spring.io/spring/docs/current/spring-framework-reference/html/beans.html">Introduction to the Spring IoC container and beans</a>
   - [Подготовка к Spring Professional Certification. Контейнер, IoC, бины](https://habr.com/ru/post/470305/)
   -  <a href="http://it.vaclav.kiev.ua/2010/12/25/spring-framework-for-begginers-part-7/">Constructor против Setter Injection </a>
   -  <a href="https://spring.io/guides">Getting Started</a>
   -  <a href="https://docs.spring.io/spring/docs/current/spring-framework-reference/">Spring Framework Reference Documentation</a>
   -  <a href="https://github.com/spring-projects">Spring на GitHub</a>
   -  <a href="https://dzone.com/refcardz/spring-annotations">Spring Annotations</a>

#### Apply 2_7_constructor_injection.patch
- [Перевод "Field Dependency Injection Considered Harmful"](https://habrahabr.ru/post/334636/)
- [Field vs Constructor vs Setter DI](http://stackoverflow.com/questions/39890849/what-exactly-is-field-injection-and-how-to-avoid-it)

В контроллерах *Constructor Injection* делать не стал, добавляется лишний код (попробуйте сделать сами). На каждом проекте свои правила, универсальных нет.

#### Дополнительно видео по Spring
   - [Юрий Ткач: Spring Framework - The Basics](https://www.youtube.com/playlist?list=PL6jg6AGdCNaWF-sUH2QDudBRXo54zuN1t)
   - [Java Brains: Spring Framework](https://www.youtube.com/playlist?list=PLC97BDEFDCDD169D7)
   - [Тимур Батыршинов: Введение в Spring](https://www.youtube.com/watch?v=MnUThz43viE&list=PL8X2nqRlWfaax2odRVXpnYXwWEF4d4s-J)

### ![video](https://cloud.githubusercontent.com/assets/13649199/13672715/06dbc6ce-e6e7-11e5-81a9-04fbddb9e488.png) 7. <a href="https://drive.google.com/open?id=0B9Ye2auQ_NsFN2N6LS1PVE96SW8">Пояснения к HW2. Обработка Autowired</a>
  
`<context:annotation-config/>` говорит спрингу при поднятии контекста обрабатывать `@Autowired` (добавляется в контекст спринга `AutowiredAnnotationBeanPostProcessor`). После того, как все бины уже в контексте постпроцессор через отражение инжектит все `@Autowired` зависимости. Будет подробнее в видео "Жизненный цикл Spring контекста" на следующем уроке.
 
## ![question](https://cloud.githubusercontent.com/assets/13649199/13672858/9cd58692-e6e7-11e5-905d-c295d2a456f1.png) Ваши вопросы
> Что такое схема в spring-app.xml xsi:schemaLocation и зачем она нужна

<a href="https://ru.wikipedia.org/wiki/XML_Schema">XML схема</a> нужна для валидации xml, IDEA делает по ней автозаполнение.

> Что означает для Spring

     <bean class="ru.javawebinar.topjava.service.UserService">
         <property name="repository" ref="inmemoryUserRepository"/>
     </bean> ?

Можно сказать так: создай и занеси в свой контекст экземпляр класса (бин) `UserService` и присвой его проперти `repository` бин `inmemoryUserRepository`, который возьми из своего контекста.

> Как биндинг происходит для `@Autowired`? Как поступать, если у нас больше одной реализации `UserRepository`?

`@Autowired`  инжектит по типу (т.е. ижектит класс который реализует `UserRepository`). Обычно он один. Если у нас несколько реализаций, Spring не поднимится и поругается - `No unique bean`.
 В этом случае <a href="http://www.mkyong.com/spring/spring-autowiring-qualifier-example/">можно уточнить имя бина через @Qualifier</a>. `@Qualifier` обычно добавляют только в случае нескольких реализаций.
См. [Inject 2 beans of same type](https://stackoverflow.com/a/2153680/548473)

> Почему нельзя сервлет помещать в Spring контекст?

Сервлеты- это исключительно классы `servlet-api` (веб контейнера), они инстанциируются им. Те технически можно ( без `init/destroy`), но идеологически - неверно. Также НЕ надо работать с cервлетом из `SpringMain`.

--------------------
- **Еще раз смотрим на [демо приложение](http://topjava.herokuapp.com) и вникаем, что такое пользователь и его еда и что он может с ней сделать. 
Когда пользователь авторизуется в приложении, его id и норма калорий "чудесным образом" попадают в  `SecurityUtil.authUserId() / SecurityUtil.authUserCaloriesPerDay()`. Как они реально туда попадут будет в уроке  9 (Spring Security, сессия и куки)**
- **Перед началом выполнения ДЗ (если есть хоть какие-то сомнения) прочитайте ВСЕ ДЗ. Если вопросы остаются- то ВСЕ подсказки**. Особенно этот пункт важный, когда будете делать реальное рабочее ТЗ.

## ![hw](https://cloud.githubusercontent.com/assets/13649199/13672719/09593080-e6e7-11e5-81d1-5cb629c438ca.png) Домашнее задание HW02
- 1: Имплементировать `InMemoryUserRepository` по аналогии с `InMemoryMealRepository` (список пользователей возвращать отсортированным по имени)
- 2: сделать `Meal extends AbstractBaseEntity`, `MealTo` перенести в пакет `ru.javawebinar.topjava.to` (transfer objects)
- 3: Изменить `MealRepository` и `InMemoryMealRepository` таким образом, чтобы вся еда всех пользователей находилась в одном общем хранилище, но при этом каждый конкретный авторизованный пользователь мог видеть и редактировать только свою еду.
  - 3.1: реализовать хранение еды для каждого пользователя можно с добавлением поля `userId` в `Meal` ИЛИ без него (как нравится). Напомню, что репозиторий один и приложение может работать одновременно с многими пользователями.
  - 3.2: если по запрошенному id еда отсутствует или чужая, возвращать `null/false` (см. комментарии в `UserRepository`)
  - 3.3: список еды возвращать отсортированный в обратном порядке по датам
  - 3.4: атомарность операций не требуется  (коллизии при одновременном изменении еды одного пользователя можно не учитывать)
- 4: Реализовать слои приложения для функциональности "еда". API контроллера должна удовлетворять все потребности демо приложения и ничего лишнего (см. [демо](http://topjava.herokuapp.com)). 
  - **Смотрите на реализацию слоя для user и делаете по аналогии! Если там что-то непонятно, не надо исправлять или делать по своему. Задавайте вопросы. Если действительно нужна правка- я сделаю и напишу всем.**
  - 4.1: после авторизации (сделаем позднее), id авторизованного юзера можно получить из `SecurityUtil.authUserId()`. Запрос попадает в контроллер, методы которого будут доступны снаружи по http, т.е. запрос можно будет сделать с ЛЮБЫМ id для еды
  (не принадлежащем авторизированному пользователю). Нельзя позволять модифицировать/смотреть чужую еду.
  - 4.2: `SecurityUtil` может использоваться только на слое web (см. реализацию `ProfileRestController`). `MealService` можно тестировать без подмены логики авторизации, принимаем в методах сервиса и репозитория параметр `userId`: id владельца еды.
  - 4.3: если еда не принадлежит авторизированному пользователю или отсутствует, в `MealService` бросать `NotFoundException`.
  - 4.4: конвертацию в `MealTo` можно делать как в слое web, так и в service ([Mapping Entity->DTO: Controller or Service?](http://stackoverflow.com/questions/31644131))
  - 4.5: в `MealService` постараться сделать в каждом методе только одни запрос к `MealRepository`
  - 4.6 еще раз: не надо в названиях методов повторять названия класса (`Meal`).
- 5: включить классы еды в контекст Spring (добавить аннотации) и вызвать из `SpringMain` любой метод `MealRestController` (проверить что Spring все корректно заинжектил)

### Optional
- 6: в `MealServlet` сделать инициализацию Spring, достать `MealRestController` из контекста и работать с едой через него (как в `SpringMain`). `pom.xml` НЕ менять, работаем со `spring-context`. Сервлет обращается к контролеру, контроллер вызывает сервис, сервис - репозиторий.
   - 6.1: учесть, что когда будем работать через Spring MVC, `MealServlet` удалим, те вся логика должна быть в контроллере
- 7: добавить в `meals.jsp` и `MealServlet` две отдельные фильтрации еды: по дате и по времени (см. [демо](http://topjava.herokuapp.com)). Сброс фильтра делать не надо (реализуем через ajax в HW8)
- 8: добавить выбор текущего залогиненного пользователя (имитация авторизации, сделать Select с двумя элементами со значениями 1 и 2 в `index.html` и `SecurityUtil.setAuthUserId(userId)` в `UserServlet`).
Настоящая атворизация будет через Spring Security позже.
---------------------
### ![error](https://cloud.githubusercontent.com/assets/13649199/13672935/ef09ec1e-e6e7-11e5-9f79-d1641c05cbe6.png) Типичные ошибки и подсказки по реализации

- 1: **В реализации `InMemoryUserRepository`**
  - 1.1: `getByEmail` попробуйте сделать через `stream`
  - 1.2: `delete` попробуйте сделать за одно обращение к map (без `containsKey`)
  - 1.3: при запросе списка юзеров предусмотрите случай одинаковых `User.name` (порядок должен быть зафиксированным). Поле `User.email`у нас уникально - в базе будет ограничение.
- 2: **В реализации `InMemoryMealRepository`**
  - 2.1: В `Meal`, которая приходит из браузера в контроллер, нет никакой информации о пользователе (еда приходит в контроллер **БЕЗ `user/userId`, она может быть только от авторизированного пользователя**). По id еды и авторизованному пользователю нужно проверить ее принадлежность (его это еда или чужая)
  **Проверьте сценарий: авторизованный пользователь пробует изменить чужую еду (id еды ему не принадлежит).**
  - 2.2: `get\update\delete` - следите, чтобы не было NPE (`NullPointException` может быть, если в хранилище отсутствует юзер или еда).
  - 2.3: Фильтрацию по датам сделать в репозитории т.к. из базы будем брать сразу отфильтрованные по дням записи. Следите чтобы **первый и последний день не были обрезаны, иначе сумма калорий будет неверная**.
  - 2.4: Если запрашивается список и он пустой, не возвращайте NULL! По пустому списку можно легко итерироваться без риска `NullPoinException`.
  - 2.5: Не дублируйте код в `getAll` и метод с фильтрацией
  - 2.6: попробуйте учесть, что следующая реализация (сортировка, фильтрация) будет делаться прямо в базе данных
- 3: Проверьте, что удалили `Meal.id` и связанные с ним методы (он уже есть в базовом `BaseEntity`)
- 4: Проверку `isBetweenInclusive` с датами сделать в `DateTimeUtil`. Попробуйте использовать дженерики и объединить ее с `isBetweenInclusive` со временем (см. [Generics Tutorials](https://docs.oracle.com/javase/tutorial/extra/generics/morefun.html))
- 5: **Реализация 'MealRestController' должен уметь обрабатывать запросы**:
  - 5.1: Отдать свою еду (для отображения в таблице, формат `List<MealTo>`), запрос БЕЗ параметров
  - 5.2: Отдать свою еду, отфильтрованную по startDate, startTime, endDate, endTime
  - 5.3: Отдать/удалить свою еду по id, параметр запроса - id еды. Если еда с этим id чужая или отсутствует - `NotFoundException`
  - 5.4: Сохранить/обновить еду, параметр запроса - Meal. Если обновляемая еда с этим id чужая или отсутствует - `NotFoundException`
  - 5.5: Сервлет мы удалим, а контроллер останется, поэтому возвращать `List<MealTo>` надо из контроллера. И userId принимать в контроллере НЕЛЬЗЯ (иначе - для чего аторизация?). 
  - 5.6: В REST при update принято передавать id (см. `AdminRestController.update`)
  - 5.7: Для получения всей своей еды сделайте отдельный `getAll` без применения фильтра
- 6: Проверьте корректную обработку пустых значений date и time (в частности, если все значения пустые, должен выводится весь список)
- 7: `id` авторизированного пользователя получаем так: `SecurityUtil.authUserId()`, cм. `ProfileRestController`
- 8: В `MealServlet`
  - 8.1: Закрывать springContext в сервлете грамотнее всего в `HttpServlet.destroy()`: если где-то в контексте Spring будет ленивая инициализация, метод-фабрика, не синглтон-scope, то контекст понадобится при работе приложения. У нас такого нет, но делать надо все грамотно.
  - 8.2: Не храните параметры фильтра как члены класса сервлета, это не многопоточно! Один экземпляр сервлета используется всеми запросами на сервер, попробуйте дернуть его из 2х браузеров.

#### Если с ДЗ большие сложности, можно получить итоговые Meal интерфейсы для сверки в личке (у меня, Татьяны, Катерины).
И напоследок история от Татьяны:
> 2.1 По id еды и авторизованному пользователю нужно проверить ее принадлежность.

На примере уточню:
Вася Пупкин нашел неименную банковскую карточку, т.е. номер есть, но имени нет.
Т.к. Вася не очень честный человек, то решил снять деньги с чужой карточки.
Наклеил *стикер со своим именем* на карточку и пришел в банк. Дает свой паспорт и карточку операционисту и просит снять всю наличность.
Варианты:
1. операционист сверяет стикер на карточке и паспорт - все ок, Вася получает наличность
2. операционист не обращает внимания на стикер, *а делает запрос в БД по номеру и сверяет ФИО в БД с паспортом* - ФИО не совпали, Вася в пролете

Кто и так это понимает, тому небольшой спойлер. А кто не понимает, может, переспросят, пообсуждают.
