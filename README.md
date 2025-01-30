## Database Schema

The system is based on the following schema: `library_management`. Below are the tables included in the schema and their functions:

### Tables

1. **user_login**  
   Stores user login details for the library system.
   - `user_id`: Unique identifier for the user.
   - `user_password`: Password for the user.
   - `first_name`, `last_name`: User’s name.
   - `sign_up_on`: Date of user registration.
   - `email_id`: User's email address.

2. **publisher**  
   Stores information about the book publishers.
   - `publisher_id`: Unique identifier for the publisher.
   - `publisher`: Name of the publisher.
   - `distributor`: Distributor details.
   - `releases_count`: Number of books released by the publisher.
   - `last_release`: Date of the publisher's last book release.

3. **author**  
   Stores information about authors.
   - `author_id`: Unique identifier for the author.
   - `first_name`, `last_name`: Author’s name.
   - `publications_count`: Number of publications by the author.

4. **books**  
   Stores information about books available in the library.
   - `book_id`: Unique identifier for the book.
   - `book_code`: A unique code for identifying the book.
   - `book_name`: Name of the book.
   - `author_id`: References `author_id` from the **author** table.
   - `publisher_id`: References `publisher_id` from the **publisher** table.
   - `book_version`: Version of the book.
   - `release_date`: Date the book was released.
   - `available_from`: Date the book is available in the library.
   - `is_available`: Indicates if the book is currently available.

5. **staff**  
   Stores information about library staff.
   - `staff_id`: Unique identifier for the staff.
   - `first_name`, `last_name`: Staff’s name.
   - `staff_role`: Role of the staff (e.g., librarian).
   - `start_date`: Date when the staff joined the library.
   - `last_date`: Date when the staff left the library (if applicable).
   - `is_active`: Indicates if the staff is currently active.
   - `work_shift_start`: Start time of the staff’s work shift.
   - `work_shift_end`: End time of the staff’s work shift.

6. **readers**  
   Stores information about library readers (members).
   - `reader_id`: Unique identifier for the reader.
   - `first_name`, `last_name`: Reader’s name.
   - `registered_on`: Date when the reader registered.
   - `books_issued_total`: Total number of books issued by the reader.
   - `books_issued_current`: Number of books currently issued.
   - `is_issued`: Indicates if the reader currently has issued books.
   - `last_issue_date`: Date the last book was issued.
   - `total_fine`: Total fine accumulated by the reader.
   - `current_fine`: Fine currently due by the reader.

7. **books_issue**  
   Tracks books that have been issued to readers.
   - `issue_id`: Unique identifier for the issue.
   - `book_id`: References `book_id` from the **books** table.
   - `issued_to`: References `reader_id` from the **readers** table.
   - `issued_on`: Date the book was issued.
   - `return_on`: Date the book is due to be returned.
   - `current_fine`: Fine for the book if it is not returned on time.
   - `fine_paid`: Indicates if the fine has been paid.
   - `payment_transaction_id`: Transaction ID of the fine payment.

8. **settings**  
   Stores the system settings, such as the number of books a reader can issue and the fine per day for late returns.
   - `book_issue_count_per_reader`: Maximum number of books a reader can issue.
   - `fine_per_day`: Fine per day for overdue books.
   - `book_return_in_days`: Number of days allowed to return a book.

---

## Installation

1. **Clone the Repository**
   ```bash
   git clone https://github.com/SwarnaSunapral/SQL-Projects.git
   cd SQL-Projects

   ```

2. **Set Up the Database**  
   Run the provided SQL scripts to create the schema and tables in your PostgreSQL or MySQL database.
   
   To create the schema and tables, execute the SQL script:

   ```bash
   psql -U your_username -d your_database -f Library_Management.sql

   ```

3. **Configuration**  
   Ensure that your database server is running, and configure the database connection if needed.

---

## Features

- **User Authentication**: Users can sign up, log in, and access their library account.
- **Books Management**: Admins can add, update, or remove books in the library.
- **Books Issuing**: Readers can issue books, and the system tracks due dates and fines.
- **Fine Management**: The system calculates and tracks fines for overdue books.
- **Staff Management**: Library staff management, including roles and work shifts.
- **Publisher and Author Management**: Keep track of publishers and authors associated with books.
- **Settings**: Configure library settings like fine rates and book issue limits.

---

## Usage

- **Issue a Book to a Reader**  
   Example query to issue a book:
   ```sql
   INSERT INTO library_management.books_issue (book_id, issued_to, issued_on, return_on, current_fine)
   VALUES ('book_id_001', 'reader_id_123', '2025-01-10', '2025-01-20', 0.0);
   ```

- **Update Fine for an Overdue Book**  
   Example query to update the fine:
   ```sql
   UPDATE library_management.books_issue
   SET current_fine = current_fine + (SELECT fine_per_day FROM library_management.settings)
   WHERE return_on < CURRENT_DATE AND fine_paid = FALSE;
   ```

---

## Contributing

Feel free to fork this project, make improvements, and submit pull requests. If you encounter any issues, open an issue in the repository.
