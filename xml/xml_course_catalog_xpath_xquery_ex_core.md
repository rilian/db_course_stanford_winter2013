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

Return titles of departments that have some course that takes "CS106B" as a prerequisite.
```
doc("courses.xml")//Department/Course/Prerequisites[Prereq="CS106B"]/../../Title
```


