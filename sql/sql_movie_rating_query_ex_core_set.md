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
select name, title FROM
(select name, count(Rating.rID) as cnt, Rating.mID, title, stars,ratingDate from Rating
join Reviewer on Rating.rid = Reviewer.rid
join Movie on Movie.mID = Rating.mID
 group by Rating.rID, Rating.mID) as T
WHERE t.cnt > 1 limit 1
```

For each movie that has at least one rating, find the highest number of stars that movie received. Return the movie title and number of stars. Sort by movie title.

```sql
select title, MAX(stars) from Rating
join Movie on Movie.mID=Rating.mID
group by Movie.mID
order by title
```

List movie titles and average ratings, from highest-rated to lowest-rated. If two or more movies have the same average rating, list them in alphabetical order.

```sql
select title, AVG(stars) as avst from Rating
join Movie on Movie.mID=Rating.mID
group by Movie.mID
order by avst DESC, title
```

Find the names of all reviewers who have contributed three or more ratings.

```sql
select name from Reviewer where rID in (
select rID from (
select rID, count(mID) as cnt
from rating
group by rID
having cnt > 2) as t)
```
