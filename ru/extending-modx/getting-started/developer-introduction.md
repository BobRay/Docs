---
title: "Введение для разработчиков"
translation: "extending-modx/getting-started/developer-introduction"
---

## Что такое MODX?

MODX Revolution - это платформа приложений контента, созданная для разработчиков, дизайнеров и пользователей, которым нужна мощная масштабируемая система со встроенным гибким управлением контентом.

## Что такое MVC?

MVC - это «Model-View-Controller», общая парадигма программирования, в которой модель данных доступна только через контроллер, который подключается к представлению, которое можно легко изменить без необходимости изменения модели.

### Что такое MVC²?

MVC² - это терминология MODX, которая называется «Модель-Вид-Контроллер/Коннектор. По сути, это добавляет новый способ доступа к модели из представления - коннекторы, которые представляют собой файлы на основе AJAX, которые «подключаются» к процессорам для обеспечения удаленного взаимодействия с CRUD.

### Отношения коннектор/процессор

Коннектор - это просто файлы шлюза, которые подключаются к определенным процессорам. Они используются главным образом для предотвращения прямого доступа к процессорам и ограничения доступа пользователей к этим процессорам.

## Что такое xPDO?

[xPDO](extending-modx/xpdo "xPDO") наше имя для открытых расширений PDO. Это легковесная библиотека ORB (объектно-реляционный мост), которая работает на PHP 4 и 5 и использует преимущества недавно принятого стандарта для сохранения базы данных в PHP 5.1+, PDO. Он реализует очень простой, но эффективный шаблон Active Record для доступа к данным, а также гибкую модель предметной области, которая позволяет изолировать логику домена от логики, специфичной для базы данных, или нет, в зависимости от ваших потребностей.

## Что такое ORM?

Как определено [Wikipedia](http://www.wikipedia.org/wiki/Object-relational_model):

> Объектно-реляционная база данных (ORD) или система управления объектно-реляционной базой данных (ORDBMS) представляет собой систему управления базами данных (СУБД), аналогичную реляционной базе данных, но с объектно-ориентированной моделью базы данных: объекты, классы и наследование являются непосредственно поддерживается в схемах базы данных и на языке запросов. Кроме того, он поддерживает расширение модели данных с помощью пользовательских типов данных и методов.

По сути, таблицы в базах данных SQL становятся классами, которые могут содержать методы, специфичные для таблиц, наследоваться от базовых классов и многое другое.

## Краткий обзор Revolution

Revolution по своей сути - структура управления контентом. Это не PHP Application Framework, как CodeIgnitor или Symfony, и он не претендует на то, чтобы быть таковым. Тем не менее, это гораздо больше, чем обычные CMS, такие как Wordpress или другие: это позволяет создавать приложения для управления контентом с легкостью и расширяемостью.

Revolution основывает свою внутреннюю структуру на том, что мы называем системой проектирования MVC². Это свободно основано на MVC, или [model-view-controller](http://en.wikipedia.org/wiki/Model-view-controller) архитектурный паттерн, в программировании.

### Модель

_M_ обозначает _Model_, который является основными классами, которые управляют записями данных. Эти базовые классы с префиксом 'mod' в Revolution обрабатывают всю логику домена для MODX Revolution.

Это также включает в себя то, что Revolution называет «процессорами», то есть сценариями, которые обрабатывают доменную логику для MODX Revolution. Они никогда не доступны напрямую и используются для обработки форм, запросов REST, запросов AJAX и многого другого. Они напоминают основные задачи обработки CRUD (Create-Read-Update-Delete).

### Вид

Представления в MODX Revolution называются «шаблонами», но используются по-разному в зависимости от контекста, о котором мы говорим.

#### В интерфейсе это шаблоны, чанки и ресурсы.

##### [Шаблоны](building-sites/elements/templates "Шаблоны")

Шаблоны - это то, как они звучат. Они позволяют вам создавать шаблоны, которые будут инкапсулировать больше специфичных для страницы данных. Думайте о них как о верхних и нижних колонтитулах, объединенных в одно целое (и многое другое!)

##### [Чанки](building-sites/elements/chunks "Чанки")

Куски - это небольшие кусочки HTML-кода, которые можно вставить куда угодно. В некотором смысле они представляют виджеты View из-за их модульности и простоты вставки.

##### Ресурсы

Ресурсы - это базовое представление одной «веб-страницы» в MODX Revolution. Они представляют одну страницу или ресурс, с помощью которого клиент получает доступ к контенту с сервера. Это могут быть файлы, веб-ссылки, символические ссылки или просто старые HTML-страницы, обернутые в [Шаблоны](building-sites/elements/templates "Шаблоны").

#### В диспетчере

На стороне менеджера MODX Revolution представление также обрабатывается шаблонами, хотя они основаны на файлах и расположены в менеджере/шаблонах и в настоящее время загружаются через Smarty.

### Контроллер

Контроллеры в MODX Revolution бывают двух видов. В клиентской части это обработчики запросов (через класс modRequest), а также сниппеты и плагины.

#### [Сниппеты](extending-modx/snippets "Сниппеты")

Сниппеты - это просто код PHP, который можно разместить в любом месте страницы. Они могут быть размещены в [Чанках](building-sites/elements/chunks "Чанках"), [Шаблоны](building-sites/elements/templates "Шаблоны"), или Ресурсы. Они просто исполняют код PHP при каждом вызове и возвращают любой вывод, который они хотели бы отправить на страницу.

#### [Плагины](extending-modx/plugins "Плагины")

Плагины также являются PHP-кодом, но нацелены на определенные системные события, которые происходят во время обработки запроса. Они могут возникать до того, как веб-страница будет обработана, после нее, до обработки запроса или во многих других местах.

Они позволяют пользователям писать общий код, который влияет на основные функциональные возможности страницы, такие как цензура слов, автоматическое создание ссылок, обработка отдельного кэша, перенаправление контекста и многое другое.

### Второй C: Коннекторы

Коннекторы - новая идея для MODX Revolution, они являются точками доступа для процессоров. Система менеджеров в MODX Revolution широко использует их - они обеспечивают безопасные места для запросов AJAX для обработки данных об определенных объектах.

Например, запрос соединителя, который загружает `/modx/connectors/resource/index.php` с параметром `get` действия GET `get` и параметром GET `id`, будет (при условии, что клиент запроса имеет доступ) захватить Ресурс с указанным идентификатором и возврат его в формате JSON (или в любом другом формате; по умолчанию в Revolution это JSON).

Каждый запрос Коннектора также защищен разрешениями контекста, загруженными на каждый запрос. Если пользователь не имеет доступа (через Политику доступа, назначенную контексту запроса), соединитель откажется предоставить данные.

Соединители позволяют выполнять динамические и безопасные запросы JSON (и, в конечном итоге, запросы на основе REST) ​​прямо из менеджера MODX.

## Смотрите также

1. [Начало работы Разработка](extending-modx/getting-started)
2. [xPDO](extending-modx/xpdo), слой базы данных для Revolution
3. [Объяснение структуры каталогов](getting-started/directory-structure "Объяснение структуры каталогов")
4. [Словарь терминов](getting-started/glossary "Словарь терминов")