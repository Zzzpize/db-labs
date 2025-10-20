# Лабораторная работа №1

## Блок 1. Простые запросы с фильтрацией

### Задача 1.1: Найдите все рейсы, задержанные более чем на 2 часа и вылетающие из UFA.

**SQL-запрос:**
```sql
select * from bookings.flights where departure_airport = 'UFA' and (actual_departure - scheduled_departure) > interval '2 hours';
```

**Результат:**
|flight_id|flight_no|scheduled_departure|scheduled_arrival|departure_airport|arrival_airport|status|aircraft_code|actual_departure|actual_arrival|
|---------|---------|-------------------|-----------------|-----------------|---------------|------|-------------|----------------|--------------|
|13540|PG0305|2017-07-28 10:10:00.000 +0500|2017-07-28 11:50:00.000 +0500|UFA|ESL|Arrived|SU9|2017-07-28 12:56:00.000 +0500|2017-07-28 14:37:00.000 +0500|
|13645|PG0564|2017-08-10 11:30:00.000 +0500|2017-08-10 14:45:00.000 +0500|UFA|UCT|Arrived|CN1|2017-08-10 15:44:00.000 +0500|2017-08-10 19:01:00.000 +0500|
|13668|PG0564|2017-08-05 11:30:00.000 +0500|2017-08-05 14:45:00.000 +0500|UFA|UCT|Arrived|CN1|2017-08-05 14:44:00.000 +0500|2017-08-05 17:57:00.000 +0500|
|13696|PG0567|2017-07-31 12:55:00.000 +0500|2017-07-31 14:10:00.000 +0500|UFA|OVS|Arrived|CR2|2017-07-31 15:51:00.000 +0500|2017-07-31 17:06:00.000 +0500|
|13729|PG0567|2017-08-09 12:55:00.000 +0500|2017-08-09 14:10:00.000 +0500|UFA|OVS|Arrived|CR2|2017-08-09 15:45:00.000 +0500|2017-08-09 17:00:00.000 +0500|
|13778|PG0148|2017-07-17 10:15:00.000 +0500|2017-07-17 10:40:00.000 +0500|UFA|REN|Arrived|CR2|2017-07-17 13:57:00.000 +0500|2017-07-17 14:21:00.000 +0500|
|13786|PG0148|2017-07-30 10:15:00.000 +0500|2017-07-30 10:40:00.000 +0500|UFA|REN|Arrived|CR2|2017-07-30 12:51:00.000 +0500|2017-07-30 13:16:00.000 +0500|
|13800|PG0148|2017-07-23 10:15:00.000 +0500|2017-07-23 10:40:00.000 +0500|UFA|REN|Arrived|CR2|2017-07-23 14:01:00.000 +0500|2017-07-23 14:26:00.000 +0500|


### Задача 1.2: Найдите все рейсы прибившие в SVO в интервале с 16:00 до 19:00 2017-07-22

**SQL-запрос:**
```sql
select * from bookings.flights where arrival_airport = 'SVO' and actual_arrival between '2017-07-22 16:00:00' and '2017-07-22 19:00:00';
```

**Результат**
|flight_id|flight_no|scheduled_departure|scheduled_arrival|departure_airport|arrival_airport|status|aircraft_code|actual_departure|actual_arrival|
|---------|---------|-------------------|-----------------|-----------------|---------------|------|-------------|----------------|--------------|
|13540|PG0305|2017-07-28 10:10:00.000 +0500|2017-07-28 11:50:00.000 +0500|UFA|ESL|Arrived|SU9|2017-07-28 12:56:00.000 +0500|2017-07-28 14:37:00.000 +0500|
|13645|PG0564|2017-08-10 11:30:00.000 +0500|2017-08-10 14:45:00.000 +0500|UFA|UCT|Arrived|CN1|2017-08-10 15:44:00.000 +0500|2017-08-10 19:01:00.000 +0500|
|13668|PG0564|2017-08-05 11:30:00.000 +0500|2017-08-05 14:45:00.000 +0500|UFA|UCT|Arrived|CN1|2017-08-05 14:44:00.000 +0500|2017-08-05 17:57:00.000 +0500|
|13696|PG0567|2017-07-31 12:55:00.000 +0500|2017-07-31 14:10:00.000 +0500|UFA|OVS|Arrived|CR2|2017-07-31 15:51:00.000 +0500|2017-07-31 17:06:00.000 +0500|
|13729|PG0567|2017-08-09 12:55:00.000 +0500|2017-08-09 14:10:00.000 +0500|UFA|OVS|Arrived|CR2|2017-08-09 15:45:00.000 +0500|2017-08-09 17:00:00.000 +0500|
|13778|PG0148|2017-07-17 10:15:00.000 +0500|2017-07-17 10:40:00.000 +0500|UFA|REN|Arrived|CR2|2017-07-17 13:57:00.000 +0500|2017-07-17 14:21:00.000 +0500|
|13786|PG0148|2017-07-30 10:15:00.000 +0500|2017-07-30 10:40:00.000 +0500|UFA|REN|Arrived|CR2|2017-07-30 12:51:00.000 +0500|2017-07-30 13:16:00.000 +0500|
|13800|PG0148|2017-07-23 10:15:00.000 +0500|2017-07-23 10:40:00.000 +0500|UFA|REN|Arrived|CR2|2017-07-23 14:01:00.000 +0500|2017-07-23 14:26:00.000 +0500|


### Задача 1.3: Найдите все утренние рейсы (вылетающие с 4:00 утра до 12:00) выполняемые самолетами Сухой Суперджет (код модели 'SU9').

**SQL-запрос:**
```sql
select * from bookings.flights where aircraft_code = 'SU9' and extract(hour from scheduled_departure) between 4 and 12;
```

**Результат. Первые 10 значений**
|flight_id|flight_no|scheduled_departure|scheduled_arrival|departure_airport|arrival_airport|status|aircraft_code|actual_departure|actual_arrival|
|---------|---------|-------------------|-----------------|-----------------|---------------|------|-------------|----------------|--------------|
|27580|PG0483|2017-09-12 09:20:00.000 +0500|2017-09-12 13:20:00.000 +0500|KEJ|DME|Scheduled|SU9|||
|367|PG0273|2017-09-10 11:50:00.000 +0500|2017-09-10 13:50:00.000 +0500|DME|CEK|Scheduled|SU9|||
|368|PG0273|2017-08-25 11:50:00.000 +0500|2017-08-25 13:50:00.000 +0500|DME|CEK|Scheduled|SU9|||
|369|PG0273|2017-08-31 11:50:00.000 +0500|2017-08-31 13:50:00.000 +0500|DME|CEK|Scheduled|SU9|||
|370|PG0273|2017-07-16 11:50:00.000 +0500|2017-07-16 13:50:00.000 +0500|DME|CEK|Arrived|SU9|2017-07-16 11:53:00.000 +0500|2017-07-16 13:54:00.000 +0500|
|371|PG0273|2017-08-12 11:50:00.000 +0500|2017-08-12 13:50:00.000 +0500|DME|CEK|Arrived|SU9|2017-08-12 11:52:00.000 +0500|2017-08-12 13:51:00.000 +0500|
|372|PG0273|2017-08-13 11:50:00.000 +0500|2017-08-13 13:50:00.000 +0500|DME|CEK|Arrived|SU9|2017-08-13 11:52:00.000 +0500|2017-08-13 13:53:00.000 +0500|
|373|PG0273|2017-09-01 11:50:00.000 +0500|2017-09-01 13:50:00.000 +0500|DME|CEK|Scheduled|SU9|||
|374|PG0273|2017-08-11 11:50:00.000 +0500|2017-08-11 13:50:00.000 +0500|DME|CEK|Arrived|SU9|2017-08-11 11:51:00.000 +0500|2017-08-11 13:50:00.000 +0500|
|375|PG0273|2017-08-14 11:50:00.000 +0500|2017-08-14 13:50:00.000 +0500|DME|CEK|Arrived|SU9|2017-08-14 11:52:00.000 +0500|2017-08-14 13:53:00.000 +0500|


### Задача 1.4.1: Найдите все билеты, забронированные пассажирами с фамилией Иванов и с email в контактной информации.

**SQL-запрос:**
```sql
select * from bookings.tickets where passenger_name like '%IVANOV' and contact_data ->> 'email' is not null;
```

**Результат. Первые 10 значений**
|ticket_no|book_ref|passenger_id|passenger_name|contact_data|
|---------|--------|------------|--------------|------------|
|0005432279258|44BF89|4901 983785|VYACHESLAV IVANOV|{"email": "v-ivanov.1965@postgrespro.ru", "phone": "+70941103649"}|
|0005432282834|082A9E|1404 456057|YURIY IVANOV|{"email": "y-ivanov_1976@postgrespro.ru", "phone": "+70284446857"}|
|0005432282867|ED1FEF|0262 284640|EVGENIY IVANOV|{"email": "ivanov-e.1987@postgrespro.ru", "phone": "+70780129753"}|
|0005432282931|266D27|4374 775579|ARTEM IVANOV|{"email": "artem-ivanov_101980@postgrespro.ru", "phone": "+70312623070"}|
|0005432282985|FCFF06|5344 368876|ALEKSANDR IVANOV|{"email": "aivanov.22071960@postgrespro.ru", "phone": "+70356983659"}|
|0005432283016|F6F0C2|6357 876625|ALEKSANDR IVANOV|{"email": "aivanov-02051983@postgrespro.ru", "phone": "+70351492765"}|
|0005432283025|537A49|6161 255459|SERGEY IVANOV|{"email": "ivanov.sergey-1968@postgrespro.ru", "phone": "+70859064647"}|
|0005432283056|A888EE|2831 721378|ROMAN IVANOV|{"email": "ivanov.roman_021960@postgrespro.ru", "phone": "+70752259483"}|
|0005432283064|86F32A|2482 029533|NIKOLAY IVANOV|{"email": "n.ivanov.18051983@postgrespro.ru", "phone": "+70081357517"}|
|0005432284634|C43B46|6713 475047|VYACHESLAV IVANOV|{"email": "ivanov-vyacheslav30081972@postgrespro.ru", "phone": "+70181287593"}|


### Задача 1.4.2: Найдите все билеты, забронированные пассажирами с фамилией Иванов и с датой бронирования меньше чем 2017-09-13.

**SQL-запрос:**
```sql
select * from bookings.tickets join bookings.bookings on bookings.tickets.book_ref = bookings.bookings.book_ref where bookings.tickets.passenger_name like '%IVANOV' and bookings.bookings.book_date < '2017-09-13';
```

**Результат. Первые 10 значений**
|ticket_no|book_ref|passenger_id|passenger_name|contact_data|book_ref|book_date|total_amount|
|---------|--------|------------|--------------|------------|--------|---------|------------|
|0005432706449|0009ED|7478 666965|VYACHESLAV IVANOV|{"email": "v_ivanov-1968@postgrespro.ru", "phone": "+70033317400"}|0009ED|2017-07-08 16:25:00.000 +0500|14000.00|
|0005433634101|000B97|6889 375514|ANDREY IVANOV|{"email": "andrey-ivanov.091973@postgrespro.ru", "phone": "+70497820699"}|000B97|2017-07-20 22:32:00.000 +0500|101200.00|
|0005432604383|001A23|9462 048671|ILYA IVANOV|{"phone": "+70841115590"}|001A23|2017-07-18 05:30:00.000 +0500|87100.00|
|0005434756896|002752|4031 563403|VALERIY IVANOV|{"email": "vivanov.011980@postgrespro.ru", "phone": "+70795153487"}|002752|2017-07-29 05:51:00.000 +0500|147100.00|
|0005432862367|0029AE|7830 262533|SERGEY IVANOV|{"phone": "+70009235667"}|0029AE|2017-07-23 17:13:00.000 +0500|24800.00|
|0005435177139|003096|7217 379164|VALERIY IVANOV|{"email": "v_ivanov-27091971@postgrespro.ru", "phone": "+70334536370"}|003096|2017-07-24 01:55:00.000 +0500|73200.00|
|0005433650613|003389|5385 743818|MAKSIM IVANOV|{"email": "mivanov-24071977@postgrespro.ru", "phone": "+70739799398"}|003389|2017-07-15 17:19:00.000 +0500|122600.00|
|0005434980410|0035EC|9657 995414|MIKHAIL IVANOV|{"phone": "+70717355545"}|0035EC|2017-08-05 21:38:00.000 +0500|52400.00|
|0005435611741|004C98|7829 119365|DENIS IVANOV|{"email": "d.ivanov101983@postgrespro.ru", "phone": "+70118436234"}|004C98|2017-07-06 19:27:00.000 +0500|57600.00|
|0005433845678|005C0B|6968 556049|MIKHAIL IVANOV|{"phone": "+70919370328"}|005C0B|2017-07-06 03:25:00.000 +0500|269600.00|


## Блок 2. Обновление данных

### Задача 2.1: Cдвиньте время вылета всех рейсов вылетающих ночью с 2017-09-11/2017-09-12 (вылетающих между 23:00 и 06:00) на 30 минут позже.

**SQL-запрос:**
```sql
update bookings.flights set scheduled_departure = scheduled_departure + interval '30 minutes', scheduled_arrival = scheduled_arrival + interval '30 minutes' where scheduled_departure between '2017-09-11 23:00:00' and '2017-09-12 06:00:00';
```


### Задача 2.2: Увеличьте стоимость всех билетов бизнес-класса, которые дешевле чем 10000, на 10%.

**SQL-запрос:**
```sql
update bookings.ticket_flights set amount = amount * 1.10 where fare_conditions = 'Business' and amount < 10000;
```


### Задача 2.3: Увеличьте стоимость билетов на рейсы, запланированные на пиковые часы (с 7:00 до 10:00 и с 17:00 до 20:00) на 1000. 

**SQL-запрос:**
```sql
update bookings.ticket_flights set amount = amount + 1000 where flight_id in (
    select flight_id from bookings.flights where (extract(hour from scheduled_departure) between 7 and 9) or (extract(hour from scheduled_departure) between 17 and 19)
);
```


### Задача 2.4: Обновите статус всех рейсов со статусом 'Delayed' на 'Сanceled' и вылетающих из SVO. 

**SQL-запрос:**
```sql
update bookings.flights set status = 'Cancelled' where status = 'Delayed' and departure_airport = 'SVO';
```


## Блок 3. Запросы с группировкой

### Задача 3.1: Рассчитайте количество рейсов и среднюю продолжительность полета для каждого аэропорта вылета.

**SQL-запрос:**
```sql
select
    departure_airport,
    count(*) as flights_count,
    avg(actual_arrival - actual_departure) as average_duration
from bookings.flights where status = 'Arrived' group by departure_airport order by departure_airport;
```

**Результат. Первые 10 строк**
|departure_airport|flights_count|average_duration|
|-----------------|-------------|----------------|
|AAQ|67|01:31:56.41791|
|ABA|119|02:07:12.605042|
|AER|292|02:11:54.863014|
|ARH|163|02:31:01.472393|
|ASF|75|02:20:16.8|
|BAX|79|03:12:34.177215|
|BQS|31|00:50:03.870968|
|BTK|31|05:04:32.903226|
|BZK|306|00:28:00|
|CEE|67|01:36:38.507463|


### Задача 3.2: Проанализируйте распределение вылетов по часам суток из аэропорта DME.

**SQL-запрос:**
```sql
select
    extract(hour from scheduled_departure) as departure_hour,
    count(*) as flights_count
from bookings.flights where departure_airport = 'DME' group by departure_hour order by departure_hour;
```

**Результат**
|departure_hour|flights_count|
|--------------|-------------|
|11|480|
|12|314|
|13|436|
|14|244|
|15|122|
|16|270|
|17|331|
|18|183|
|19|157|
|20|305|
|21|244|
|22|131|


### Задача 3.3: Для каждой timezone найдите широту самого северный аэропорт.

**SQL-запрос:**
```sql
select
    timezone,
    max(coordinates[1]) as northest
from bookings.airports_data group by timezone order by northest desc;
```

**Результат. Первые 10 строк**
|timezone|north|
|--------|-----|
|Asia/Krasnoyarsk|69.31109619140625|
|Europe/Moscow|68.78170013427734|
|Asia/Yekaterinburg|66.5907974243164|
|Asia/Yakutsk|66.4003982544|
|Asia/Anadyr|64.73490142822266|
|Asia/Magadan|59.9109992980957|
|Asia/Irkutsk|58.13610076904297|
|Europe/Samara|56.82809829711914|
|Asia/Novokuznetsk|55.27009963989258|
|Asia/Novosibirsk|55.012599945068|


### Задача 3.4: Для каждой даты в июнях найдите колличество рейсов со статусом 'Delayed'

**SQL-запрос:**
```sql
select 
	scheduled_departure::date as flight_date,
    count(*) as delayed_flights_count
from bookings.flights where status = 'Delayed' and extract(month from scheduled_departure) = 6 group by flight_date order by flight_date;
```

**Результат нулевой**


