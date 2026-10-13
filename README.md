# Library_Management_System
This project demonstrate the implementation of a Library Management System using SQL . It creating and managing tables. Performing CRUD operation and executing advanced SQL queries. The goal is to showcase skills in database design, manipulation and quering.

<img width="736" height="552" alt="download (3)_1_11zon" src="https://github.com/user-attachments/assets/5f4b2314-f5b1-4090-ab14-bedcf66373cf" />


## Objectives 
1. **Set up the library Management System Database** : Create and populate the database with tables for branches, employees, members, books, issued_status, and return status.
2. **CRUD Operations** : Perform Create, Read, Update and Delete Operation on the data.
3. **CTAS (Create Table As Select)** : Utilize CTAS to create new table based on query results.

  ``` sql
   DROP TABLE  IF EXISTS branch;
   CREATE  Table branch
  (
       branch_id VARCHAR(10) PRIMARY KEY,
            manager_id VARCHAR(10),
            branch_address VARCHAR(30),
            contact_no VARCHAR(15)
);

-- Create table "Employee"
DROP TABLE IF EXISTS employee;
CREATE TABLE Employees
  (
          emp_id VARCHAR(10) PRIMARY KEY,
            emp_name VARCHAR(30),
            position VARCHAR(30),
            salary DECIMAL(10,2),
            branch_id VARCHAR(10),
            FOREIGN KEY (branch_id) REFERENCES  branch(branch_id) 
);

-- Create table Members
DROP TABLE IF EIXSTS members
CREATE TABLE Members
  (
            member_id VARCHAR(10) PRIMARY KEY,
            member_name VARCHAR(30),
            member_address VARCHAR(30),
            reg_date DATE

  );

-- Create table "Books"
DROP TABLE IF Exists Books;
CREATE TABLE books
  (
           isbn VARCHAR(50) PRIMARY KEY,
            book_title VARCHAR(80),
            category VARCHAR(30),
            rental_price DECIMAL(10,2),
            status VARCHAR(10),
            author VARCHAR(30),
            publisher VARCHAR(30)
);

-- Create Table "Issued _Status"
DROP TABLE IF Exists Issued_Status
CREATE TABLE Issued_Status
  (
            issued_id VARCHAR(10) PRIMARY KEY,
            issued_member_id VARCHAR(30),
            issued_book_name VARCHAR(80),
            issued_date DATE,
            issued_book_isbn VARCHAR(50),
            issued_emp_id VARCHAR(10),
            FOREIGN KEY (issued_member_id) REFERENCES members(member_id),
            FOREIGN KEY (issued_emp_id) REFERENCES employees(emp_id),
            FOREIGN KEY (issued_book_isbn) REFERENCES books(isbn) 
  );

  -- Create Table "Return Status"
  DROP TABLE IF EXISTS Return Status;
  CREATE TABLE Return_Status
  (
        return_id VARCHAR(10) PRIMARY KEY,
            issued_id VARCHAR(30),
            return_book_name VARCHAR(80),
            return_date DATE,
            return_book_isbn VARCHAR(50),
            FOREIGN KEY (return_book_isbn) REFERENCES books(isbn)
);

- **Database Creation**: Created a Database named `library_management`.
- **Table Creation**: Created tables for branches, employees, members, books, issued_status, and return_status. Each table includes    relevant columns and relationships.

**Task 1. Create a New Book Record**
-- "978-1-60129-456-2', 'To Kill a Mockingbird', 'Classic', 6.00, 'yes', 'Harper Lee', 'J.B. Lippincott & Co.')"

```sql
  UPDATE members
  SET member_address = '123 Oak St'
  WHERE member_id = 'C103' ;


**Task 3: Delete a Record from the Issued Status Table.**
-- Objective: Delete the record with issued_id = 'IS121' from the issued_status table.

```sql
DELETE FROM issued_status
WHERE   issued_id =   'IS121';
```

**Task 4: Retrieve All Books Issued by a Specific Employee**
-- Objective: Select all books issued by the employee with emp_id = 'E101'.
```sql
SELECT * FROM issued_status
WHERE issued_emp_id = 'E101'
```

**Task 5: List Members Who Have Issued More Than One Book**
-- Objective: Use GROUP BY to find members who have issued more than one book.
```sql
SELECT
    issued_emp_id,
    COUNT(*)
FROM issued_status
GROUP BY 1
HAVING COUNT(*) > 1
```
### 3. CTAS (Create table As Select ) 
-- **Task 6: Create Summary tables** :Used CTAS to generate new table based on query results - each book and total book_issued_cnt**
``` sql
CREATE TABLE book_issued_cnt AS
SELECT
   b.isbn,
   b.book_title,
  Count(ist.issued_id) as issue_count
  FROM issued_Status as ist
  JOIN books as b
  ON ist.issued_book_isbn = b.isbn
  GROUP BY b.isbn, b.book_title;
```

### 4. Data Analysis & Findings
The following  SQL queries were used to address specific question.

Task 7.**Retrieve All books in a specific category
```sql
  SELECT * FROM Books
  WHERE Category = "Classic"

```
Task 8.**Task 8: Find Total Rental Income by Category**
``` sql
  SELECT 
	b.category,
    SUM(b.rental_price) as Rental_Price,
    Count(*) as Book_Count
  FROM books as b
  JOIN Issued_Status as ist 
  ON b.isbn = ist.issued_book_isbn
  GROUP BY 1 
  ORDER BY Book_count;

```
### 4.JOINS 
Task 9: **Retrieve the List of Books Not Yet Returned**
``` sql
	SELECT *
	FROM issued_Status as ist
	LEFT JOIN
	return_status as rs
	ON rs.issued_id = ist.issued_id
	WHERE rs.return_id is Null;

Task 10: **Display all members who have issued at least one book.**
``` sql
	SELECT
		members.member_id,
		members.member_name,
		COUNT(ist.issued_member_id) as total_books_issued
	FROM members
	JOIN issued_status as ist
	ON members.member_id = ist.issued_member_id
	GROUP BY members.member_id
	HAVING COUNT (ist.issued_member_id)>=1;

