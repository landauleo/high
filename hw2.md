##MySQL

###Производительность индексов:

####Описание/Пошаговая инструкция выполнения:
1. Сгенерировать любым способом 1,000,000 анкет. Имена и Фамилии должны быть реальными (чтобы учитывать селективность индекса) (или воспользоваться уже готовым списком)
   
2. Реализовать функционал поиска анкет по префиксу имени и фамилии (одновременно) в вашей социальной сети (реализовать метод /user/search из спецификации) (запрос в форме firstName LIKE ? and secondName LIKE ?). Сортировать вывод по id анкеты. Использовать InnoDB движок
   
3. Провести нагрузочные тесты этого метода. Поиграть с количеством одновременных запросов. 1/10/100/1000
   
4. Построить графики и сохранить их в отчет
   
5. Сделать подходящий индекс
   
6. Повторить пункт 3 и 4.
   
7. В качестве результата предоставить отчет в котором должны быть:
   графики latency до индекса;
   графики throughput до индекса;
   графики latency после индекса;
   графики throughput после индекса;
   запрос добавления индекса;
   explain запросов после индекса;
   объяснение почему индекс именно такой;

#####To migrate swagger from SpringFox to SpringDoc:
- https://springdoc.org/#migrating-from-springfox

|                   | without index | btree index | hash index |
|-------------------|---------------|-------------|------------|
| users insert      | 87s           | 93s         | 88s        |
| 1 user search     | 0,389427s     | 0,045628s   | 0,079947s  |
| 10 users search   | 3s            | 0,05413s    | 0,073272s  |
| 100 users search  | 34s           | 0,369564s   | 0,308973s  |
| 1000 users search | 331s          | 0,186761    | 0,062209s  |

#####Why not to use HASH index here?
- according to docs https://dev.mysql.com/doc/refman/8.0/en/index-btree-hash.html "Only whole keys can be used to search for a row", which is not a flexible way for username search

####mysql EXPLAIN command
#####preparation:

      mysql --host=localhost --user=root --password=changeme app

      explain analyze SELECT * FROM user WHERE first_name LIKE 'максим' AND second_name LIKE 'носов';
- HASH index:


      +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | EXPLAIN                                                                                                                                                                                                                                                                                                                                      |
      +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | -> Filter: (`user`.first_name like 'максим')  (cost=626 rows=33.6) (actual time=3.22..23.3 rows=41 loops=1)
      -> Index range scan on user using user_second_name_idx over (second_name = 'носов'), with index condition: (`user`.second_name like 'носов')  (cost=626 rows=772) (actual time=1.14..23.2 rows=772 loops=1)
      |
      +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      1 row in set (0.03 sec)
- BTREE index:


      +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | EXPLAIN                                                                                                                                                                                                                                                                                                                                      |
      +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      | -> Filter: (`user`.first_name like 'максим')  (cost=675 rows=41.4) (actual time=5.26..43.7 rows=41 loops=1)
      -> Index range scan on user using user_second_name_idx over (second_name = 'носов'), with index condition: (`user`.second_name like 'носов')  (cost=675 rows=772) (actual time=4.03..43.6 rows=772 loops=1)
      |
      +----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
      1 row in set (0.05 sec)

####Swagger:
localhost:8080/swagger-ui/index.html#/