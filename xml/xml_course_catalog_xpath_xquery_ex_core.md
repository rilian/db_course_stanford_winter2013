# XML Course-Catalog XPath and XQuery Exercises (core set)

Return all Title elements (of both departments and courses).
```
doc("courses.xml")//Title
```

Return last names of all department chairs.
```
doc("courses.xml")//Department/Chair//Last_Name
```

Return titles of courses with enrollment greater than 500.
```
doc("courses.xml")//Course[@Enrollment>500]/Title
```
