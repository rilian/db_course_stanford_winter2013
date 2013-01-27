# SQL Movie-Rating View Modification Exercises (challenge-level)

Finally, write a single instead-of trigger that combines all three of the previous triggers to enable simultaneous updates to attributes mID, title, and/or stars in view LateRating. Combine the view-update policies of the three previous problems, with the exception that mID may now be updated. Make sure the ratingDate attribute of view LateRating has not also been updated -- if it has been updated, don't make any changes.
```sql
create trigger upd_all
INSTEAD OF UPDATE of mid,title,stars on LateRating
for each row when new.ratingDate = old.ratingDate begin
update Movie set title=new.title where mid=old.mid;
update Rating set stars=new.stars where ratingDate=old.ratingDate and mid=old.mid;
update Movie set mid=new.mid where mid=old.mid;
update Rating set mid=new.mid where mid=old.mid;
end;
```

Write an instead-of trigger that enables insertions into view HighlyRated.

Policy: An insertion should be accepted only when the (mID,title) pair already exists in the Movie table. (Otherwise, do nothing.) Insertions into view HighlyRated should add a new rating for the inserted movie with rID = 201, stars = 5, and NULL ratingDate.
```sql
create trigger ins
INSTEAD OF INSERT on HighlyRated
for each row when exists (select 1 from Movie where mid=new.mid and title=new.title)
begin insert into rating(rid,mid,stars) values(201,new.mid,5); end;
```

Write an instead-of trigger that enables insertions into view NoRating.

Policy: An insertion should be accepted only when the (mID,title) pair already exists in the Movie table. (Otherwise, do nothing.) Insertions into view NoRating should delete all ratings for the corresponding movie.
```sql
create trigger ins
INSTEAD OF INSERT on norating
for each row when exists(select 1 from Movie where mid=new.mid and title=new.title)
begin delete from rating where mid=new.mid; end;
```
