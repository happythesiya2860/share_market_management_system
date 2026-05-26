# share_market_management_system
# 📈 Share Market Management System

A **console-based Share Market Management System** built in Java with MySQL, simulating real-world stock trading operations. This system allows users to register, manage funds, buy/sell stocks, apply for IPOs, maintain a watchlist, and track transaction history — all through a secure, PIN-protected CLI interface.

---

## 📋 Table of Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Database Schema](#-database-schema)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Database Setup](#database-setup)
  - [Running the Project](#running-the-project)
- [Module Breakdown](#-module-breakdown)
- [Data Structures Used](#-data-structures-used)
- [Validations](#-validations)
- [Future Enhancements](#-future-enhancements)
- [Author](#-author)

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔐 User Registration | Register with PAN, Aadhaar, mobile, bank account & IFSC |
| 🔑 Secure Login | PIN-based authentication (4-digit security PIN) |
| 💰 Fund Management | Add or withdraw funds, view current balance |
| 📊 Stock Trading | Buy & Sell stocks with real-time price simulation (±5% fluctuation) |
| 🗂️ Portfolio | View holdings with avg price, quantity, invested amount & current value |
| 📋 Watchlist | Create, manage, and view personalized stock watchlists |
| 🏦 IPO | Browse and apply for upcoming IPOs |
| 🧾 Order History | View today's buy/sell orders (stack-based, LIFO) |
| 📜 Transaction History | Full fund transaction log using a linked list |
| 🛡️ Duplicate Guard | SQL triggers prevent duplicate PAN/Aadhaar/Mobile registration |

---

## 🛠️ Tech Stack

- **Language:** Java (JDK 8+)
- **Database:** MySQL 8.x
- **Connectivity:** JDBC (`com.mysql.cj.jdbc.Driver`)
- **IDE:** IntelliJ IDEA
- **Build:** IntelliJ IDEA module structure (`.iml`)

---

## 📁 Project Structure

```
Share Market Management/
│
├── src/
│   ├── pac1/   BUY.java               → Buy stock logic
│   ├── pac2/   SELL.java              → Sell stock logic
│   ├── pac3/   portfolio.java         → Portfolio display & value update
│   ├── pac4/   IPO_Admin.java         → Admin: Add/Remove/View IPOs
│   ├── pac5/   IPOUser.java           → User: Browse & Apply for IPOs
│   ├── pac6/   ShareMarketManagementSystem.java  → Main entry point (menu)
│   ├── pac7/   NewUserRegistration.java           → User registration
│   ├── pac8/   searchAndWatchlist.java            → Search stocks & watchlist
│   ├── pac9/   fund.java              → Add/Withdraw/View funds
│   ├── pac10/  Order.java             → Order history (Stack DSA)
│   └── pac11/  trancationhistory.java → Transaction log (Linked List DSA)
│
├── out/production/   → Compiled .class files
├── Share Market Management.iml
└── .gitignore
```

---

## 🗄️ Database Schema

The application connects to a MySQL database named **`share-market`**.

### Tables Created Automatically at Runtime

```sql
-- Stores client personal & bank details
CREATE TABLE CLIENT_DATA (
    CLIENT_NAME           VARCHAR(50),
    CLIENT_PAN            VARCHAR(50) UNIQUE,
    CLIENT_AADHAR         VARCHAR(50) UNIQUE,
    CLIENT_MOBILE_NUMBER  VARCHAR(50) UNIQUE,
    CLIENT_ACCOUNT_NUMBER VARCHAR(50),
    CLIENT_IFSC           VARCHAR(50)
);

-- Stores 4-digit security PINs
CREATE TABLE PIN (
    CLIENT_MOBILE_NUMBER VARCHAR(50),
    PIN                  INT
);

-- Stores user wallet balance
CREATE TABLE FUND (
    CLIENT_MOBILE_NUMBER VARCHAR(50),
    FUND                 DOUBLE
);

-- Stores user stock holdings
CREATE TABLE PORTFOLIO (
    STOCK_NAME           VARCHAR(50),
    STOCK_AVG_PRICE      DOUBLE,
    STOCK_QUANTITY       INT,
    STOCK_INVEST         DOUBLE,
    CURRENT_VALUE        DOUBLE,
    TOTAL_INVESTMENT     DOUBLE,
    TOTAL_VALUE          DOUBLE,
    CLIENT_MOBILE_NUMBER VARCHAR(50)
);

-- IPO applications submitted by users
CREATE TABLE IPOAPPLY (
    IPO_NAME   VARCHAR(50),
    CLIENT_PAN VARCHAR(50),
    LOT        INT,
    UPI_ID     VARCHAR(50)
);

-- Logs duplicate registration attempts (via trigger)
CREATE TABLE DUPLICATE_LOG (
    ATTEMPT_TIME         TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    CLIENT_PAN           VARCHAR(50),
    CLIENT_AADHAR        VARCHAR(50),
    CLIENT_MOBILE_NUMBER VARCHAR(50)
);
```

### Pre-populated Tables (Manual Setup Required)

```sql
-- Stock data table — populate manually before running
CREATE TABLE STOCK_DATA (
    STOCK_NAME  VARCHAR(50),
    STOCK_PRICE DOUBLE
);

-- IPO data — managed via IPO Admin module (pac4)
CREATE TABLE IPO (
    IPO_NAME        VARCHAR(50),
    BID_PRICE       DOUBLE,
    LOT_SIZE        INT,
    START_DATE      VARCHAR(50),
    LAST_DATE       VARCHAR(50),
    ALLOTMENT_DATE  VARCHAR(50),
    IPO_SIZE        DOUBLE,
    ONE_LOT_AMOUNT  DOUBLE,
    IPO_TYPE        VARCHAR(50)
);
```

---

## 🚀 Getting Started

### Prerequisites

- Java JDK 8 or above
- MySQL Server 8.x
- MySQL Connector/J (JDBC Driver JAR)
- IntelliJ IDEA (recommended) or any Java IDE

### Database Setup

1. Start your MySQL server.
2. Create the database:
   ```sql
   CREATE DATABASE `share-market`;
   USE `share-market`;
   ```
3. Create and seed the `STOCK_DATA` table with some initial stock entries:
   ```sql
   CREATE TABLE STOCK_DATA (STOCK_NAME VARCHAR(50), STOCK_PRICE DOUBLE);

   INSERT INTO STOCK_DATA VALUES ('RELIANCE', 2950.50);
   INSERT INTO STOCK_DATA VALUES ('TCS', 3800.00);
   INSERT INTO STOCK_DATA VALUES ('INFOSYS', 1450.75);
   INSERT INTO STOCK_DATA VALUES ('HDFC', 1600.00);
   INSERT INTO STOCK_DATA VALUES ('WIPRO', 480.30);
   ```
4. The rest of the tables are created **automatically** when you run the application.

### Running the Project

1. **Clone the repository:**
   ```bash
   git clone https://github.com/<your-username>/Share-Market-Management.git
   cd Share-Market-Management
   ```

2. **Add MySQL Connector JAR** to your project classpath:
   - Download `mysql-connector-j-8.x.x.jar` from [MySQL Downloads](https://dev.mysql.com/downloads/connector/j/)
   - In IntelliJ IDEA: `File → Project Structure → Modules → Dependencies → + JAR`

3. **Update DB credentials** (if your MySQL has a password):
   ```java
   // In ShareMarketManagementSystem.java and other files
   Connection con = DriverManager.getConnection(
       "jdbc:mysql://localhost:3306/share-market", "root", "YOUR_PASSWORD"
   );
   ```

4. **Run the main class:**
   ```
   pac6.ShareMarketManagementSystem
   ```

---

## 🧩 Module Breakdown

### `pac6` — Main System (Entry Point)
- Presents the main menu: New Registration / Login / Exit
- Validates mobile number format (10 digits, starts with 6/7/8/9)
- After login, routes user to: Watchlist, Fund, Buy, Sell, Portfolio, IPO, Orders, Transaction History

### `pac7` — New User Registration
- Collects: mobile, name, Aadhaar (12-digit), PAN (format: `ABCDE1234F`), bank account & IFSC
- Creates database tables if not already present
- Sets up a MySQL **trigger** (`log_duplicate_attempts`) to catch and log duplicate registrations
- Detects exact duplicate field (PAN / Aadhaar / Mobile) and reports it

### `pac1` — Buy Stocks
- Lists all available stocks from `STOCK_DATA`
- Simulates real-time price with ±5% random fluctuation
- Suggests max purchasable quantity based on available funds
- Updates `PORTFOLIO` (avg price, quantity, invested amount, current value)
- Deducts amount from `FUND` table
- Pushes buy order to `Order` stack (pac10)

### `pac2` — Sell Stocks
- Validates stock exists in user's portfolio
- Applies ±5% random price fluctuation
- Updates portfolio (reduces qty, recalculates avg, invested amount)
- Credits fund back to wallet
- Pushes sell order to `Order` stack

### `pac3` — Portfolio
- Displays all holdings for logged-in user
- Dynamically recalculates current values with real-time simulation
- Shows avg buy price, quantity, invested, and current value

### `pac8` — Search & Watchlist
- Create named watchlists (stored as dynamic tables in MySQL)
- Add/Remove stocks to/from a watchlist
- View stocks in any watchlist

### `pac9` — Fund Management
- Add funds to wallet
- Withdraw funds (validates sufficient balance)
- View current wallet balance
- All fund movements recorded in `trancationhistory` (pac11)

### `pac4` — IPO Admin
- Standalone admin tool to: Add IPO, Remove IPO, View all IPOs
- Stores IPO details: name, bid price, lot size, dates, size, type

### `pac5` — IPO User
- Browse all available IPOs
- Apply for an IPO by selecting lots and providing UPI ID
- Validates UPI ID format

### `pac10` — Order History (Stack)
- Implements a **stack** using a fixed-size array (capacity: 100)
- `pushorder()` adds buy/sell orders
- `display()` prints today's orders in LIFO order

### `pac11` — Transaction History (Linked List)
- Implements a **singly linked list**
- `insert()` appends each fund transaction (type + amount)
- `display()` traverses and prints all fund transactions

---

## 📐 Data Structures Used

| Data Structure | Used In | Purpose |
|---|---|---|
| **Stack** (Array-based) | `pac10 / Order.java` | Store and display today's buy/sell orders |
| **Singly Linked List** | `pac11 / trancationhistory.java` | Maintain chronological fund transaction log |

---

## ✅ Validations

| Field | Rule |
|---|---|
| Mobile Number | 10 digits, must start with 6, 7, 8, or 9 |
| Aadhaar Number | Exactly 12 numeric digits |
| PAN Number | Format: 5 letters + 4 digits + 1 letter (e.g., `ABCDE1234F`) |
| Security PIN | 4-digit integer |
| Stock Quantity | Cannot exceed affordable quantity based on current fund |
| Duplicate Check | PAN, Aadhaar, and Mobile must be unique across all users |

---

## 🔮 Future Enhancements

- [ ] GUI using JavaFX or Swing
- [ ] Real-time stock price via an external API
- [ ] OTP-based login (replacing PIN)
- [ ] Historical price charts for each stock
- [ ] Admin dashboard with analytics
- [ ] Export portfolio/transaction reports as PDF
- [ ] Multi-threaded price update simulation
- [ ] Password hashing for security PIN storage

---
---

> ⭐ If you found this project helpful, please give it a star on GitHub!
