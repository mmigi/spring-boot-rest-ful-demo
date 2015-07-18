# spring-boot-rest-ful-demo

## API

### Как использовать
1. Отправить файл(картинку) на сервер можно, передав по адресу 
`http://192.168.59.103:8080/upload `
следющие данные:
```
"name" - в формате String
"file" - в формате MultipartFile
```
Сервер вернет URI файла. Данные будут сохранены в таблице pictures.<br/>
2. Добавить нового пользователя на сервер можно, передав по адресу 
  `http://192.168.59.103:8080/create `
следующие данные:
```
"email" в формате String
"name" в формате String
"phone" в формате String
"address" в формате String
"pictureId" в формате Long
```
Сервер вернет URI пользователя. Данные будут сохранены в таблице users.<br/>
3. получить информацию о пользователе можно, передав по адресу 
`http://192.168.59.103:8080/user/{id}`
id пользователя, указав его вместо "{id}".
пример:
`http://192.168.59.103:8080/user/2`<br/>
4. Изменить статус пользователя можно, передав по адресу 
`http://192.168.59.103:8080/user/{id}/{status}`
id пользователя, указав его вместо "{id}" и статус в формате Boolean (true/false), указав его вместо "{status}"
пример:
`http://192.168.59.103:8080/user/2/true`
Ответ сервера будет содержать:
- ID пользователя
- текущий статус (который был указан в запросе)
- предыдущий статус.
<br/>
Внимание! Ответ сервера происходит с искусственной задержкой.
<br/>
5. Получить общую статистику можно, передав по адресу
`http://192.168.59.103:8080/user/statistic`
следующие данные:
```
"status" в формате Boolean (не обязательно)
"time" В формате: "dd-MM-yyyy-HH-mm-ss" (не обязательно)
```
Ответ сервера будет формироваться в зависимости от переданных ему данных:
- если не указанно ни одного парамета, то сервер вернет списком статистику по всем пользователям
- если указан параметр "status", то сервер вернет списком статистику по пользователем с указанным статусом
- если указан параметр "time", то сервер вернет списком статистику по пользователем с временем обновления большим, чем время указанное в запросе
- если указаны оба параметра, то сервер вернет списком статистику по пользователем с указанным статусом и временем обновления большим, чем время указанное в запросе.

## Тестирование
Протестировать загрузку файла(1) и создание пользователя(2) можно перейдя по ссылкам ниже и заполнив простые html формы:
- http://192.168.59.103:8080/upload - загрузить картинку
- http://192.168.59.103:8080/create-user - создать пользователя

### Примечание
Все исключения перехватываются и выводятся пользователю текст ошибки.

## Инструкция для запуска и работы приложения
(необходимо, чтобы был запущен boot2docker)

1. запускаем MySQL сервер в контейнере:
```  
docker run --name demo-mysql -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=demo -e MYSQL_USER=demo_user -e       MYSQL_PASSWORD=demo_pass -p 3306:3306 -d mysql:5.6
```
2. Собираем и создаем image приложения:
`mvn clean package docker:build`
<br/>
3. Запускаем приложение в контейнере:
`docker run -p 8080:8080 --name demo-app --link demo-mysql:mysql -d rest-demo/rest-ful-project`
<br/>
приложение будет запущено на порту: 8080.
