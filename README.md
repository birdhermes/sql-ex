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


















