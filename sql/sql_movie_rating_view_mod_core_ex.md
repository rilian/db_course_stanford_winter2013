# SQL Movie-Rating View Modification Exercises (core set)

Write an instead-of trigger that enables updates to the title attribute of view LateRating.
```sql
create trigger upd
INSTEAD OF UPDATE of title on LateRating
for each row begin update Movie set title=New.title where mid=New.mid; end;
```

Write an instead-of trigger that enables updates to the stars attribute of view LateRating.
```sql
create trigger upd
INSTEAD OF UPDATE of stars on LateRating
for each row begin update Rating set stars=new.stars where ratingDate=new.ratingDate and mid=new.mid; end;
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
