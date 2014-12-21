# SQL Movie-Rating Query Exercises (challenge-level)

For each movie, return the title and the 'rating spread', that is, the difference between highest and lowest ratings given to that movie. Sort by rating spread from highest to lowest, then by movie title.
```sql
select movie.title, max(stars)-min(stars) as spread
from rating join movie on rating.mid=movie.mid
group by rating.mid
order by spread desc, title
```

Find the difference between the average rating of movies released before 1980 and the average rating of movies released after 1980. (Make sure to calculate the average rating for each movie, then the average of those averages for movies before 1980 and movies after. Don't just calculate the overall average rating before and after 1980.)
```sql
select max(a1)-min(a1) from
(select avg(av1) a1 from
(select avg(stars) av1 from rating r join movie m on r.mid=m.mid where m.year < 1980
group by r.mid)
union
select avg(av2) a1 from
(select avg(stars) av2 from rating r join movie m on r.mid=m.mid where m.year > 1980
group by r.mid))
```

Some directors directed more than one movie. For all such directors, return the titles of all movies directed by them, along with the director name. Sort by director name, then movie title. (As an extra challenge, try writing the query both with and without COUNT.)
```sql
select title, director 
from movie 
where director in (select director 
                  from (select director, count(title) as s 
                        from movie 
                        group by director) as t
                  where t.s>1)
order by 2,1
```

Find the movie(s) with the highest average rating. Return the movie title(s) and average rating. (Hint: This query is more difficult to write in SQLite than other systems; you might think of it as finding the highest average rating and then choosing the movie(s) with that average rating.)
```sql
select m.title, avg(r.stars) as strs from rating r
join movie m on m.mid = r.mid group by r.mid
having strs = (select max(s.stars) as stars from (select mid, avg(stars) as stars from rating
group by mid) as s)
```

Find the movie(s) with the lowest average rating. Return the movie title(s) and average rating. (Hint: This query may be more difficult to write in SQLite than other systems; you might think of it as finding the highest average rating and then choosing the movie(s) with that average rating.)
```sql
select m.title, avg(r.stars) as strs from rating r
join movie m on m.mid = r.mid group by r.mid
having strs = (select min(s.stars) as stars from (select mid, avg(stars) as stars from rating
group by mid) as s)
```

For each director, return the director's name together with the title(s) of the movie(s) they directed that received the highest rating among all of their movies, and the value of that rating. Ignore movies whose director is NULL.
```sql
select director, title, stars
from movie m, rating r
where m.mid = r.mid and director is not null
group by director 
order by stars desc
```

