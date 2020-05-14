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




















