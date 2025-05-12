# Pocket Ninja Budgeting Application

Pocket Ninja is a comprehensive personal finance management application that helps users track expenses, set budget goals, and manage their finances more effectively.

## Features

- **User Management**: Register and login with secure authentication
- **Dashboard**: View financial overview with income, expenses, and budget progress
- **Budget Tracking**: Set monthly budget goals and track spending by category
- **Expense Tracking**: Record and categorize transactions with detailed information
- **Account Management**: Manage multiple financial accounts in one place
- **Category Management**: Customize spending categories with icons and colors
- **Financial Insights**: Visualize spending patterns and financial progress

## Tech Stack

### Frontend
- **Framework**: React.js with TypeScript
- **State Management**: React Query for server state
- **Routing**: Wouter for client-side routing
- **Forms**: React Hook Form with Zod validation
- **UI Components**: Shadcn UI components with Tailwind CSS
- **Icons**: Lucide React for interface icons

### Backend
- **Server**: Express.js with TypeScript
- **Database**: PostgreSQL with Drizzle ORM
- **Authentication**: Session-based authentication with Passport.js
- **API**: RESTful API architecture

## Database Structure

The application uses a PostgreSQL database with the following tables:

### Users Table
```
Column        Type        Nullable    Description
-----------   ----------  ----------  -----------------------------
id            integer     not null    Primary key
username      text        not null    User's login name
password      text        not null    User's password (hashed)
email         text        nullable    User's email address
first_name    text        nullable    User's first name
last_name     text        nullable    User's last name
```

### Categories Table
```
Column         Type             Nullable    Description
------------   --------------   ----------  -----------------------------
id             integer          not null    Primary key
user_id        integer          not null    Foreign key to users table
name           text             not null    Category name
icon           text             not null    Icon identifier for display
color          text             not null    Color code for display
budget_limit   double precision not null    Monthly spending limit
```

### Accounts Table
```
Column         Type             Nullable    Description
------------   --------------   ----------  -----------------------------
id             integer          not null    Primary key
user_id        integer          not null    Foreign key to users table
name           text             not null    Account name
type           text             not null    Account type (checking, savings, etc.)
balance        double precision not null    Current balance
icon           text             nullable    Icon identifier for display
last_four      text             nullable    Last four digits of account number
```

### Transactions Table
```
Column          Type                     Nullable    Description
-------------   ----------------------   ----------  -----------------------------
id              integer                  not null    Primary key
user_id         integer                  not null    Foreign key to users table
account_id      integer                  not null    Foreign key to accounts table
category_id     integer                  not null    Foreign key to categories table
amount          double precision         not null    Transaction amount
description     text                     not null    Transaction description
date            timestamp                not null    Transaction date and time
type            text                     not null    Transaction type (income/expense)
receipt_image   text                     nullable    URL to receipt image
```

### Budgets Table
```
Column         Type              Nullable    Description
------------   --------------    ----------  -----------------------------
id             integer           not null    Primary key
user_id        integer           not null    Foreign key to users table
month          integer           not null    Budget month (1-12)
year           integer           not null    Budget year
min_goal       double precision  nullable    Minimum savings goal
max_goal       double precision  not null    Maximum spending limit
```

## Setting Up the Database

The application uses Drizzle ORM to interact with the PostgreSQL database. The database can be set up using the following steps:

1. **Database Connection**: The application connects to PostgreSQL using the `DATABASE_URL` environment variable.

2. **Schema Definition**: The database schema is defined in `shared/schema.ts` using Drizzle ORM schema declarations.

3. **Migrations**: Run the following command to push the schema to the database:
   ```
   npm run db:push
   ```

4. **Storage Implementation**: The database operations are implemented in `server/storage.ts` via the `DatabaseStorage` class.

## Kotlin (Android) Implementation

For developers who want to implement a similar database structure in a Kotlin Android application, the following approach can be used with Room Database:

1. **Add Room Dependencies** to your app's `build.gradle`:
   ```gradle
   dependencies {
       def room_version = "2.5.0"
       implementation "androidx.room:room-runtime:$room_version"
       implementation "androidx.room:room-ktx:$room_version"
       kapt "androidx.room:room-compiler:$room_version"
   }
   ```

2. **Create Entity Classes** for each table (User, Category, Account, Transaction, Budget).

3. **Define Data Access Objects (DAOs)** with methods for CRUD operations.

4. **Create a Database Class** that brings everything together.

5. **Set Up Repositories** for clean architecture.

## Getting Started

1. Clone the repository
2. Install dependencies:
   ```
   npm install
   ```
3. Set up environment variables (create a `.env` file):
   ```
   DATABASE_URL=postgres://<username>:<password>@<host>:<port>/<database_name>
   ```
4. Set up the database:
   ```
   npm run db:push
   ```
5. Start the development server:
   ```
   npm run dev
   ```

## API Endpoints

The application provides the following API endpoints:

### Authentication
- `POST /api/auth/register` - Register a new user
- `POST /api/auth/login` - Log in an existing user
- `POST /api/auth/logout` - Log out the current user
- `GET /api/auth/me` - Get the current authenticated user

### Categories
- `GET /api/categories` - Get all categories for the authenticated user
- `POST /api/categories` - Create a new category
- `GET /api/categories/:id` - Get a specific category
- `PATCH /api/categories/:id` - Update a category
- `DELETE /api/categories/:id` - Delete a category

### Accounts
- `GET /api/accounts` - Get all accounts for the authenticated user
- `POST /api/accounts` - Create a new account
- `GET /api/accounts/:id` - Get a specific account
- `PATCH /api/accounts/:id` - Update an account
- `DELETE /api/accounts/:id` - Delete an account

### Transactions
- `GET /api/transactions` - Get all transactions for the authenticated user
- `POST /api/transactions` - Create a new transaction
- `GET /api/transactions/:id` - Get a specific transaction
- `PATCH /api/transactions/:id` - Update a transaction
- `DELETE /api/transactions/:id` - Delete a transaction

### Budgets
- `GET /api/budgets/:month/:year` - Get budget for a specific month and year
- `POST /api/budgets` - Create a new budget
- `PATCH /api/budgets/:id` - Update a budget

### Stats
- `GET /api/stats/spending/:month/:year` - Get total spending for a month
- `GET /api/stats/income/:month/:year` - Get total income for a month
- `GET /api/stats/categories/:month/:year` - Get spending by category for a month

## License

This project is licensed under the MIT License - see the LICENSE file for details.