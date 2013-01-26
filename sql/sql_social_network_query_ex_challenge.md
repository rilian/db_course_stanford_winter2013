# SQL Social-Network Query Exercises (challenge-level)

Find all students who do not appear in the Likes table (as a student who likes or is liked) and return their names and grades. Sort by grade, then by name within each grade.
```sql
select name, grade from highschooler where id not in (select id1 from likes union select id2 from likes)
```

For each student A who likes a student B where the two are not friends, find if they have a friend C in common (who can introduce them!). For all such trios, return the name and grade of A, B, and C.
```sql
select h1.name, h1.grade, h2.name, h2.grade from
likes L join highschooler H1 on l.id1=h1.id
join highschooler H2 on l.id2=h2.id
where not exists (select 1 from friend F where (F.id1=L.id1 and f.id2=L.id2) or (F.id2=L.id1 and f.id1=L.id2))
...
```

Find the difference between the number of students in the school and the number of different first names.
```sql
select (select count(id) from Highschooler)-(select count(distinct name) from Highschooler)
```

What is the average number of friends per student? (Your result should be just one number.)
```sql
select avg(c) from (select id1, count(id2) c from friend group by id1)
```

Find the number of students who are either friends with Cassandra or are friends of friends of Cassandra. Do not count Cassandra, even though technically she is a friend of a friend.
```sql
select count(id2) from friend where id1 in (
  select id2 from friend where id1 in (select id from highschooler where name='Cassandra')
)
and id1 not in (select id from highschooler where name='Cassandra')
```

Find the name and grade of the student(s) with the greatest number of friends.
```sql
select name, grade from highschooler where id in (select id1 from (
select id1, count(id2) c from friend group by id1 having c=(select max(c2) from (select id1, count(id2) c2 from friend group by id1 order by c2 desc))))
```
