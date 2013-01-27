# SQL Social-Network Triggers Exercises (challenge-level)

Write one or more triggers to maintain symmetry in friend relationships. Specifically, if (A,B) is deleted from Friend, then (B,A) should be deleted too. If (A,B) is inserted into Friend then (B,A) should be inserted too. Don't worry about updates to the Friend table.
```sql
create trigger after_insert
after insert on Friend
for each row when not exists (select 1 from Friend where Friend.id1=new.id2 and Friend.id2=new.id1)
begin insert into Friend values(new.id2, new.id1); end;
|
create trigger after_delete
after delete on Friend
when exists (select 1 from Friend where Friend.id1=old.id2 and Friend.id2=old.id1)
begin delete from Friend where Friend.id1=old.id2 and Friend.id2=old.id1; end;
```

Write a trigger that automatically deletes students when they graduate, i.e., when their grade is updated to exceed 12. In addition, write a trigger so when a student is moved ahead one grade, then so are all of his or her friends.
```sql
create trigger grad
after update of grade on Highschooler
for each row when new.grade > 12
begin delete from Highschooler where id=new.id; end;
|
create trigger move
after update of grade on Highschooler
for each row when new.grade = old.grade + 1
begin
update Highschooler
set grade = grade + 1
where id in (select distinct id2 from Friend where id1=new.id);
end;
```

Write a trigger to enforce the following behavior: If A liked B but is updated to A liking C instead, and B and C were friends, make B and C no longer friends. Don't forget to delete the friendship in both directions, and make sure the trigger only runs when the "liked" (ID2) person is changed but the "liking" (ID1) person is not changed.
```sql
create trigger fr
after update of id2 on Likes
for each row when old.id1=new.id1 and exists(select 1 from Friend where id1=old.id2 and id2=new.id2)
begin delete from Friend where (id1=old.id2 and id2=new.id2) or (id1=new.id2 and id2=old.id2); end;
```
