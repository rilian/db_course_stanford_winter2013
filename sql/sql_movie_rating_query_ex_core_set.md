# SQL Movie-Rating Query Exercises (core set)

Find the titles of all movies directed by Steven Spielberg.

```sql
select title from Movie where director='Steven Spielberg'
```

Find all years that have a movie that received a rating of 4 or 5, and sort them in increasing order.

```sql
select year from Movie
where mID IN (select mID from Rating where stars IN (4,5))
order by year asc
```

Find the titles of all movies that have no ratings.

```sql
select title from Movie where mID NOT IN (select mID from Rating)
```

Some reviewers didn't provide a date with their rating. Find the names of all reviewers who have ratings with a NULL value for the date.

```sql
select name from Reviewer
where rID in (select rID from  Rating where ratingDate IS NULL)
```

Write a query to return the ratings data in a more readable format: reviewer name, movie title, stars, and ratingDate. Also, sort the data, first by reviewer name, then by movie title, and lastly by number of stars.

```sql
select Reviewer.name, Movie.title, Rating.stars, Rating.ratingDate
FROM Rating join Reviewer on Rating.rID = Reviewer.rID
 join Movie on Movie.mID=Rating.mID
order by Reviewer.name, Movie.title, stars
```

For all cases where the same reviewer rated the same movie twice and gave it a higher rating the second time, return the reviewer's name and the title of the movie.

```sql
select Reviewer.name, Movie.title
from Reviewer, Movie, (select R1.rID, R1.mID from Rating R1, Rating R2 where R1.rID=R2.rID and R1.mID=R2.mID and R2.ratingDate>R1.ratingDate and R2.stars>R1.stars) as T
where Reviewer.rID=T.rID and Movie.mID=T.mID
```

For each movie that has at least one rating, find the highest number of stars that movie received. Return the movie title and number of stars. Sort by movie title.

```sql
select title, MAX(stars) from Rating
join Movie on Movie.mID=Rating.mID
group by Movie.mID
order by title
```

For each movie, return the title and the 'rating spread', that is, the difference between highest and lowest ratings given to that movie. Sort by rating spread from highest to lowest, then by movie title. 

```sql
select * 
from (select title, max(stars)-min(stars) value
        from rating natural join movie 
        group by title) 
order by value desc
```

 Find the difference between the average rating of movies released before 1980 and the average rating of movies released after 1980. (Make sure to calculate the average rating for each movie, then the average of those averages for movies before 1980 and movies after. Don't just calculate the overall average rating before and after 1980.) 

```sql
select abs(avg(a)-(select avg(b) 
                    from (select avg(stars) b 
                    from rating natural join movie 
                    where year <1980 
                    group by title)))
from (select avg(stars) a
        from rating natural join movie 
        where year >1980 
        group by title) 
```
