Автоматическая генерация тестов
---

Скрипт предназначен для создания заготовки для тестирования на основе xUnitFor1C. Парсится текст модуля, находятся все экспортируемые методы и их параметры. На каждый экспортируемый метод создается заготовка для теста по правилу:

Если Метод = Процедура Тогда Тест__Должен__<ИмяПроцедуры>

Если Метод = Функция Тогда Тест__Должен__Вернуть__<ИмяФункции>

Исходный код:

<pre>
// Имеет ли пользователь Административные полномочия
// 
// Возвращаемое значение:
//   Булево   - Является Да/Нет
//
Функция ЭтоАдминистратор() Экспорт
	Возврат Истина;
КонецФункции // ЭтоАдминистратор()

// Вернуть какой-то список
//
//# Параметры:
//#  ТипДанных  - Тип - Тип данных для обработки
//                 <продолжение описания параметра>
//#  ПостОбработка - Булево - Описание
//
//# Возвращаемое значение:
//#   Массив   - Записи
//#   Неопределено  - Записи
//#   Соответствие - Данные в ином виде
//
//#TestMethod: 
&НаСервере
Функция ПолучитьСписокОбъектов(ТипДанных, ПостОбработка = Истина) Экспорт // текст
	
КонецФункции // ПолучитьСписок()
</pre>

Результа работы:

<pre>
СписокТестов.Добавить("Тест_Должен_Вернуть_ЭтоАдминистратор");
СписокТестов.Добавить("Тест_Должен_Вернуть_ПолучитьСписокОбъектов");

Процедура Тест_Должен_Вернуть_ЭтоАдминистратор() Экспорт
	Сообщить("Тест пустышка!");
	// Результат = ОбъектТестирования.ЭтоАдминистратор();
КонецПроцедуры

Процедура Тест_Должен_Вернуть_ПолучитьСписокОбъектов() Экспорт
	Сообщить("Тест пустышка!");
	// ТипДанных = Неопределено;
	// ПостОбработка = Неопределено;
	// Результат = ОбъектТестирования.ПолучитьСписокОбъектов(ТипДанных, ПостОбработка);
КонецПроцедуры
</pre>


Для получения методов с тестами и без анализируется специальная метка в коде:

<code>//#TestMethod: <ИмяМетодаВТесте></code>

Для исключения метода из списка тестируемых необходимо установить значение равное 'none'

<code>//#TestMethod: none</code>

Параметры и возвращаемые значения получаются парсингом описания метода, парсер анализирует строки начинающиеся с '//#'.

Учитывается:

<code>//#  ВидСправочника  - Строка - ВидСправочника</code>

Не учитывается:

<code>//  ВидСправочника  - Строка - ВидСправочника</code>

**ВАЖНО: Параметры методов должны размещаться в одной строке**


##Классы

###РаботаСТекстовымиФайлами

###ГенераторТестов

  - `ПолучитьСписокМетодов` - Структура - Список методов. 

    Парсит текст файла с модулем, получает список всех методов и параметров. 
    **ВАЖНО: Параметры берутся только из описания метода**

    Ключи: 

    - Тип - Строка - Процедура/Функция

    - Имя - Строка - Имя метода

    - Экспорт - Булево - Признак экспортного метода

    - Параметры - Структура - Параметры метода

    - Возврат - Структура - Возвращаемые значения функции, если процедура - пустая структура

    - Тест - Строка - Имя тесового метода

  - `ПолучитьСписокМетодовБезТестов` - Структура - Методы без тестов. Тестированию подлежат **экспортные** методы.

  - `ПолучитьСписокМетодовCТестами` - Структура - Методы с указанным методом теста. Тестированию подлежат **экспортные** методы

  - `СоздатьТесты` - Строка - Генерация костяка теста для методов без тестов.

###ГенераторМодулейНаОсновеТестов
