# sql-ex

##### #1. Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd
SELECT model, speed, hd FROM PC WHERE price < 500
##### #2. Найдите производителей принтеров. Вывести: maker
SELECT DISTINCT maker FROM product WHERE type = 'Printer'
##### #3. Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.
SELECT DISTINCT model, ram, screen FROM Laptop WHERE price > 1000
##### #4. Найдите все записи таблицы Printer для цветных принтеров.
SELECT * FROM Printer WHERE color = 'y'
##### #5.Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.
SELECT model, speed, hd FROM PC WHERE (cd = '12x' or cd = '24x') and price < 600
##### #6. Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость.
select distinct product.maker as maker_1, laptop.speed as speed_1 from product inner join laptop on product.model = laptop.model where hd >= 10 order by laptop.speed
##### #7. Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).
select pc.model, pc.price from pc join product on pc.model = product.model where maker ='B'
union
select laptop.model, laptop.price from laptop join product on laptop.model = product.model where maker = 'B'
union
select printer.model, printer.price from printer join product on printer.model = product.model where maker = 'B'
##### #8. Найдите производителя, выпускающего ПК, но не ПК-блокноты.
select distinct maker from product where type in ('PC' )
EXCEPT
select distinct maker from product where type in ('Laptop')
##### #9. Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker
select distinct maker from product inner join pc on pc.model = product.model where speed >= 450
##### #10. Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price
Select model, price from printer where price in (select max(price) from printer)
##### #11. Найдите среднюю скорость ПК.
Select avg (speed) from pc
##### #12. Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.
Select avg (speed) from laptop where price > 1000
##### #13. Найдите среднюю скорость ПК, выпущенных производителем A.
Select avg (speed) from pc inner join product on pc.model = product.model
 where maker = 'A'
##### #14. Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий.
Select distinct cl.class, sh.name, cl.country from classes as cl right join ships as sh on cl.class = sh.class where cl.numGuns >= 10
group by 
cl.class, sh.name, cl.country
##### #15. Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD
select hd from pc group by hd having count (model) >= 2
##### #16. *not ok* Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.
1) Select distinct max(model) as model, min(model) as model,  speed, ram from pc group by ram, speed
having max(model) > min(model)
2) Select distinct max(a.model) as maxmodel, min(b.model) as minmodel,  a.speed ,b.speed, a.ram, b.ram from pc as a, pc b where a.speed=b.speed group by a.ram, b.ram, a.speed, b.speed having max(a.model)>min(b.model)
3) select max(model), min(model), pc.speed, ram from pc, (select max(model), min(model), speed, ram from pc) as apc where pc.speed = apc.speed having max(model)>min(model)
##### #17. Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК. Вывести: type, model, speed
select distinct product.type, laptop.model, laptop.speed from laptop, product where laptop.speed < all (select  speed from pc) and product.type = 'laptop'
##### #18. Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price
SELECT distinct c.maker, a.priceA price
FROM (SELECT distinct MIN(price) priceA
      FROM Printer
      WHERE color ='y'
      ) a INNER JOIN
      Printer b ON a.priceA = b.price INNER JOIN
      Product c ON b.model = c.model
##### #19. Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов. Вывести: maker, средний размер экрана.
select distinct maker,  avg(screen) from product inner join laptop on product.model = laptop.model
group by maker
##### #20. Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК.
select distinct maker, count(model) as count_model from product where type = 'pc'
group by maker
having count(model) >= 3
##### #21. Найдите максимальную цену ПК, выпускаемых каждым производителем, у которого есть модели в таблице PC. Вывести: maker, максимальная цена.
Select distinct maker, max(price) as maxprice from product inner join pc on product.model=pc.model
Group by maker
##### #22. Для каждого значения скорости ПК, превышающего 600 МГц, определите среднюю цену ПК с такой же скоростью. Вывести: speed, средняя цена.
Select speed, avg(price) as price from pc where speed > 600
Group by speed
##### #23. Найдите производителей, которые производили бы как ПК со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц. Вывести: Maker
Select maker from product inner join pc on product.model = pc.model where speed >= 750
Intersect
Select maker from product inner join laptop on product.model = laptop.model where speed >= 750
Group by maker
##### #24. Перечислите номера моделей любых типов, имеющих самую высокую цену по всей имеющейся в базе данных продукции.
select top 1 with ties model from
(select l.model, l.price
from laptop l
union 
select p.model, p.price
from printer p
union
select pc.model, pc.price
from pc ) op
order by price desc
##### #25. *OK но не прошло проверку* Найдите производителей принтеров, которые производят ПК с наименьшим объемом RAM и с самым быстрым процессором среди всех ПК, имеющих наименьший объем RAM. Вывести: Maker
select distinct p.maker
from
product p
join 
(select max(speed), model from pc where ram in(
select min(ram) from pc) group by pc.model) pc
on p.model = pc.model
where p.type in ('Printer', 'PC')
##### #26. 
##### #27. *not OK* Найдите средний размер диска ПК каждого из тех производителей, которые выпускают и принтеры. Вывести: maker, средний размер HD.
1) select maker, avg(pc.hd) as avg_hd from product inner join pc on product.model=pc.model inner join printer on pc.model=printer.model
where pc.model in (select model from printer)
group by maker

2) пример
SELECT Result.maker, AVG(result.hd)
FROM (SELECT PC.hd, Product.maker
      FROM PC INNER JOIN
           Product ON PC.model = Product.model
      WHERE Product.maker IN (SELECT Product1.maker
                              FROM Product Product1  INNER JOIN
                                   Printer ON Product1.model = Printer.model
                              GROUP BY Product1.maker
                              )
      ) AS result
GROUP BY result.maker;
##### #28. Используя таблицу Product, определить количество производителей, выпускающих по одной модели.
select count(maker) from product where maker in
(select maker from product
group by maker 
having count(model) = 1)
##### #29. В предположении, что приход и расход денег на каждом пункте приема фиксируется не чаще одного раза в день [т.е. первичный ключ (пункт, дата)], написать запрос с выходными данными (пункт, дата, приход, расход). Использовать таблицы Income_o и Outcome_o.
select outcome_o.point, outcome_o.date, income_o.inc, outcome_o.out from outcome_o left join income_o on outcome_o.point = income_o.point and outcome_o.date = income_o.date
union
select income_o.point, income_o.date, income_o.inc, outcome_o.out from income_o left join outcome_o on income_o.point=outcome_o.point and income_o.date = outcome_o.date
##### #30. В предположении, что приход и расход денег на каждом пункте приема фиксируется произвольное число раз (первичным ключом в таблицах является столбец code), требуется получить таблицу, в которой каждому пункту за каждую дату выполнения операций будет соответствовать одна строка. Вывод: point, date, суммарный расход пункта за день (out), суммарный приход пункта за день (inc). Отсутствующие значения считать неопределенными (NULL).
select point, date, SUM(sum_out), SUM(sum_inc) 
from( select point, date, SUM(inc) as sum_inc, null as sum_out from Income Group by point, date  
Union 
select point, date, null as sum_inc, SUM(out) as sum_out from Outcome Group by point, date ) as t  
group by point, date order by point
##### #31. Для классов кораблей, калибр орудий которых не менее 16 дюймов, укажите класс и страну.
Select class, country from classes where bore >= 16.0
##### #32. 
##### #33. Укажите корабли, потопленные в сражениях в Северной Атлантике (North Atlantic). Вывод: ship.
Select distinct ship from outcomes where battle = 'North Atlantic' and result = 'sunk'
##### #34. По Вашингтонскому международному договору от начала 1922 г. запрещалось строить линейные корабли водоизмещением более 35 тыс.тонн. Укажите корабли, нарушившие этот договор (учитывать только корабли c известным годом спуска на воду). Вывести названия кораблей.
Select name  from classes,ships where launched>=1922 and displacement>35000 and type='bb' and    
ships.class = classes.class
##### #35. В таблице Product найти модели, которые состоят только из цифр или только из латинских букв (A-Z, без учета регистра). Вывод: номер модели, тип модели.
select distinct model, type from product where model  not like '%[^0-9]%' or model not
 like '%[^A-Z]%'
##### #36. Перечислите названия головных кораблей, имеющихся в базе данных (учесть корабли в Outcomes).
1) Select name from ships where class = name 
union 
select ship as name from classes,outcomes where classes.class = outcomes.ship
2) select name from ships
inner join classes on ships.name = classes.class
union all
select ship as name from outcomes
inner join classes on outcomes.ship = classes.class
where result = 'ok'
order by 1
##### #37. Найдите классы, в которые входит только один корабль из базы данных (учесть также корабли в Outcomes).
select classes.class from classes left join (select ships.class, ships.name from ships
union 
select outcomes.ship, outcomes.ship from outcomes) as s on classes.class = s.class 
group by classes.class
having count (s.name) = 1
##### #38. Найдите страны, имевшие когда-либо классы обычных боевых кораблей ('bb') и имевшие когда-либо классы крейсеров ('bc').
select country from classes where type= 'bb' 
intersect 
select country from classes where type= 'bc'
##### #39. Найдите корабли, `сохранившиеся для будущих сражений`; т.е. выведенные из строя в одной битве (damaged), они участвовали в другой, произошедшей позже.
select distinct outcomes.ship from outcomes left join battles as b on outcomes.battle = b.name where outcomes.result = 'damaged' and 
exists
(select outcomes.ship from  outcomes as ou left join battles as ba on ou.battle=ba.name  where ou.ship = outcomes.ship  and b.date < ba.date  
)
##### #40. Найти производителей, которые выпускают более одной модели, при этом все выпускаемые производителем модели являются продуктами одного типа. Вывести: maker, type
Select distinct cl.class, sh.name, cl.country from classes as cl right join ships as sh on cl.class = sh.class where cl.numGuns >= 10
group by
cl.class, sh.name, cl.country
##### #41. *OK не прошло проверное тестирование* Для каждого производителя, у которого присутствуют модели хотя бы в одной из таблиц PC, Laptop или Printer, определить максимальную цену на его продукцию. Вывод: имя производителя, если среди цен на продукцию данного производителя присутствует NULL, то выводить для этого производителя NULL, иначе максимальную цену
select distinct maker, case s.price
when NULL
then'NULL'
else max(s.price)
end 'max_price'
from product left join 
(select model, price from pc
union
select model, price from laptop
union
select model, price from printer) s
on product.model = s.model
group by maker
##### #42. Найдите названия кораблей, потопленных в сражениях, и название сражения, в котором они были потоплены.
Select ship, battle from outcomes where result = 'sunk'
##### #43. Укажите сражения, которые произошли в годы, не совпадающие ни с одним из годов спуска кораблей на воду
select distinct name  as years from battles where year(date) not in (select launched  from ships where launched is not null)
##### #44. Найдите названия всех кораблей в базе данных, начинающихся с буквы R.
Select distinct name from ships where name like 'R%'
union
Select distinct ship from outcomes where ship like 'R%'
##### #45. Найдите названия всех кораблей в базе данных, состоящие из трех и более слов (например, King George V). Считать, что слова в названиях разделяются единичными пробелами, и нет концевых пробелов.
Select name from ships where name like '% % %'
union 
select ship from outcomes where ship like '% % %'
##### #46. Для каждого корабля, участвовавшего в сражении при Гвадалканале (Guadalcanal), вывести название, водоизмещение и число орудий.
select name as n, displacement as d, numguns as ng 
from ships inner join classes on ships.class=classes.class 
where name in (select ship from outcomes where battle = 'Guadalcanal')   
union 
select ship as n, displacement as d, numguns as ng 
from outcomes inner join classes on outcomes.ship=classes.class 
where battle = 'Guadalcanal' and ship not in (select name from ships)   
union  
select ship as n, null as d, null as ng from outcomes 
where battle = 'Guadalcanal' and ship not in (select name from ships) 
and ship not in  (select class from classes)
##### #47. Определить страны, которые потеряли в сражениях все свои корабли.
1) МОЙ ВАРИАНТ. ОК, НО НЕ ПРИНЯТ ПРОВЕРОЧНОЙ СИСТЕМОЙ
SELECT country from classes where class in(
select s.name as ship from CLASSES C inner JOIN SHIPS S ON C.CLASS = S.CLASS
where s.name in (select distinct ship from classes c inner join outcomes o on o.ship = c.class where result like 'sunk')
union
select ship from classes c inner join outcomes o on o.ship = c.class 
where result like 'sunk'
)

2) НЕ МОЙ ВАРИАНТ:
WITH T1 AS ( SELECT COUNT(name) as co, country FROM
(SELECT name, country FROM Classes INNER JOIN Ships ON Ships.class = Classes.class
UNION
SELECT ship, country FROM Classes INNER JOIN Outcomes ON Outcomes.ship = Classes.class) FR1
GROUP BY country
),

T2 AS ( SELECT COUNT(name) as co, country FROM ( SELECT name, country FROM Classes INNER JOIN Ships ON Ships.class = Classes.class
WHERE name IN (SELECT DISTINCT ship FROM Outcomes WHERE result LIKE 'sunk')
UNION
SELECT ship, country FROM Classes INNER JOIN Outcomes ON Outcomes.ship = Classes.class
WHERE ship IN (SELECT DISTINCT ship FROM Outcomes WHERE result LIKE 'sunk')
) FR2 GROUP BY country )

SELECT T1.country FROM T1
INNER JOIN T2 ON T1.co = t2.co and t1.country = t2.country
##### #48. Найдите классы кораблей, в которых хотя бы один корабль был потоплен в сражении.
Select class as n from ships where name in(select ship from outcomes where result='sunk')   
union  
Select ship as n from outcomes  
where ship not in(Select name from ships) and ship in(Select class from classes) and result='sunk'
##### #49. Найдите названия кораблей с орудиями калибра 16 дюймов (учесть корабли из таблицы Outcomes).
Select name as n from ships where class in  (select class from classes where bore = 16)
union
select ship as n from outcomes where ship not in (Select name from ships) and ship in (select class from classes where bore = 16)
##### #50. Найдите сражения, в которых участвовали корабли класса Kongo из таблицы Ships.
select battle from outcomes o join 
ships s on
s.name = o.ship
where upper(s.class) = 'KONGO'
group by battle
##### #51. Найдите названия кораблей, имеющих наибольшее число орудий среди всех имеющихся кораблей такого же водоизмещения (учесть корабли из таблицы Outcomes).
select NAME from(select name as NAME, displacement, numguns 
from ships inner join classes on ships.class = classes.class
union
select ship as NAME, displacement, numguns from outcomes inner join classes on outcomes.ship= classes.class) as d1 inner join (select displacement, max(numGuns) as numguns from ( select displacement, numguns from ships inner join classes on ships.class = classes.class 
union
select displacement, numguns  from outcomes inner join classes on outcomes.ship= classes.class) as f
group by displacement) as d2 on d1.displacement=d2.displacement and d1.numguns =d2.numguns
##### #52. Определить названия всех кораблей из таблицы Ships, которые могут быть линейным японским кораблем, имеющим число главных орудий не менее девяти, калибр орудий менее 19 дюймов и водоизмещение не более 65 тыс.тонн
select distinct s.name 
from
ships s, classes c
where 
s.class = c.class and c.type = 'bb' and upper(c.country) = 'JAPAN' and (c.numGuns >=9 or c.numGuns is Null) and (c.bore < 19 or c.bore is null) and (c.displacement <= 65000 or c.displacement is NULL)
##### #53. Определите среднее число орудий для классов линейных кораблей. Получить результат с точностью до 2-х десятичных знаков.
Select cast(avg(numguns*1.0) as numeric(4,2)) from classes where type = 'bb'
##### #54. С точностью до 2-х десятичных знаков определите среднее число орудий всех линейных кораблей (учесть корабли из таблицы Outcomes).
with num as(select ship, type, numguns from classes
join outcomes on
classes.class = outcomes.ship
union
select name, type, numguns from classes
join ships on
classes.class = ships.class)
select cast(avg(numguns*1.0) as numeric(4,2)) as avg_num from num where type  ='bb'
##### #55. *OK но сайт не принял* Для каждого класса определите год, когда был спущен на воду первый корабль этого класса. Если год спуска на воду головного корабля неизвестен, определите минимальный год спуска на воду кораблей этого класса. Вывести: класс, год.
with summ as ((select  class, min(launched) as n from ships group by class)
union
(select class, NULL as n from classes where class not in (select name from ships) group by class))
Select * from summ
##### #56. Для каждого класса определите число кораблей этого класса, потопленных в сражениях. Вывести: класс и число потопленных кораблей.

##### #57. Для классов, имеющих потери в виде потопленных кораблей и не менее 3 кораблей в базе данных, вывести имя класса и число потопленных кораблей.

##### #58. Для каждого типа продукции и каждого производителя из таблицы Product c точностью до двух десятичных знаков найти процентное отношение числа моделей данного типа данного производителя к общему числу моделей этого производителя. Вывод: maker, type, процентное отношение числа моделей данного типа к общему числу моделей производителя

##### #59. Посчитать остаток денежных средств на каждом пункте приема для базы данных с отчетностью не чаще одного раза в день. Вывод: пункт, остаток.
1)OK но сайт не принял
select dd.point , (ISNULL(ss.inc, 0) - ISNULL(dd.out, 0) ) as remain
from (select point, sum(out) as out from outcome_o group by point) as dd,
(select point, sum(inc) as inc from income_o group by point) as ss
where ss.point = dd.point
2) не мой вариант
SELECT c1, c2-
(CASE
WHEN o2 is null THEN 0
ELSE o2
END)
from
(SELECT point c1, sum(inc) c2 FROM income_o
group by point) as t1
left join
(SELECT point o1, sum(out) o2 FROM outcome_o
group by point) as t2
on c1=o1
##### #60. Посчитать остаток денежных средств на начало дня 15/04/01 на каждом пункте приема для базы данных с отчетностью не чаще одного раза в день. Вывод: пункт, остаток. Замечание. Не учитывать пункты, информации о которых нет до указанной даты.
Select point, sum(inc)-isnull((select sum(out) from Outcome_o where point=a.point and date < CONVERT(datetime, '15-04-2001', 105)),0) from Income_o as a
where date < CONVERT(datetime, '15-04-2001', 105)
group by point
##### #61. Посчитать остаток денежных средств на всех пунктах приема для базы данных с отчетностью не чаще одного раза в день.
Select (ISNULL(ss.inc, 0) - ISNULL(dd.out, 0) ) as ost from
(select sum(inc) as inc from income_o) as ss,
(select sum(out) as out from outcome_o) as dd
##### #62. Посчитать остаток денежных средств на всех пунктах приема на начало дня 15/04/01 для базы данных с отчетностью не чаще одного раза в день.
Select (ISNULL(ss.inc, 0) - ISNULL(dd.out, 0)) as ost from
(select sum(inc) as inc from income_o where '2001-04-15' > date) as ss,
(select sum(out) as out from outcome_o where '2001-04-15' > date) as dd
##### #63. Определить имена разных пассажиров, когда-либо летевших на одном и том же месте более одного раза.
select name from passenger where id_psg in (select id_psg from pass_in_trip
group by place, id_psg
having count(*)>1)
##### #64. Используя таблицы Income и Outcome, для каждого пункта приема определить дни, когда был приход, но не было расхода и наоборот. Вывод: пункт, дата, тип операции (inc/out), денежная сумма за день.

##### #65. Пронумеровать уникальные пары {maker, type} из Product, упорядочив их следующим образом: - имя производителя (maker) по возрастанию; - тип продукта (type) в порядке PC, Laptop, Printer. Если некий производитель выпускает несколько типов продукции, то выводить его имя только в первой строке; остальные строки для ЭТОГО производителя должны содержать пустую строку символов ('').
*OK, НО СПИСАНО ПОЛНОСТЬЮ*
select row_number() over(order by maker) num, maker2 maker, type from (
select maker, maker2 = 
case row_number() over (partition by maker order by case
when type = 'PC' then 1 
when type = 'Laptop' then 2
when type = 'Printer' then 3
else 4 end)
when 1 then maker else '' end, type
from (select distinct maker, type from product) x) y
##### #66. Для всех дней в интервале с 01/04/2003 по 07/04/2003 определить число рейсов из Rostov. Вывод: дата, количество рейсов

##### #67. Найти количество маршрутов, которые обслуживаются наибольшим числом рейсов. Замечания. 1) A - B и B - A считать РАЗНЫМИ маршрутами. 2) Использовать только таблицу Trip
select count (*) from
(select top 1 with ties count(*) as s , town_to, town_from from trip
group by town_to, town_from
order by s desc) as t
##### #68. Найти количество маршрутов, которые обслуживаются наибольшим числом рейсов. Замечания. 1) A - B и B - A считать ОДНИМ И ТЕМ ЖЕ маршрутом. 2) Использовать только таблицу Trip

##### #69. По таблицам Income и Outcome для каждого пункта приема найти остатки денежных средств на конец каждого дня, в который выполнялись операции по приходу и/или расходу на данном пункте. Учесть при этом, что деньги не изымаются, а остатки/задолженность переходят на следующий день. Вывод: пункт приема, день в формате "dd/mm/yyyy", остатки/задолженность на конец этого дня.
*OK РЕШЕНИЕ НЕ МОЕ, СПИСАНО*
with t as (
select point, "date", inc, 0 "out"
from income
union all
select point, "date", 0 inc, "out"
from outcome
)
select point,
   convert(char(10),"date",103) "date", 
   (select sum(inc) - sum("out")
    from t
    where "date" <= p."date"
    and point = p.point) rest
from t p
group by point, "date"
order by point, "date"
##### #70. Укажите сражения, в которых участвовало по меньшей мере три корабля одной и той же страны.
*ОК  НО НЕ ПРИНИМАЕТСЯ*
select distinct o.battle
from outcomes o join ships s
on o.ship = s.name
join classes c
on c.class = s.class
group by o.battle, c.country
having count(country) > 2
##### #71. Найти тех производителей ПК, все модели ПК которых имеются в таблице PC.
*OK ЧАСТИЧНО СПИСАНО*
select  p1.maker from product p1
left join pc p2
on p1.model = p2.model
where p1.type = 'pc'
group by p1.maker
having count(p1.model) = count(p2.model)
##### #72. Среди тех, кто пользуется услугами только какой-нибудь одной компании, определить имена разных пассажиров, летавших чаще других. Вывести: имя пассажира и число полетов.
1) *OK НО СИСТЕМА НЕ ПРИНИМАЕТ РЕШЕНИЕ*
select top 1 with ties  p.name, count(*) as c  from pass_in_trip pit
left join passenger p on
p.id_psg = pit.id_psg
left join trip t on
t.trip_no = pit.trip_no
group by p.name
order by c desc
2) *OK*
select TOP 1 WITH TIES name, c3 from passenger
join
(select c1, max(c3) c3 from
(
select pass_in_trip.ID_psg c1, Trip.ID_comp c2, count(*) c3 from pass_in_trip
join trip on trip.trip_no=pass_in_trip.trip_no
group by pass_in_trip.ID_psg, Trip.ID_comp
) as t
group by c1
having count(*)=1) as tt
on ID_psg=c1
order by c3 desc
##### #73. Для каждой страны определить сражения, в которых не участвовали корабли данной страны. Вывод: страна, сражение

##### #74. Вывести классы всех кораблей России (Russia). Если в базе данных нет классов кораблей России, вывести классы для всех имеющихся в БД стран. Вывод: страна, класс
*OK НО СИСТЕМА НЕ ПРИНИМАЕТ РЕШЕНИЕ*
select c.country, c.class from classes c
left join ships s on
c.class = s.class
where c.class in(
case when country = 'russia' then c.class
else c.class
end)
group by c.country, c.class
##### #75. Для тех производителей, у которых есть продукты с известной ценой хотя бы в одной из таблиц Laptop, PC, Printer найти максимальные цены на каждый из типов продукции. Вывод: maker, максимальная цена на ноутбуки, максимальная цена на ПК, максимальная цена на принтеры. Для отсутствующих продуктов/цен использовать NULL.

##### #76. Определить время, проведенное в полетах, для пассажиров, летавших всегда на разных местах. Вывод: имя пассажира, время в минутах.

##### #77. Определить дни, когда было выполнено максимальное число рейсов из Ростова ('Rostov'). Вывод: число рейсов, дата.
select  top 1 with ties count(distinct trip_no) , date from pass_in_trip where trip_no in
(select trip_no from trip  where town_from = 'rostov')
group by date
order by count(distinct trip_no) desc
##### #78. Для каждого сражения определить первый и последний день месяца, в котором оно состоялось. Вывод: сражение, первый день месяца, последний день месяца. Замечание: даты представить без времени в формате "yyyy-mm-dd".
select name, convert(date, DATEADD(DAY,1,EOMONTH(date,-1))) as firstD , convert(date, eomonth(date) ) AS lastD from battles
##### #79. Определить пассажиров, которые больше других времени провели в полетах. Вывод: имя пассажира, общее время в минутах, проведенное в полетах

##### #80. Найти производителей компьютерной техники, у которых нет моделей ПК, не представленных в таблице PC.
select maker from product
except
select maker from product where model not in (select model from pc)
and type = 'pc'
##### #81. Из таблицы Outcome получить все записи за тот месяц (месяцы), с учетом года, в котором суммарное значение расхода (out) было максимальным.
SELECT O.*
FROM outcome O
INNER JOIN
(
SELECT TOP 1 WITH TIES YEAR(date) AS Y, MONTH(date) AS M, SUM(out) AS ALL_TOTAL
FROM outcome
GROUP BY YEAR(date), MONTH(date)
ORDER BY ALL_TOTAL DESC
) R ON YEAR(O.date) = R.Y AND MONTH(O.date) = R.M
##### #82. В наборе записей из таблицы PC, отсортированном по столбцу code (по возрастанию) найти среднее значение цены для каждой шестерки подряд идущих ПК. Вывод: значение code, которое является первым в наборе из шести строк, среднее значение цены в наборе.

##### #83. Определить названия всех кораблей из таблицы Ships, которые удовлетворяют, по крайней мере, комбинации любых четырёх критериев из следующего списка: numGuns = 8, bore = 15, displacement = 32000, type = bb, launched = 1915, class=Kongo, country=USA
select  name
from ships s join classes  c on c.class = s.class
where
case when numGuns = 8 then 1 else 0 end +
case when bore = 15 then 1 else 0 end +
case when displacement = 32000  then 1 else 0 end +
case when type = 'bb' then 1 else 0 end +
case when launched = 1915 then 1 else 0 end +
case when s.class='Kongo' then 1 else 0 end +
case when country='USA' then 1 else 0 end >= 4
##### #84. Для каждой компании подсчитать количество перевезенных пассажиров (если они были в этом месяце) по декадам апреля 2003. При этом учитывать только дату вылета. Вывод: название компании, количество пассажиров за каждую декаду
select company.name,
sum((case when day(date) >=  1 and day(date) <= 10 then 1 else 0 end) )"1-10",
sum((case when day(date) >=  11 and day(date) <= 20 then 1 else 0 end) ) "11-20",
sum((case when day(date) >=  21  and day(date) <= 30 then 1 else 0 end)) "21-30"
from pass_in_trip
join trip on trip.trip_no = pass_in_trip.trip_no
join company on company.id_comp = trip.id_comp
where month(date) = 4 and year(date) = 2003
group by company.name















































