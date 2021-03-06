# science-project

This is my 7th grade science project on the effect of digital distractions on working adults.

## SQL Queries

Gets all users who have partially completed the test
```
select user_data->>'name' as name, id from userinfo where (select count(id) from questioninfo where userinfo.id = user_id) > 0 and (select count(id) from questioninfo where userinfo.id = user_id) < 15;
```
Gets all users who have not started the test (excluding demo users)
```
select user_data->>'email' as email from userinfo where user_data->>'email' not like 'demo%' and (select count(id) from questioninfo where userinfo.id = user_id) != 15;
```
Gets all users who have completed the test
```
select userinfo.id, userinfo.user_data->>'distraction_amt' as distAmt, userinfo.user_data->>'name' as name, AVG(questioninfo.score_percent) as score, AVG(questioninfo.time_taken) as time from userinfo inner join questioninfo on userinfo.id = questioninfo.user_id group by userinfo.id;
```
Gets the amount of completed users for each distraction amount
```
select user_data->>'distraction_amt' as distAmt, count(distinct id) as count from userinfo where (select count(id) from questioninfo where userinfo.id = user_id) = 15 group by 1;
```
Updates userinfo table
```
update userinfo set user_data = ‘{user data}' where id = {id};
```
Gets the average scores and times for each group
```
select distamt, AVG(score) as score, AVG(time) as time from temp1 group by distamt order by distamt;
```
