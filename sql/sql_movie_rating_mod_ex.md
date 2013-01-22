# SQL Movie-Rating Modification Exercises

Add the reviewer Roger Ebert to your database, with an rID of 209.

```sql
insert into Reviewer(rID, name) values (209, 'Roger Ebert')
```

Insert 5-star ratings by James Cameron for all movies in the database. Leave the review date as NULL.

```sql
insert into Rating  ( rID, mID, stars, ratingDate )
select Reviewer.rID , Movie.mID, 5, null from Movie
left outer join Reviewer
where Reviewer.name='James Cameron'
```

For all movies that have an average rating of 4 stars or higher, add 25 to the release year. (Update the existing tuples; don't insert new tuples.)

```sql
update movie
set year = year + 25
where mID in (
  select mID from (
  select AVG(stars) as astar, mID from Rating
  where mID=rating.mID
  group by mID
  having astar >=4)
)
```

Remove all ratings where the movie's year is before 1970 or after 2000, and the rating is fewer than 4 stars.

```sql
delete from rating
where mID in (select mID from movie where year <1970 or year > 2000)
and stars < 4
```

