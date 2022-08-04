Есть разного рода параметры и хидеры для rest запросов(ajax) которые предназначены для ноды, выполнять определенную логику, эти параметры не идут в базу вместе с запросом

### Хидеры
1. **readonly** (boolean, определяет что запрос должен идти на readonly базу)

пример: 
```js
$.ajax({
    url: 'reqs/executeFunc',
    type: 'post',
    headers: {
        'readonly': true
    },
    data: {
        sqlpath: 'security.f_send_reset_password_link',
        email: 'a.azhibek@cloudmaker.kz'
    },
    success: function (result) {
        console.log(result);
    },
    error: function (error) {
        console.log(error);
    }
});

```
2. **cacheable** (boolean, определяет что запрос кэшируемый, сначало ищет в кэше, если нету, то идет в базу, достает данные, их отправляет клиенту и сразу же записывает в кэш)

пример: 
```js
$.ajax({
    url: 'reqs/executeFunc',
    type: 'post',
    headers: {
        'cacheable': true
    },
    data: {
        sqlpath: 'security.f_send_reset_password_link',
        email: 'a.azhibek@cloudmaker.kz'
    },
    success: function (result) {
        console.log(result);
    },
    error: function (error) {
        console.log(error);
    }
});
```
- если readonly не нужен, то можно его не писать, или можно писать со значением false
- если cacheable не нужен, то можно его не писать, или можно писать со значением false
- cacheable и readonly так же реализованы и для веб сокетных запросов, но так как в этих запросах мы не можем отправлять хидеры, эти параметры отправляются как параметры запроса
___
### Параметры
##### Эти параметры в базу не пойдут, используются только нодой
1. **request_to_queue** (boolean, **но нужно отправлять в виде текста**, определяет идет ли запрос в очередь, если нет то выполняется сразу, если идет в очередь, то записывается в таблицу `topcore.execute_queue`, оттуда выполняется джобом в порядке очереди, а клиенту возвращается что запрос добавлен в очередь)
пример: 
```js
$.ajax({
    url: 'reqs/executeFunc',
    type: 'post',
    data: {
        sqlpath: 'security.f_send_reset_password_link',
        request_to_queue: 'false',
        domain: 'tophr.kz',
        email: 'a.azhibek@cloudmaker.kz'
    },
    success: function (result) {
        console.log(result);
    },
    error: function (error) {
        console.log(error);
    }
});
```
2. **columns** (массив строк, список колонок которые должны прилетать, в executeFunc запрашиваются только эти колонки, в sprav/other1234 запрашиваются все, и при отправке на клиента оставляются только эти колонки, все остальные удаляются, то есть **для sprav/other1234 оптимизацию не дает**, а **для executeFunc дает оптимизацию**)

пример: 
```js
$.ajax({
    url: 'reqs/executeFunc',
    type: 'post',
    contentType: 'application/json;charset=UTF-8',
    data: JSON.stringify({
        sqlpath: "ds.f_sprav_tophr_tree_by_analysis_path_v3",
        p: {
            "_gridoperationtype": "select",
            "currentgridrec": {},
            "show_connected": false,
            "analysis_paths": 18,
            "conn_entities": false,
            "entity_groups": false,
            "date_start": "01.01.0100",
            "date_end": "31.12.9999",
            "file_type": "AVATAR",
            "is_overrided": true,
            "filter": null,
            "add_filter": null,
            "custom_form": false,
            "grid_id": "33333",
            "levels": "2",
            "limit": "100",
            "version_type": "default"
        },
        columns: ["name", "codes_path", "analysis_path_id", "entity_type_code"]
    }),
    success: function (result) {
        console.log(result);
    },
    error: function (error) {
        console.log(error);
    }
});

```
3. **excludeColumns** (массив строк, список колонок которые будут удалены с конечного ответа, обрезается на ноде, **оптимизацию не дает**)

пример:
```js
$.ajax({
    url: 'reqs/executeFunc',
    type: 'post',
    contentType: 'application/json;charset=UTF-8',
    data: JSON.stringify({
        sqlpath: "ds.f_sprav_tophr_tree_by_analysis_path_v3",
        p: {
            "_gridoperationtype": "select",
            "currentgridrec": {},
            "show_connected": false,
            "analysis_paths": 18,
            "conn_entities": false,
            "entity_groups": false,
            "date_start": "01.01.0100",
            "date_end": "31.12.9999",
            "file_type": "AVATAR",
            "is_overrided": true,
            "filter": null,
            "add_filter": null,
            "custom_form": false,
            "grid_id": "33333",
            "levels": "2",
            "limit": "100",
            "version_type": "default"
        },
        excludeColumns: ["name", "codes_path", "analysis_path_id", "entity_type_code"]
    }),
    success: function (result) {
        console.log(result);
    },
    error: function (error) {
        console.log(error);
    }
});

```
