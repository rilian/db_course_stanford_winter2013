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

*

```sql

```

*

```sql

```

