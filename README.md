# task-8

##  Task 8 – Stored Procedures and Functions

###  Objective:

To design and implement **reusable SQL blocks** using `STORED PROCEDURES` and `FUNCTIONS` to encapsulate logic, simplify queries, and promote clean database programming.

---

### Tools Used:

* **MySQL Workbench** (recommended)

---

###  Deliverables:

* 1 Stored Procedure: `GetBorrowerInfo`
* 1 Stored Function: `GetBookBorrowCount`
* Usage examples included for both

---

###  Definitions:

#### 1 Stored Procedure: `GetBorrowerInfo`

* **Input:** `borrower_id` (INT)
* **Output:** Name, Email, and Total Books Borrowed
* **Logic:** Joins `Borrowers` with `BorrowRecords`, filters by `BorrowerID`

```sql
CREATE PROCEDURE GetBorrowerInfo(IN borrower_id INT)
BEGIN
    SELECT 
        b.Name,
        b.Email,
        COUNT(r.RecordID) AS TotalBooksBorrowed
    FROM Borrowers b
    LEFT JOIN BorrowRecords r ON b.BorrowerID = r.BorrowerID
    WHERE b.BorrowerID = borrower_id
    GROUP BY b.Name, b.Email;
END;
```

**Usage:**

```sql
CALL GetBorrowerInfo(201);
```

---

#### 2 Stored Function: `GetBookBorrowCount`

* **Input:** `book_id` (INT)
* **Returns:** INT (Number of times the book was borrowed)
* **Logic:** Counts entries in `BorrowRecords` where `BookID = book_id`

```sql
CREATE FUNCTION GetBookBorrowCount(book_id INT) 
RETURNS INT
BEGIN
    DECLARE borrow_count INT;
    SELECT COUNT(*) INTO borrow_count
    FROM BorrowRecords
    WHERE BookID = book_id;
    RETURN borrow_count;
END;
```

**Usage:**

```sql
SELECT Title, GetBookBorrowCount(BookID) AS TimesBorrowed FROM Books;
```

---

###  Why Use Them?

* **Stored Procedures:** Encapsulate logic for reuse and simplify external code
* **Functions:** Return computed values in queries, like a formula or metric
* **Security:** Limit user permissions to call procedures instead of exposing raw tables

---

### 📝 Notes:

* Always `DROP PROCEDURE/FUNCTION IF EXISTS` before creation during testing.
* MySQL Workbench is required; SQLite does not support this feature.


