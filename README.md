# mysql-comprehensive-assessment
-- Create the database
CREATE DATABASE library;

-- Use the library database
USE library;

-- Create Branch table
CREATE TABLE Branch (
    Branch_no INT PRIMARY KEY,
    Manager_Id INT,
    Branch_address VARCHAR(255),
    Contact_no VARCHAR(15)
);

-- Create Employee table
CREATE TABLE Employee (
    Emp_Id INT PRIMARY KEY,
    Emp_name VARCHAR(100),
    Position VARCHAR(100),
    Salary DECIMAL(10, 2),
    Branch_no INT,
    FOREIGN KEY (Branch_no) REFERENCES Branch(Branch_no)
);

-- Create Books table
CREATE TABLE Books (
    ISBN VARCHAR(20) PRIMARY KEY,
    Book_title VARCHAR(255),
    Category VARCHAR(100),
    Rental_Price DECIMAL(10, 2),
    Status VARCHAR(3) CHECK (Status IN ('yes', 'no')),
    Author VARCHAR(100),
    Publisher VARCHAR(100)
);

-- Create Customer table
CREATE TABLE Customer (
    Customer_Id INT PRIMARY KEY,
    Customer_name VARCHAR(100),
    Customer_address VARCHAR(255),
    Reg_date DATE
);

-- Create IssueStatus table
CREATE TABLE IssueStatus (
    Issue_Id INT PRIMARY KEY,
    Issued_cust INT,
    Issued_book_name VARCHAR(255),
    Issue_date DATE,
    Isbn_book VARCHAR(20),
    FOREIGN KEY (Issued_cust) REFERENCES Customer(Customer_Id),
    FOREIGN KEY (Isbn_book) REFERENCES Books(ISBN)
);

-- Create ReturnStatus table
CREATE TABLE ReturnStatus (
    Return_Id INT PRIMARY KEY,
    Return_cust INT,
    Return_book_name VARCHAR(255),
    Return_date DATE,
    Isbn_book2 VARCHAR(20),
    FOREIGN KEY (Return_cust) REFERENCES Customer(Customer_Id),
    FOREIGN KEY (Isbn_book2) REFERENCES Books(ISBN)
);
-- Insert data into Branch table
INSERT INTO Branch VALUES
(1, 101, '123 Library St, City A', '1234567890'),
(2, 102, '456 Book Ln, City B', '0987654321');

-- Insert data into Employee table
INSERT INTO Employee VALUES
(1, 'Alice Smith', 'Manager', 60000, 1),
(2, 'Bob Johnson', 'Assistant', 40000, 1),
(3, 'Charlie Davis', 'Librarian', 45000, 2),
(4, 'David Harris', 'Manager', 70000, 2);

-- Insert data into Books table
INSERT INTO Books VALUES
('978-3-16-148410-0', 'Book A', 'Fiction', 20.00, 'yes', 'Author A', 'Publisher A'),
('978-1-23-456789-7', 'Book B', 'Non-Fiction', 25.00, 'no', 'Author B', 'Publisher B'),
('978-0-12-345678-9', 'Book C', 'Science', 30.00, 'yes', 'Author C', 'Publisher C');

-- Insert data into Customer table
INSERT INTO Customer VALUES
(1, 'John Doe', '789 Main St, City A', '2020-01-01'),
(2, 'Jane Smith', '456 Side St, City B', '2021-06-15'),
(3, 'Emily Jones', '123 High St, City A', '2023-03-20');

-- Insert data into IssueStatus table
INSERT INTO IssueStatus VALUES
(1, 1, 'Book A', '2023-06-01', '978-3-16-148410-0'),
(2, 2, 'Book C', '2023-06-15', '978-0-12-345678-9');

-- Insert data into ReturnStatus table
INSERT INTO ReturnStatus VALUES
(1, 1, 'Book A', '2023-06-10', '978-3-16-148410-0');
-- Display all tables 
SELECT * FROM Branch;
SELECT * FROM Employee;
SELECT * FROM Books;
SELECT * FROM Customer;
SELECT * FROM IssueStatus;
SELECT * FROM ReturnStatus;

-- 1. Retrieve the book title, category, and rental price of all available books.
SELECT Book_title, Category, Rental_Price
FROM Books
WHERE Status = 'yes';

-- 2. List the employee names and their respective salaries in descending order of salary.
SELECT Emp_name, Salary
FROM Employee
ORDER BY Salary DESC;

-- 3. Retrieve the book titles and the corresponding customers who have issued those books.
SELECT Books.Book_title, Customer.Customer_name
FROM IssueStatus
JOIN Books ON IssueStatus.Isbn_book = Books.ISBN
JOIN Customer ON IssueStatus.Issued_cust = Customer.Customer_Id;

-- 4. Display the total count of books in each category.
SELECT Category, COUNT(*) AS Total_Books
FROM Books
GROUP BY Category;

-- 5. Retrieve the employee names and their positions for the employees whose salaries are above Rs.50,000.
SELECT Emp_name, Position
FROM Employee
WHERE Salary > 50000;

-- 6. List the customer names who registered before 2022-01-01 and have not issued any books yet.
SELECT Customer_name
FROM Customer
WHERE Reg_date < '2022-01-01'
  AND Customer_Id NOT IN (SELECT Issued_cust FROM IssueStatus);

-- 7. Display the branch numbers and the total count of employees in each branch.
SELECT Branch_no, COUNT(*) AS Total_Employees
FROM Employee
GROUP BY Branch_no;

-- 8. Display the names of customers who have issued books in the month of June 2023.
SELECT Customer.Customer_name
FROM IssueStatus
JOIN Customer ON IssueStatus.Issued_cust = Customer.Customer_Id
WHERE Issue_date BETWEEN '2023-06-01' AND '2023-06-30';

-- 9. Retrieve book_title from book table containing history.
SELECT Book_title
FROM Books
WHERE Book_title LIKE '%history%';

-- 10. Retrieve the branch numbers along with the count of employees for branches having more than 5 employees.
SELECT Branch_no, COUNT(*) AS Total_Employees
FROM Employee
GROUP BY Branch_no
HAVING COUNT(*) > 5;
SELECT Branch_no, COUNT(*) AS Total_Employees
FROM Employee
GROUP BY Branch_no
HAVING COUNT(*) > 2;

-- 11. Retrieve the names of employees who manage branches and their respective branch addresses.
SELECT Employee.Emp_name, Branch.Branch_address
FROM Employee
JOIN Branch ON Employee.Emp_Id = Branch.Manager_Id;


-- 12. Display the names of customers who have issued books with a rental price higher than Rs. 25.
SELECT Customer.Customer_name
FROM IssueStatus
JOIN Books ON IssueStatus.Isbn_book = Books.ISBN
JOIN Customer ON IssueStatus.Issued_cust = Customer.Customer_Id
WHERE Books.Rental_Price > 25;

-- insert more rows according to get most of the query results.
-- Insert data into Employee table
INSERT INTO Employee (Emp_Id, Emp_name, Position, Salary, Branch_no) VALUES
(105, 'Eve Clark', 'Assistant', 52000, 1),
(106, 'Frank Wright', 'Librarian', 48000, 1),
(107, 'Grace Hall', 'Librarian', 53000, 2),
(108, 'Henry Adams', 'Assistant', 55000, 2),
(109, 'Ivy Baker', 'Manager', 80000, 2),
(110, 'Jack Carter', 'Assistant', 49000, 1);

-- Insert data into Books table
INSERT INTO Books (ISBN, Book_title, Category, Rental_Price, Status, Author, Publisher) VALUES
('978-0-11-111111-1', 'Modern History', 'History', 28.00, 'yes', 'Author E', 'Publisher E'),
('978-0-22-222222-2', 'Classic Literature', 'Fiction', 22.00, 'no', 'Author F', 'Publisher F'),
('978-0-33-333333-3', 'Biography of A', 'Biography', 18.00, 'yes', 'Author G', 'Publisher G'),
('978-0-44-444444-4', 'Mystery Novel', 'Mystery', 27.00, 'yes', 'Author H', 'Publisher H'),
('978-0-55-555555-5', 'Poetry Collection', 'Poetry', 12.00, 'no', 'Author I', 'Publisher I'),
('978-0-66-666666-6', 'World War II', 'History', 32.00, 'yes', 'Author J', 'Publisher J');

-- Insert data into Customer table
INSERT INTO Customer (Customer_Id, Customer_name, Customer_address, Reg_date) VALUES
(5, 'Aaliya Khan', '202 Lake Rd, City A', '2022-05-05'),
(6, 'William Lee', '303 Park Ave, City B', '2021-12-12'),
(7, 'Sophia Wilson', '404 Oak St, City A', '2023-04-04'),
(8, 'Benjamin Moore', '505 Pine St, City B', '2020-10-10'),
(9, 'Oliver Thomas', '606 Cedar St, City A', '2022-03-03'),
(10, 'Isabella Martin', '707 Birch St, City B', '2023-06-06');

-- Insert data into IssueStatus table
INSERT INTO IssueStatus (Issue_Id, Issued_cust, Issued_book_name, Issue_date, Isbn_book) VALUES
(3, 3, 'Historical Analysis', '2023-06-20', '978-0-00-000000-0'),
(4, 4, 'Modern History', '2023-06-25', '978-0-11-111111-1');

-- Insert data into ReturnStatus table
INSERT INTO ReturnStatus (Return_Id, Return_cust, Return_book_name, Return_date, Isbn_book2) VALUES
(1, 1, 'Fictional Story', '2023-06-10', '978-3-16-148410-0'),
(2, 2, 'Scientific Discovery', '2023-06-25', '978-0-12-345678-9');

