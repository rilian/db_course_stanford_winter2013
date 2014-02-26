# SQL Social-Network Query Exercises (challenge-level)

Find all students who do not appear in the Likes table (as a student who likes or is liked) and return their names and grades. Sort by grade, then by name within each grade.
```sql
select name, grade
from highschooler
where id not in (select id1 from likes)
and id not in (select ID2 from likes)
order by 2,1
```

For each student A who likes a student B where the two are not friends, find if they have a friend C in common (who can introduce them!). For all such trios, return the name and grade of A, B, and C.
```sql
select h1.name, h1.grade, h2.name, h2.grade, h3.name, h3.grade
from highschooler h1, highschooler h2, highschooler h3,
(select l.id1 as lid1, l.id2 as lid2, f2.id1 as f2id1 from likes l, friend f2, friend f3 where
not exists (select f.id1, f.id2 from friend f where f.id1 = l.id1 and f.id2 = l.id2)
and f2.id2 = l.id1 and f3.id2 = l.id2 and f2.id1 = f3.id1) as t
where h1.id = t.lid1 and h2.id = t.lid2 and h3.id = f2id1
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
select h.name, h.grade from highschooler h, friend f where
h.id = f.id1 group by f.id1 having count(f.id2) = (
select max(r.c) from
(select count(id2) as c from friend group by id1) as r)
```
