#  College Club Membership Management  
## Database Normalization 
<img width="861" height="346" alt="image" src="https://github.com/user-attachments/assets/53fc6b80-02e6-438a-a156-af60cf6f43b9" />


---

##  Overview

Normalization is a database design technique used to organize data efficiently.  
It reduces data redundancy and prevents problems such as:

- Insert anomalies  
- Update anomalies  
- Delete anomalies  

The process divides one large table into smaller related tables.

---

## 🗂 Database Schema – Unnormalized Form

Initial Table Structure:

```
(StudentID, StudentName, Email, ClubName, ClubRoom, ClubMentor, JoinDate)
```

All student, club, and membership information is stored in one single table.

---

##  Identify Data Problems

###  Problem 1: Data Redundancy
- Student information (name, email) is repeated multiple times.
- Club information (room, mentor) is also repeated.

###  Problem 2: Update Anomaly
If the Music Club room changes, it must be updated in many rows.  
Missing even one row results in inconsistent data.

###  Problem 3: Insert Anomaly
A new club cannot be added unless at least one student joins it.

###  Problem 4: Delete Anomaly
If the last student leaves a club, the club information is also deleted.

---

##  Why the Table is Not Normalized

- Multiple entities (Student, Club, Membership) stored together.
- Repeated data exists.
- Dependencies are unclear.
- Causes insert, update, and delete anomalies.

---

#  Quick Start

```bash
git clone https://github.com/YourUsername/YourRepository
cd YourRepository
```

---

#  Step 1: Create Unnormalized Table

```sql
CREATE TABLE DATA (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    StudentID INT,
    StudentName VARCHAR(50),
    Email VARCHAR(50),
    ClubName VARCHAR(50),
    ClubRoom VARCHAR(50),
    ClubMentor VARCHAR(50),
    JoinDate DATE
);
```

## Insert Sample Data

```sql
INSERT INTO DATA (StudentID, StudentName, Email, ClubName, ClubRoom, ClubMentor, JoinDate) VALUES
(1, 'Asha', 'asha@email.com', 'Music Club', 'R101', 'Mr. Raman', '2024-01-10'),
(2, 'Bikash', 'bikash@email.com', 'Sports Club', 'R202', 'Ms. Sita', '2024-01-12'),
(1, 'Asha', 'asha@email.com', 'Sports Club', 'R202', 'Ms. Sita', '2024-01-15'),
(3, 'Nisha', 'nisha@email.com', 'Music Club', 'R101', 'Mr. Raman', '2024-01-20'),
(4, 'Rohan', 'rohan@email.com', 'Drama Club', 'R303', 'Mr. Kiran', '2024-01-18'),
(5, 'Suman', 'suman@email.com', 'Music Club', 'R101', 'Mr. Raman', '2024-01-22'),
(2, 'Bikash', 'bikash@email.com', 'Drama Club', 'R303', 'Mr. Kiran', '2024-01-25'),
(6, 'Pooja', 'pooja@email.com', 'Sports Club', 'R202', 'Ms. Sita', '2024-01-27'),
(3, 'Nisha', 'nisha@email.com', 'Coding Club', 'Lab1', 'Mr. Anil', '2024-01-28'),
(7, 'Aman', 'aman@email.com', 'Coding Club', 'Lab1', 'Mr. Anil', '2024-01-30');
```

---

#  Step 2: Apply Normalization

---

##  First Normal Form (1NF)

- Each column contains atomic values.
- No repeating groups.
- Each row is unique.

(The DATA table already satisfies 1NF.)

---

##  Second Normal Form (2NF)

Separate Students and Clubs into different tables.

```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(50),
    Email VARCHAR(50)
);

CREATE TABLE Club (
    ClubID INT PRIMARY KEY,
    ClubName VARCHAR(50),
    ClubRoom VARCHAR(50),
    ClubMentor VARCHAR(50)
);
```

---

##  Third Normal Form (3NF)

Create Membership table to remove transitive dependency.

```sql
CREATE TABLE Membership (
    StudentID INT,
    ClubID INT,
    JoinDate DATE,
    PRIMARY KEY (StudentID, ClubID),
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (ClubID) REFERENCES Club(ClubID)
);
```

---

#  Verification & Testing

---

##  Test Case 1: View Students

```sql
SELECT * FROM Student;
```

---

##  Test Case 2: View Clubs

```sql
SELECT * FROM Club;
```

---

## Test Case 3: View Membership

```sql
SELECT * FROM Membership;
```

---

##  Test Case 4: Join Operation

Display Student Name, Club Name, and Join Date:

```sql
SELECT s.StudentName, c.ClubName, m.JoinDate
FROM Membership m
JOIN Student s ON m.StudentID = s.StudentID
JOIN Club c ON m.ClubID = c.ClubID;
```

---

#  Technologies Used

- MySQL 8.0
- SQL
- Docker
- GitHub

---

#  Learning Outcomes

- Understood database normalization (1NF, 2NF, 3NF)
- Reduced redundancy
- Prevented anomalies
- Implemented foreign keys
- Performed JOIN operations

---

 Author: Udaya Tamang  
Bachelor of Computer Science (Cyber Security)
