# XML Course-Catalog XPath and XQuery Exercises (core set)

Return all Title elements (of both departments and courses).
```
doc("courses.xml")//Title
```

Return last names of all department chairs.
```
doc("courses.xml")//Department/Chair//Last_Name
```
