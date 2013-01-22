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

```
