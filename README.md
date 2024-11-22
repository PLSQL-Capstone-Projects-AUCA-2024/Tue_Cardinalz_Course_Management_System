# _**Course_Management_System**_

## **CARDINALS GROUP MEMBERS**:

1.	  MUGISHA Julien			          26967
2.	  IRADUKUNDA Delphine 			    23665
3.	  SHEJA N M Yves				        26500
4.	  ISHIMWE Mireille 			        26828
5.	  INEZA HABAMENSHI Darryl		    25948
6.	  UWAYO Olga 				            26139
7.	  KAMALI MUSASIRA Philbert		  26261
8.	  IRAKOZE Arlaine Peace			    23753
9.	  ISHEMA NGABO Ange			        26035


## The provided documents in the main branch (Phase 2 and 3) outline the design and objectives of a course management system aimed at streamlining tasks for university lecturers.
📝Here are some insights of the whole project:
<br>
<br>



## PHASE 2: 💼Business Process Modeling focuses on defining and visualizing the workflow of the course management system. This phase includes:
<br>

⚡ _**Scope**_ : Centralizing course management tasks like attendance, assignments, and grading.

🎯 _**Objectives**_ : Automate attendance tracking, simplify grading, and offer real-time performance data.

😈 _**Entities**_ : Lecturer, Course, Class, Student, Assignment, Grade, and Department, each playing a specific role in the workflow.

😎 _**Significance in MIS**_ : The system supports decision-making by giving lecturers insights into student performance, enhancing efficiency in course administration​(refernce to Phase 2.docx ).
<br>
<br>
<br>

![pl](https://github.com/user-attachments/assets/4d5217ea-19ca-4363-9d76-fe8aaf20d969)

<br>
<br>


## PHASE 3: ⚙️Logical Model Design provides a structured database schema for the system. Key components include:
<br>
<br>


👌 _**Entities**_ : Department, Lecturer, Course, Student, Assignment, Submission, Grade, and Attendance.

🫂 _**Relationships**_ : Defined using primary and foreign keys, the model connects students, lecturers, and courses to facilitate assignment submissions, grading, and attendance tracking.

🎊 _**Goals**_ : Create an organized structure to support accurate and accessible data, enabling effective tracking of student progress and lecturer workload management​(reference to Phase 3.docx).

![Phase 3 (Logical model structure)](https://github.com/user-attachments/assets/62ccfcbe-81b3-4c63-a719-22a532f49ad1)


Together, these documents lay the foundation for a course management system that supports lecturers in administrative tasks and contributes to better educational outcomes.

<br>
<br>

# PHASE 4: 📡 _CREATING DATABASE AND ORACLE ENTERPRISE MANAGER._

## CREATION OF A PLUGGABLE DATABASE:

 We have to create a pluggable database for our project and we name it " _**TUE_CARDINALS_COURSEMANAGEMENTSYSTEM**_ ". We created it using the following codes

 ```sql

-- Step 1: Create the Pluggable Database (PDB)

CREATE PLUGGABLE DATABASE tue_cardinals_CourseManagementSystem
ADMIN USER tue_cardinals IDENTIFIED BY cardinals
ROLES = (DBA)
FILE_NAME_CONVERT = ('C:\app\CIOOL\product\21c\oradata\XE\pdbseed',
'C:\app\CIOOL\product\21c\oradata\XE\tue_cardinals_CourseManagementSystem/');

-- Step 2: Open the Pluggable Database

ALTER PLUGGABLE DATABASE tue_cardinals_CourseManagementSystem OPEN;

```
The result should be like this 

![alt text](Phase%204%20(Create%20Pluggable%20DB%20SCREENSHOT).PNG)

After this you should configure the Service Name through Net configuration assissant.

## ORACLE ENTERPRISE MANAGER:

Login with the username created and its password with the container as our pluggable database

It should give you, a homepage like this:

![alt text](Phase%204%20(Oracle%20Enterprise%20HomePage%20SCREENSHOT)%20.PNG)

_For any futher information please go check on the Oracle enterprise tablespace screenshot and Oracle enterprise login screenshot._

<br>
<br>

# PHASE 5: 📝 _TABLE IMPLEMENTATION AND DATA INSERTION._

## TABLE CREATION:

Here are the codes for the creation of all tables while keeping data integrity and using corresponding constraints :

```sql
-- Table department
CREATE TABLE DEPARTMENT (
Department_ID INT PRIMARY KEY,
Name VARCHAR(100) NOT NULL,
Contact_Details VARCHAR(255) NOT NULL
);

-- Table lecturer
CREATE TABLE LECTURER (
Lecturer_ID INT PRIMARY KEY,
Name VARCHAR(100) NOT NULL,
Contact_Details VARCHAR(255) NOT NULL,
Department_ID INT NOT NULL,
FOREIGN KEY (Department_ID) REFERENCES DEPARTMENT(Department_ID)
);

-- Table course
CREATE TABLE COURSE (
Course_ID INT PRIMARY KEY,
Name VARCHAR(100) NOT NULL,
Description VARCHAR(200),
Credits INT NOT NULL,
Semester INT,
Lecturer_ID INT NOT NULL,
FOREIGN KEY (Lecturer_ID) REFERENCES LECTURER(Lecturer_ID)
);

-- Table student
CREATE TABLE STUDENT (
Student_ID INT PRIMARY KEY,
Name VARCHAR(100) NOT NULL,
Email VARCHAR(100) UNIQUE,
Major VARCHAR(100) NOT NULL,
Year_of_study INT NOT NULL,
Department_ID INT NOT NULL,
FOREIGN KEY (Department_ID) REFERENCES DEPARTMENT(Department_ID)
);

-- Table assignment
CREATE TABLE ASSIGNMENT (
Assignment_ID INT PRIMARY KEY,
Course_ID INT NOT NULL,
Description VARCHAR(200),
Due_Date DATE NOT NULL,
Submission_Link VARCHAR(255),
FOREIGN KEY (Course_ID) REFERENCES COURSE(Course_ID)
);

-- Table grade
CREATE TABLE GRADE (
GRADE_ID INT PRIMARY KEY,
Student_ID INT NOTNULL,
Assignment_ID INT NOT NULL,
Grade_Value INT NOT NULL,
FOREIGN KEY (Student_ID) REFERENCES STUDENT(Student_ID),
FOREIGN KEY (Assignment_ID) REFERENCES ASSIGNMENT(Assignment_ID)
);

-- Table submission
CREATE TABLE SUBMISSION (
Submission_ID INT PRIMARY KEY,
Assignment_ID INT NOT NULL,
Student_ID INT NOT NULL,
Submission_Date DATE NOT NULL,
File_Link VARCHAR(255),
FOREIGN KEY (Assignment_ID) REFERENCES ASSIGNMENT(Assignment_ID),
FOREIGN KEY (Student_ID) REFERENCES STUDENT(Student_ID)
);

-- Table attendance
CREATE TABLE ATTENDANCE (
Attendance_ID INT PRIMARY KEY,
Course_ID INT NOT NULL,
Student_ID INT NOT NULL,
Status VARCHAR(20) NOT NULL,
Attendance_Date DATE DEFAULT CURRENT_DATE,
FOREIGN KEY (Course_ID) REFERENCES COURSE(Course_ID),
FOREIGN KEY (Student_ID) REFERENCES STUDENT(Student_ID)
);

```
Here are some few sample of the Tables created for futher information, I recommend you to check out  phase 5 files attached

![alt text](Phase%205%20(Desc%20Tables%20SCREENSHOT).PNG)

## DATA INSERTION:

For the purpose of keeping the readme short, we chose to use few example to demostrate the insertion of data in our Tables. 

### 1. insertion for Department Table
![alt text](Phase%205%20(Department%20Table%20SCREENSHOT).jpeg)

### 2. insertion for Course Table
![alt text](Phase%205%20(Course%20table%20SCREENSHOT).jpeg)

_**REMARK**_ : _Please check the  files provided in the repo  for the rest of screenshots_

<br>
<br>

# PHASE 6:  📓 _DATABASE INTERACTIONS AND TRANSACTION_

## DATABASE OPERATIONS

They are many DML and DDL operations that can be than but we focus on mostly the "JOIN".

We thought and got an idea of how to make a good use of joins outside the scope the other group would use and we thought we could use views and there are the code for views:

```sql
CREATE VIEW StudentMarks AS
SELECT 
    s.Student_ID,
    s.Name AS Student_Name,
    d.Name AS Department_Name,
    a.Description AS Assignment_Description,
    su.File_Link AS Submission_Link,
    su.Submission_Date,
    g.Grade_Value
FROM 
    STUDENT s
JOIN 
    DEPARTMENT d ON s.Department_ID = d.Department_ID
LEFT JOIN 
    GRADE g ON s.Student_ID = g.Student_ID
LEFT JOIN 
    ASSIGNMENT a ON g.Assignment_ID = a.Assignment_ID
LEFT JOIN 
    SUBMISSION su ON su.Assignment_ID = a.Assignment_ID AND su.Student_ID = s.Student_ID;
```

this views will be called and it will bring the student id selected, their name, department they belong to,the assignment did and their grades.

calling the views:

``` sql
SELECT * FROM StudentMarks WHERE STUDENT_ID = 1; -- for example

```

##  Transaction Management:

Firstly, we will give a short explanation of what a transaction is :

a transaction is a sequence of operations performed as a single logical unit of work, ensuring data consistency and reliability.

so we chose a simple transaction and here is the following:

 ``` sql
BEGIN TRANSACTION;

INSERT INTO ATTENDANCE (Attendance_ID, Course_ID, Student_ID, Status)
VALUES (26, 1, 40, 'Present');

UPDATE COURSE
SET Seats_Available = Seats_Available - 1
WHERE Course_ID = 1;

COMMIT;

```
<br>
<br>

# THIS MARK THE CONCLUSION OF OUR PROJECT 

# _**CREDITS**_ :

_credit of this project goes to every member of the group that worked relentlessly and hard for this to happen :_




1. **MUGISHA Julien**  
   - Business Process Modeling  
   - Lecturer Table Creation  
   - Repository Creation  

2. **IRADUKUNDA Delphine**  
   - Business Process Modeling  
   - Course Table Creation  
   - Repository Creation  

3. **SHEJA N M Yves**  
   - Logical Modeling  
   - Database Creation  
   - Transaction Operation Creation  

4. **ISHIMWE Mireille**  
   - Business Process Modeling  
   - Department Table Creation  
   - View Creation  

5. **INEZA HABAMENSHI Darryl**  
   - Logical Modeling  
   - Submission Table Creation  
   - View Creation  

6. **UWAYO Olga**  
   - Business Process Modeling  
   - Attendance Table Creation  
   - View Creation  

7. **KAMALI MUSASIRA Philbert**  
   - Logical Modeling  
   - Grade Table Creation  
   - Transaction Operation Creation  

8. **IRAKOZE Arlaine Peace**  
   - Logical Modeling  
   - Assignment Table Creation  
   - Repository Creation  

9. **ISHEMA NGABO Ange**  
   - Business Process Modeling  
   - Students Table Creation  
   - View Creation  





