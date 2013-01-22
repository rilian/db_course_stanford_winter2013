# SQL Social-Network Modification Exercises

It's time for the seniors to graduate. Remove all 12th graders from Highschooler.

```sql
delete from Highschooler where grade=12
```

If two students A and B are friends, and A likes B but not vice-versa, remove the Likes tuple.

```sql
DELETE FROM likes
where exists (select 1 from friend where friend.id1 = likes.id1 and friend.id2=likes.id2)
and not exists (select 1 from likes as L2 where L2.id1 = likes.id2 and L2.id2=likes.id1)
```

For all cases where A is friends with B, and B is friends with C, add a new friendship for the pair A and C. Do not add duplicate friendships, friendships that already exist, or friendships with oneself.

```sql
insert into Friend (id1, id2)
select DISTINCT i1, i2 from (
  select F1.id1 as i1, F2.id2 as i2
  from friend F1  join friend F2 on F1.id2 = F2.id1
) as t
where t.i1 != t.i2
and not exists (select 1 from Friend where id1=i1 and id2=i2)
and not exists (select 1 from Friend where id2=i1 and id1=i2)
```
