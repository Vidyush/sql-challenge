1.select * from city where countrycode='USA' and population>100000;
2. select name from city where countrycode='USA';
3. ****describe city;
4. select * from city where id=1661;
5. select * from city where countrycode='JPN';
6. select name from city where countrycode='JPN';
7. select city,state from station;
8. select distinct city from station where id%2=0;
9. select count(city) - count(distinct city) as difference from station;
10. select city, char_length(city) as length from station order by length, city limit1;                                                     select city, char_length(city) as length from station order by length desc, city limit 1;
11. select distinct city from station where city regexp '^[aeiou]';
12. select distinct city from station where city regexp '[aeiou]$';
13. select distinct city from station where city regexp '^[^aeiou]';
14. select distinct city from station where city regexp '[^aeiou]$';
15. select distinct city from station where city regexp '^[^aeiou] or city regexp[^aeiou]$';
16. select distinct city from station where city regexp '^[^aeiou].*[^aeiou]$';
17. select distinct product.product_id, product.product_name
from product join sales
on product.product_id = sales.product_id
where sales.sale_date between '2019-01-01' and '2019-03-31'
and product.product_id not in (select distinct sales.product_id from sales where sale_date <  '2019-01-01' or sale_date> '2019-03-31') ;
18.  select distinct a.author_id as id from views a 
join views b
on a.author_id = b.author_id
where a.author_id = b.viewer_id
order by a.author_id;

19. select round(100*d2.immediate_orders/count(d1.delivery_id),2) as immediate_percentage
from Delivery d1,
    (select count(order_date) as immediate_orders
    from Delivery 
    where (order_date = customer_pref_delivery_date)) d2;

20. ????????????

21. select empID, count(team_id) over(partition by team_id) as team_size from EMPLOYEE order by empID;

22. select ctr.country_name, case 
								when avg(wt.weather_state<=15) then 'Cold'
								when avg(wt.weather_state>=25) then 'Hot'
								else 'Warm'
								end as weather_type
							from countries ctr join weather wt on ctr.country_id=wt.country_id
							where month(wt.day) = 11 group by ctr.country_name;

23. with cte as (select pr.product_id,pr.price,sl.units,sl.units*pr.price as total_price
from Prices pr join UnitsSold sl
on pr.product_id=sl.product_id
where sl.purchase_date between pr.start_date and pr.end_date)
select product_id, round(sum(total_price)/sum(units), 2) as average_price
from cte group by product_id;

24. with cte as (select player_id, event_date, row_number() over(partition by player_id order by event_date) as row_num from Activity) select player_id, event_date as first_login from cte where row_num=1;

25. with cte as(select player_id, device_id, row_number() over (partition by player_id order event_date) as row_num from Activity) select player_id, device_id from cte where row_num=1;

26. select distinct pr.product_name,sum(o.unit) over (partition by pr.product_id) as unit from products pr join orders o
on pr.product_id = o.product_id where unit>=100 and o.order_date like '2020-02%'; 

27. SELECT *
FROM Users
WHERE REGEXP_LIKE(mail, '^[a-zA-Z][a-zA-Z0-9\_\.\-]*@leetcode.com')

28. select o.customer_id, name
from
Orders o
join
Product p
on o.product_id = p.product_id
join
Customers c
on o.customer_id = c.customer_id
group by 1, 2
having sum(case when date_format(order_date, '%Y-%m')='2020-06'
then price*quantity end) >= 100
and 
sum(case when date_format(order_date, '%Y-%m')='2020-07'
then price*quantity end) >= 100

29. select distinct cont.title from content cont
	join TVProgram tv
	on cont.content_id = tv.content_id
	where cont.Kids_content = 'Y' and cont.content_type = 'Movies' and program_date like '2020-06';

46. select customer_id from customer group by customer_id having count(distinct product_key) = (select count(*) from product);


47. with cte as (select pr.project_id, pr.employee_id,dense_rank() over(partition by pr.project_id order by emp.experience_years desc) as row_num from Project pr join Employee emp on pr.employee_id=emp.employee_id)
select project_id, employee_id from cte where row_num=1;

49. with cte as (select student_id, course_id, grade, dense_rank() over (partition by student_id order by grade desc, course_id) as row_num) select student_id, course_id, grade from cte where row_num=1;
