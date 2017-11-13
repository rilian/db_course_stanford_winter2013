# SQL Social-Network Triggers Exercises (core set)

Write a trigger that makes new students named 'Friendly' automatically like everyone else in their grade. That is, after the trigger runs, we should have ('Friendly', A) in the Likes table for every other Highschooler A in the same grade as 'Friendly'.
```sql
CREATE trigger fr
after insert on Highschooler
for each row when (new.name='Friendly')
begin insert into Likes select new.id, id from Highschooler where grade=new.grade and id <> new.id; end;
```

Write one or more triggers to manage the grade attribute of new Highschoolers. If the inserted tuple has a value less than 9 or greater than 12, change the value to NULL. On the other hand, if the inserted tuple has a null value for grade, change it to 9.
```sql
create trigger g1
after insert on Highschooler
for each row when (new.grade < 9 or new.grade > 12)
begin update Highschooler set grade=null where id=new.id; end;

create trigger g2
after insert on Highschooler
for each row when (new.grade is null)
begin update Highschooler set grade=9 where id=new.id; end;
```

Write a trigger that automatically deletes students when they graduate, i.e., when their grade is updated to exceed 12.
```sql
create trigger g
after update of grade on Highschooler
for each row when new.grade>12
begin delete from Highschooler where id=new.id; end;
```
Write a trigger that automatically deletes students when they graduate, i.e., when their grade is updated to exceed 12 (same as Question 3). In addition, write a trigger so when a student is moved ahead one grade, then so are all of his or her friends.
```sql
create trigger g1
after update of grade on Highschooler
for each row when new.grade>12
begin
  delete from Highschooler where id=new.id;
end;

create trigger g2
after update on Highschooler
for each row
begin
  update Highschooler set grade=grade+1 where ID in (Select ID2 from Friend where ID1=new.ID);
end;
 ```
