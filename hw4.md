##Postgres

###Лента постов от друзей:

####Описание/Пошаговая инструкция выполнения:
1. Лента постов друзей (метод /post/feed из спецификации) https://github.com/OtusTeam/highload/blob/master/homework/openapi.json
2. Лента постов друзей формируется на уровне кешей
3. (Опционально) Формирование ленты производить через постановку задачи в очередь на часть друзей, чтобы избежать эффекта леди Гаги 
4. В ленте держать последние 1000 обновлений друзей https://github.com/OtusTeam/highload/blob/master/homework/posts.txt
5. Лента должна кешироваться

####Swagger:
localhost:8080/swagger-ui/index.html#/