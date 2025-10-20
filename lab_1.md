# Лабораторная работа №1

## Блок 1. Простые запросы с фильтрацией

### Задача 1.1: Найдите все рейсы, задержанные более чем на 2 часа и вылетающие из UFA.

**SQL-запрос:**
```sql
select * from bookings.flights where departure_airport = 'UFA' and (actual_departure - scheduled_departure) > interval '2 hours';
```

**Пояснение: Вычитаем плановое время из фактического, чтобы получить задержку, и фильтруем по ней с помощью interval.**



### Задача 1.2: Найдите все рейсы прибившие в SVO в интервале с 16:00 до 19:00 2017-07-22

**SQL-запрос:**
```sql
select * from bookings.flights where arrival_airport = 'SVO' and actual_arrival between '2017-07-22 16:00:00' and '2017-07-22 19:00:00';
```

**Пояснение: Используем between с полными метками времени для проверки попадания в точный интервал даты и времени.**


### Задача 1.3: Найдите все утренние рейсы (вылетающие с 4:00 утра до 12:00) выполняемые самолетами Сухой Суперджет (код модели 'SU9').

**SQL-запрос:**
```sql
select * from bookings.flights where aircraft_code = 'SU9' and extract(hour from scheduled_departure) between 4 and 12;
```

**Пояснение: Функция extract(hour from ...) извлекает час из полной даты для фильтрации по времени суток.**


### Задача 1.4.1: Найдите все билеты, забронированные пассажирами с фамилией Иванов и с email в контактной информации.

**SQL-запрос:**
```sql
select * from bookings.tickets where passenger_name like '%IVANOV' and contact_data ->> 'email' is not null;
```

**Пояснение: Оператор ->> извлекает значение из поля jsonb как текст, что позволяет проверить наличие email.**


### Задача 1.4.2: Найдите все билеты, забронированные пассажирами с фамилией Иванов и с датой бронирования меньше чем 2017-09-13.

**SQL-запрос:**
```sql
select * from bookings.tickets join bookings.bookings on bookings.tickets.book_ref = bookings.bookings.book_ref where bookings.tickets.passenger_name like '%IVANOV' and bookings.bookings.book_date < '2017-09-13';
```

**Пояснение: join ... on объединяет таблицы билетов и бронирований, чтобы фильтровать по данным из обеих таблиц.**


## Блок 2. Обновление данных

### Задача 2.1: Cдвиньте время вылета всех рейсов вылетающих ночью с 2017-09-11/2017-09-12 (вылетающих между 23:00 и 06:00) на 30 минут позже.

**SQL-запрос:**
```sql
update bookings.flights set scheduled_departure = scheduled_departure + interval '30 minutes', scheduled_arrival = scheduled_arrival + interval '30 minutes' where scheduled_departure between '2017-09-11 23:00:00' and '2017-09-12 06:00:00';
```

**Пояснение: Обновляем сразу два поля времени (departure и arrival), чтобы сдвинуть рейс, сохранив его продолжительность.**


### Задача 2.2: Увеличьте стоимость всех билетов бизнес-класса, которые дешевле чем 10000, на 10%.

**SQL-запрос:**
```sql
update bookings.ticket_flights set amount = amount * 1.10 where fare_conditions = 'Business' and amount < 10000;
```

**Пояснение: Условие where отбирает нужные билеты, а amount * 1.10 увеличивает их стоимость на 10%.**


### Задача 2.3: Увеличьте стоимость билетов на рейсы, запланированные на пиковые часы (с 7:00 до 10:00 и с 17:00 до 20:00) на 1000. 

**SQL-запрос:**
```sql
update bookings.ticket_flights set amount = amount + 1000 where flight_id in (
    select flight_id from bookings.flights where (extract(hour from scheduled_departure) between 7 and 9) or (extract(hour from scheduled_departure) between 17 and 19)
);
```

**Пояснение: Запрос в where ... in (...) находит рейсы в пиковые часы, а основной запрос обновляет стоимость билетов на них.**


### Задача 2.4: Обновите статус всех рейсов со статусом 'Delayed' на 'Сanceled' и вылетающих из SVO. 

**SQL-запрос:**
```sql
update bookings.flights set status = 'Cancelled' where status = 'Delayed' and departure_airport = 'SVO';
```

**Пояснение: Простое обновление поля status с двумя условиями в where для точного отбора нужных рейсов.**


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

**Пояснение: group by группирует рейсы по аэропорту, а count() и avg() считают статистику для каждой группы.**


### Задача 3.2: Проанализируйте распределение вылетов по часам суток из аэропорта DME.

**SQL-запрос:**
```sql
select
    extract(hour from scheduled_departure) as departure_hour,
    count(*) as flights_count
from bookings.flights where departure_airport = 'DME' group by departure_hour order by departure_hour;
```

**Пояснение: extract(hour from ...) извлекает час вылета, по которому затем происходит группировка для подсчета рейсов.**


### Задача 3.3: Для каждой timezone найдите широту самого северный аэропорт.

**SQL-запрос:**
```sql
select
    timezone,
    max(coordinates[1]) as northest
from bookings.airports_data group by timezone order by northest desc;
```

**Пояснение: Обращаемся к широте через индекс coordinates[1] с типом point и находим ее максимум max() для каждой timezone.**


### Задача 3.4: Для каждой даты в июнях найдите колличество рейсов со статусом 'Delayed'

**SQL-запрос:**
```sql
select 
	scheduled_departure::date as flight_date,
    count(*) as delayed_flights_count
from bookings.flights where status = 'Delayed' and extract(month from scheduled_departure) = 6 group by flight_date order by flight_date;
```

**Пояснение: Приведение ::date отбрасывает время, позволяя сгруппировать все рейсы по календарным дням.**


