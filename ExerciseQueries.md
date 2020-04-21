<!-- **2 columns, id is uniquely identifyingsqlite> .schema departments   -->

CREATE TABLE departments (
id INTEGER PRIMARY KEY,
name TEXT NOT NULL
);

<!-- **department column used as FOREIGN KEY, to avoid duplicating data -->

sqlite> .schema teachers
CREATE TABLE teachers (
id INTEGER PRIMARY KEY,
name TEXT NOT NULL,
department INTEGER,
FOREIGN KEY(department) REFERENCES departments(id)
);

<!-- **Display the name column for every row in the students table -->

sqlite> SELECT students.name FROM students
...> ;
name

---

lauren
dan
naomi
kim
sam
chris

<!-- **Display every column for the teachers table. -->

sqlite> SELECT \* FROM teachers; //department column contains FOREIGN KEYS referencing departments table
id name department

---

1 fred 1
2 pamela 2
3 beth 1
4 sunny 2

<!-- **and then every column for the departments table. -->

sqlite> SELECT \* FROM departments; //Beth is in 'cs' department
id name

---

1 cs
2 psy

<!-- **Display just the name column for all the students whose names are not naomi. -->

sqlite> SELECT students.name
...> FROM students
...> WHERE students.name != 'naomi';
name

---

lauren
dan
kim
sam
chris

<!-- **Display the name and department id of teachers whose own id is greater than 2 or whose name is 'fred' -->

sqlite> SELECT teachers.name, teachers.department
...> FROM teachers
...> WHERE teachers.id > 2
...> OR teachers.name = 'fred';
name department

---

fred 1
beth 1
sunny 2

<!-- **Display the id and name of all the students whose names end in 'm' -->

sqlite> SELECT students.id, students.name
...> FROM students
...> WHERE students.name
...> LIKE '%m';
id name

---

4 kim
5 sam

<!-- **Display all columns for students whose names do not contain the letter 'a'. -->

sqlite> SELECT \* FROM students
...> WHERE students.name
...> NOT LIKE '%a%';
id name

---

4 kim
6 chris

 <!-- **Display just the names of all the teachers whose id is NOT either 1, 2, or 4 -->

sqlite> SELECT teachers.name
...> FROM teachers
...> WHERE teachers.id
...> NOT IN (1, 2, 4);
name

---

beth

 <!-- **Display just the names of all the teachers whose department is either 1 or 4 -->

sqlite> SELECT teachers.name
...> FROM teachers
...> WHERE teachers.department
...> IN (1, 4);
name

---

fred
beth

<!-- **Display the name and id of all the teachers in the 'psy' department (should be pamela and sunny, with their respective ids) -->

sqlite> SELECT teachers.name, teachers.id
...> FROM teachers
...> WHERE teachers.department
...> IN (SELECT departments.id FROM departments WHERE departments.name = 'psy');
name id

---

pamela 2
sunny 4

<!-- **Display the name of the department that 'sunny' teaches for (should be 'psy') -->

sqlite> SELECT departments.name
...> FROM departments
...> WHERE departments.id
...> IN (SELECT teachers.department FROM teachers WHERE teachers.name = 'sunny');
name

---

psy

<!-- **2 columns, 8 rows -->

sqlite> SELECT departments.id, classes.id FROM departments, classes;
id id

---

1 1
1 2
1 3
1 4
2 1
2 2
2 3
2 4

<!-- **4 columns 24 rows -->

sqlite> SELECT students.\*, teachers.name FROM students, teachers;
id name name

---

1 lauren fred
1 lauren pamela
1 lauren beth
1 lauren sunny
2 dan fred
2 dan pamela
2 dan beth
2 dan sunny
3 naomi fred
3 naomi pamela
3 naomi beth
3 naomi sunny
4 kim fred
4 kim pamela
4 kim beth
4 kim sunny
5 sam fred
5 sam pamela
5 sam beth
5 sam sunny
6 chris fred
6 chris pamela
6 chris beth
6 chris sunny

<!-- **Display the name and id of all the teachers in the 'psy' department (should be pamela and sunny, with their respective ids) -->

sqlite> SELECT teachers.name, teachers.id
...> FROM teachers, departments
...> WHERE teachers.department = departments.id AND departments.name = 'psy';
name id

---

pamela 2
sunny 4

<!-- **Display the name of the department that 'sunny' teaches for (should be 'psy') -->

sqlite> SELECT departments.name
...> FROM departments, teachers
...> WHERE departments.id = teachers.department AND teachers.name = 'sunny';
name

---

psy

<!-- **NO DIFFERENCE -->

SELECT _ FROM students, teachers;
SELECT _ FROM students INNER JOIN teachers;

<!-- **Display the name and id of all the teachers in the 'psy' department (should be pamela and sunny, with their respective ids) -->

sqlite> SELECT teachers.name, teachers.id
...> FROM teachers
...> INNER JOIN departments
...> ON teachers.department = departments.id AND departments.name = 'psy';
name id

---

pamela 2
sunny 4

<!-- ** Display the name of the department that 'sunny' teaches for (should be 'psy') -->

sqlite> SELECT departments.name
...> FROM departments
...> INNER JOIN teachers
...> WHERE departments.id = teachers.department AND teachers.name = 'sunny';
name

---

psy

<!-- **Which classes is 'sam' taking? 'sam' is taking the 'commumication' class -->

sqlite> SELECT classes.name
...> FROM classes
...> INNER JOIN classes_students
...> ON classes.id = classes_students.classes_id
...> INNER JOIN students
...> ON students.id = classes_students.student_id
...> WHERE students.name = 'sam';
name

---

communication

<!-- ** What are the names of the students in the 'compromise' class?  'naomi', 'chris', and 'kim' -->

sqlite> SELECT students.name
...> FROM students
...> INNER JOIN classes_students
...> ON students.id = classes_students.student_id
...> INNER JOIN classes
...> ON classes.id = classes_students.classes_id
...> WHERE classes.name = 'compromise';
name

---

naomi
chris
kim

<!-- **What are the names of the students taking any class in the 'cs' department?
   'lauren', 'dan', 'naomi', 'kim' and 'chris' -->

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
