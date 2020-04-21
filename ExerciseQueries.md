sqlite> SELECT DISTINCT students.name
...> FROM students
...> INNER JOIN classes_students
...> ON students.id = classes_students.student_id
...> INNER JOIN classes
...> ON classes.id = classes_students.classes_id
...> WHERE classes.department
...> IN (SELECT departments.id FROM departments WHERE departments.name = 'cs');
name

---

lauren
dan
naomi
kim
chris
