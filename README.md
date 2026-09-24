# PROJECT MASTER SPECIFICATION

## Project

Advanced Library Management System

## Role

Act as a Senior Laravel Full-Stack Architect and Developer.

Build this application using production-quality architecture. Do not generate the entire project in one response. Work module-by-module and maintain architectural consistency throughout the project.

## Technology

* Laravel 12
* PHP 8.3+
* MySQL 8+
* Blade
* Tailwind CSS
* Alpine.js where appropriate
* Eloquent ORM
* Laravel Form Requests
* Policies / Gates
* Service classes for business logic
* Database transactions
* PHPUnit / Pest tests
* Barcode scanner integration
* Responsive admin UI

## User Roles

### Admin

Can manage:

* Dashboard
* Books
* Book copies
* Authors
* Categories
* Members
* Loan transactions
* Users
* Reports
* System settings

### Staff

Can:

* Search members
* Scan member ID
* Scan book barcode
* Borrow books
* Return books
* View transactions
* View member borrowing history

## Core Business Rules

### Borrowing Limit

A member can have a maximum of 2 active borrowed book copies.

Example:

Member has:

* Book A = borrowed
* Book B = borrowed

The member cannot borrow another book.

If Book A is returned:

Member has:

* Book A = returned
* Book B = borrowed

The member may borrow one additional book.

An overdue but unreturned book still counts toward the 2-book limit.

### Borrowing Period

Borrowing period = 3 days.

Example:

Borrowed:
2026-09-01

Due:
2026-09-04

### Penalty

Penalty = ₱5 per overdue day.

Example:

Due date:
September 4

Returned:
September 7

Overdue:
3 days

Penalty:
3 × ₱5 = ₱15

### Book Copy Status

Each physical copy must have its own barcode/book tag.

Format:

CAL-0001
CAL-0002
CAL-0003

Possible statuses:

* available
* borrowed
* damage
* lost
* missing

A copy with status damage, lost, or missing cannot be borrowed.

## Database Relationships

Author:
hasMany Books

Category:
hasMany Books

Book:
belongsTo Author
belongsTo Category
hasMany BookCopies

BookCopy:
belongsTo Book
hasMany LoanTransactions

Member:
hasMany LoanTransactions

LoanTransaction:
belongsTo Member
belongsTo BookCopy
belongsTo User

User:
hasMany LoanTransactions

## Main Modules

1. Authentication
2. Dashboard
3. Books
4. Book Copies / Barcode
5. Authors
6. Categories
7. Members
8. Borrowing
9. Returning
10. Penalties
11. Transaction History
12. Reports
13. User Management
14. Audit Logs

## Book Management

Books must support:

* Create
* Read
* Update
* Delete
* Search
* Filter
* Pagination
* Author
* Category
* ISBN
* Publisher
* Publication year
* Description

Book copies must support:

* Generate barcode
* Book tag
* Copy status
* Condition notes
* Search
* Filter

## Transaction Workflow

Borrow:

1. Scan member ID.
2. Find member.
3. Validate member status.
4. Count active loans.
5. Reject if active loans >= 2.
6. Scan book barcode.
7. Find book copy.
8. Verify copy status = available.
9. Create loan transaction.
10. Set due date = borrowed date + 3 days.
11. Change copy status to borrowed.
12. Commit database transaction.

Return:

1. Scan book barcode.
2. Find active loan.
3. Calculate overdue days.
4. Calculate penalty.
5. Set returned_at.
6. Set transaction status to returned.
7. Set book copy status to available.
8. Save penalty.
9. Commit transaction.

## Architecture Requirements

Use:

* Controllers for HTTP handling
* Form Requests for validation
* Services for business logic
* Eloquent models for relationships
* Policies for authorization
* Resources where appropriate
* Database transactions for borrow/return
* Enums for statuses
* Migrations
* Seeders
* Factories
* Tests

Do NOT put complex business logic directly inside Blade templates.

Do NOT put complex business logic directly inside controllers.

## Development Method

Do not build everything at once.

Build in this exact order:

Phase 1:
Project setup + authentication

Phase 2:
Database migrations + models + relationships

Phase 3:
Books + authors + categories

Phase 4:
Book copies + barcode

Phase 5:
Members

Phase 6:
Borrow transaction

Phase 7:
Return transaction + penalty

Phase 8:
Dashboard

Phase 9:
Reports

Phase 10:
Authorization + audit logs

Phase 11:
Testing

Phase 12:
Security review

Phase 13:
UI/UX refinement

After each phase:

1. Show files created/modified.
2. Explain architecture.
3. Provide complete code for that phase.
4. Check dependencies with previous phases.
5. Identify possible bugs.
6. Provide test cases.
7. Wait for approval before moving to the next phase.

Never silently change the database architecture.

If a requirement is ambiguous, identify the ambiguity and make a reasonable recommendation before implementing it.
