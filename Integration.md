Интеграция в ноде – integration.js
Основные функции в integration.js. Каждая строка, начиная с 233 закомментирована, всё, что до неё – это подключение и всё такое взятое с index.js.

callingFunction – вызывает интеграцию с определённой периодичностью. В конфиге integrationConfig – delayMs.

testIntegration – Первая функция вызываемая callingFunction. Она сначала подключается к базе. И проверяет последнюю успешную интеграцию, и берёт её дату. Проверяет последнюю запись в inbound data c batch_id = 713. И вызывается getIntegrationData передавая ей дату. 

getIntegrationData – основная функция собирающая json объект. 

Всё, кроме employees собирается напрямую. Employees собираются и немного изменяется структура самого json. Смотреть комментарии в коде.

Далее возвращается обратно в testIntegration и отправляется в базу. (В будущем сначала будет отправляться в секретную базу, получать изменённый ответ, и отправлять в реальную базу)
Данные отправляются в функцию topcore.f_entities_calls . Саурановская функция в которой проходят все манипуляции с данными.

После этого вызывается функция ds.f_load_orders. Функция которая обрабатывает приказы и заявления на увольнения(Все комментарии в самой функции). 

http://172.22.222.252:33999/bankstructure/v1/swagger.json – Сваггер по вебсервису с данными. 

На примере /employee – Выдаёт список всех сотрудников, работающих на данный момент. 

/employee?changeAfter=2022-03-30T00:00:00 – Выдаёт всех сотрудников, у которых были изменения, переводы с 30 марта 2022 года. 

/employee?changeAfter=2022-03-30T00:00:00&active=1 – Выдаёт всех сотрудников уволенных с 30 марта 2022 года.

Эти же ключи используются в /departments и /staffing. 

В /orders используется ключ from и to 

Дополнительно. 

Периодически проверять когда была последняя интеграция. (Select * from integration.inbound_data where batch_id = 713). Если была слишком давно, попросить Эльдара скинуть логи с ноды.

Заявления на увольнение – ordertypeid = 7199

Бранч с комментариями в ноде – commentsIntegration
 
Бранч с куском с приватной базой – integrationPrivate (Прочитать комментарий коммита там)

Функция по переносу справочных со старого на новое подразделение 
topcore.f_transfer_old_sprav_pos(old_dep_md, new_dep_md) (Старое, новое)
