# School Database Design Explanation

## 1. Table Explanations & Relationships

* **`students`**: Stores core information about individual students, including their first name, last name, and a unique email address.
* **`courses`**: Stores information about available academic courses, such as course names and unique course codes.
* **`enrolments`**: Serves as a junction (join) table connecting `students` and `courses` while holding enrolment-specific attributes like `grade`.

### Relationships
* **`students` to `enrolments` (One-to-Many):** One student can have multiple course enrolment records, but each enrolment record belongs to exactly one student.
* **`courses` to `enrolments` (One-to-Many):** One course can have multiple student enrolments, but each enrolment record belongs to exactly one course.
* **`students` to `courses` (Many-to-Many):** A single student can enroll in multiple courses, and a single course can contain multiple students.

### Why a Join Table is Needed
Relational databases cannot directly implement a many-to-many relationship cleanly without duplicating data or breaking normalisation rules. The `enrolments` join table breaks down the many-to-many relationship into two one-to-many relationships. It also provides a logical place to store data that depends on both entities simultaneously, such as a student's specific `grade` in a specific course.

---

## 2. Database Index

**Proposed Index:** `CREATE INDEX idx_students_email ON students(email);`

**Reasoning:** Email addresses are frequently queried during logins, user searches, and account lookups. Adding a B-Tree index on the `email` column speeds up `WHERE email = ...` lookup queries from an O(N) full table scan to an O(log N) index lookup as the user base grows.

---

## 3. SQL vs. NoSQL Decision

For this student enrollment system, **SQL (Relational Database)** is the superior choice. The system relies heavily on structured data with explicit relational ties between entities (students, courses, and enrolments). SQL databases offer native support for foreign key constraints, `JOIN` queries, and `UNIQUE` constraints that enforce strict data integrity—preventing orphan enrolment records or duplicate course registrations. While NoSQL databases scale well for unstructured document data, managing complex cross-entity relationships in NoSQL often requires manual application-level joins and risks data duplication.
