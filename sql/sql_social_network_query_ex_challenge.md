# SQL Social-Network Query Exercises (challenge-level)

Find all students who do not appear in the Likes table (as a student who likes or is liked) and return their names and grades. Sort by grade, then by name within each grade.
```sql
select name, grade from highschooler where id not in (select id1 from likes union select id2 from likes)
```

For each student A who likes a student B where the two are not friends, find if they have a friend C in common (who can introduce them!). For all such trios, return the name and grade of A, B, and C.
```sql
--
```


```sql
--
```


```sql
--
```


```sql
--
```


```sql
--
```
