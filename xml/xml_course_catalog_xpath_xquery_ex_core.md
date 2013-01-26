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

Return last names of all professors or lecturers who use a middle initial. Don't worry about eliminating duplicates.
```
doc("courses.xml")//(Professor|Lecturer)[Middle_Initial]/Last_Name
```

Return the count of courses that have a cross-listed course (i.e., that have "Cross-listed" in their description).
```
doc("courses.xml")/Course_Catalog/count(Department/Course[contains(Description, "Cross-listed")])
```

Return the average enrollment of all courses in the CS department.
```
doc("courses.xml")//Department[@Code="CS"]/avg(Course/@Enrollment)
```

Return last names of instructors teaching at least one course that has "system" in its description and enrollment greater than 100.
```
doc("courses.xml")//Course[@Enrollment>100 and contains(Description,"system")]/Instructors//Last_Name
```
