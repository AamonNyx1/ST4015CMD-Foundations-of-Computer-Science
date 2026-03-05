#  Foundation of Computer Science – Coursework Summary  
## Enhancing Secure Data Exchange with Encoding Formats and Protocol Integration  

---

#  Overview

This coursework demonstrates practical implementation of:

- Docker containerisation  
- Docker bridge networking  
- Client–Server architecture  
- Base64 encoding  
- File transfer using HTTP  
- IP-based communication between containers  
- Differences between HTTP and HTTPS  

The project is divided into Task 1 and Task 3.

---

#  TASK 1 – Base64 Encoding and File Transfer Using Docker

##  Objective

To demonstrate encoding of data before transmission and decoding after reception using two Docker containers connected via a custom network.

---

##  Step 1 – Create Docker Network

```bash
docker network create blue
docker network ls
```

---

##  Step 2 – Start Server Container

```bash
docker run -it --name server --network blue python:3.12-slim bash
```

---

##  Step 3 – Install Required Packages (Server)

```bash
apt update
apt install -y wget iputils-ping
```

---

##  Step 4 – Create Original File (Server)

```bash
echo "This is secret data for Base64 Demo" > dem.txt
ls
```

---

##  Step 5 – Encode File Using Base64 (Server)

```bash
base64 dem.txt > encoded.txt
cat encoded.txt
```

Example encoded output:

```
VGhpcyBpcyBzZWNyZXQgZGF0YSBmb3IgQmFzZTY0IERlbW8K
```

---

##  Step 6 – Check Server IP (Windows CMD)

```bash
docker inspect -f "{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}" server
```

Example output:

```
172.18.0.2
```

---

##  Step 7 – Start HTTP Server (Server)

```bash
python -m http.server 8082
```

---

##  Step 8 – Start Client Container

(Open new CMD window)

```bash
docker run -it --name client --network blue python:3.12-slim bash
```

---

##  Step 9 – Install Packages (Client)

```bash
apt update
apt install -y wget iputils-ping
```

---

##  Step 10 – Test Network Connectivity (Client)

Using container name:

```bash
ping server
```

Using IP address:

```bash
ping 172.18.0.2
```

---

##  Step 11 – Download Encoded File (Client)

Using hostname:

```bash
wget http://server:8082/encoded.txt
```

Using IP:

```bash
wget http://172.18.0.2:8082/encoded.txt
```

---

##  Step 12 – Decode File (Client)

```bash
base64 -d encoded.txt > decoded.txt
cat decoded.txt
```

Expected output:

```
This is secret data for Base64 Demo
```

---

##  What Task 1 Demonstrates

- Docker networking  
- Container-to-container communication  
- DNS resolution within Docker  
- Base64 encoding before transmission  
- File transfer via HTTP  
- Decoding after reception  
- Difference between HTTP and HTTPS  

---
#  Task 3 – Database Normalisation (1NF to 3NF)

##  Module: Foundation of Computer Science  
##  Topic: Database Design and Normalisation  

---

#  Overview

Task 3 demonstrates the process of database normalisation from:

- First Normal Form (1NF)  
- Second Normal Form (2NF)  
- Third Normal Form (3NF)  

The goal is to remove redundancy, eliminate partial dependencies, and ensure proper relational database structure using primary and foreign keys.

---

#  Step 1 – First Normal Form (1NF)

In 1NF, all values must be atomic (no repeating groups).

```sql
CREATE TABLE ClubMembership_1NF (
    StudentID INT,
    StudentName VARCHAR(50),
    Email VARCHAR(50),
    ClubName VARCHAR(50),
    ClubRoom VARCHAR(50),
    ClubMentor VARCHAR(50),
    JoinDate DATE,
    PRIMARY KEY (StudentID, ClubName, JoinDate)
);

SELECT * FROM ClubMembership_1NF;
```

###  Explanation

- Data is stored in one single table.
- Composite primary key: `(StudentID, ClubName, JoinDate)`
- Still contains redundancy.
- Student and club details repeat multiple times.

---

#  Step 2 – Second Normal Form (2NF)

To achieve 2NF:
- Remove partial dependencies.
- Separate student and club information into different tables.

## Create Student Table

```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(50),
    Email VARCHAR(50)
);
```

## Create Club Table

```sql
CREATE TABLE Club (
    ClubID INT PRIMARY KEY,
    ClubName VARCHAR(50),
    ClubRoom VARCHAR(50),
    ClubMentor VARCHAR(50)
);
```

## Create Membership Table

```sql
CREATE TABLE Membership (
    MembershipID INT PRIMARY KEY AUTO_INCREMENT,
    StudentID INT,
    ClubID INT,
    JoinDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (ClubID) REFERENCES Club(ClubID)
);
```

## View Tables

```sql
SELECT * FROM Student;
SELECT * FROM Club;
SELECT * FROM Membership;
```

###  Explanation

- Student data stored separately.
- Club data stored separately.
- Membership table connects Student and Club.
- Foreign keys enforce referential integrity.
- Redundancy reduced significantly.

---

#  Step 3 – Third Normal Form (3NF)

To achieve 3NF:
- Remove transitive dependencies.
- Ensure non-key attributes depend only on the primary key.

```sql
CREATE TABLE StudentClub_3NF (
    StudentClubID INT PRIMARY KEY AUTO_INCREMENT,
    StudentID INT,
    ClubID INT,
    JoinDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (ClubID) REFERENCES Club(ClubID)
);
```

## Join Query to Retrieve Data

```sql
SELECT s.StudentName, c.ClubName, sc.JoinDate
FROM StudentClub_3NF sc
JOIN Student s ON sc.StudentID = s.StudentID
JOIN Club c ON sc.ClubID = c.ClubID;
```

###  Explanation

- StudentClub_3NF acts as junction table.
- Many-to-many relationship handled properly.
- Data integrity maintained.
- No repeating groups.
- No partial or transitive dependency.

---

#  Insert Sample Data

```sql
INSERT INTO Student VALUES (8, 'Srijan', 'srijan@email.com');
INSERT INTO Student VALUES (9, 'mello', 'mello@email.com');

INSERT INTO Club VALUES (501, 'Hacking Club', 'R699', 'Ms. Piya');
INSERT INTO Club VALUES (502, 'Hacking Club', 'R669', 'Ms. Sia');

SELECT * FROM Student;
SELECT * FROM Club;
```

---

#  What Task 3 Demonstrates

- Application of database normalisation principles
- Use of primary keys
- Use of composite keys
- Implementation of foreign key constraints
- Many-to-many relationship handling
- Data redundancy reduction
- Structured relational database design

---

#  Key Concepts Applied

- 1NF: Atomic values, no repeating groups  
- 2NF: Removal of partial dependency  
- 3NF: Removal of transitive dependency  
- Referential Integrity  
- Primary Key vs Foreign Key  
- Junction Table  

---

#  Conclusion

The database was successfully transformed from a single unstructured table (1NF) into a fully normalised relational schema (3NF).  
The final structure eliminates redundancy, ensures data integrity, and supports scalable database design.
