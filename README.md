# Bank Management System

## Project Description
The **Bank Management System** is a console-based C++ application designed to manage customer accounts in a bank. This project covers essential banking functionalities, such as creating accounts, depositing and withdrawing money, transferring funds, generating account statements, and managing customer records. It is a simple and effective tool for understanding file handling, classes, and object-oriented programming concepts in C++.

## Features
### 1) Create New Account
Add new customer details, including name, address, phone number, initial balance, and account type.

### 2) Deposit Money
Allows users to deposit money into a customer's account.

### 3) Withdraw Money
Enables users to withdraw money from a customer's account, ensuring the balance does not fall below the required amount.

### 4) Transfer Money
Facilitate money transfer between two accounts.

### 5) Generate Account Statement
Retrieve and display detailed information about a specific account.

### 6) View All Customers
Display a list of all customers with their account details.

### 7) Search Customers by Balance
View customer accounts with balances greater than a specified amount.

### 8) Modify Customer Details
Update an existing customer's information.

### 9) Delete Account
Remove a customer’s account details from the system.

### Technical Details
- Programming Language: C++
- File Handling: Customer data is stored in and retrieved from a binary file (**bank.txt**). Temporary files are used for updating and deleting records.
- Core Concepts Used:
    - Classes and Objects
    - File I/O
    - Member Functions
    - Basic Data Validation
- Headers Used:
    - <iostream> for input and output operations.
    - <iomanip> for formatting output.
    - <fstream> for file handling.
    - <string.h> for string manipulation.

## How to Run
1) Setup:

- Ensure a C++ compiler (like GCC or Turbo C++) is installed.
- Place the source code in a .cpp file.

2) Compile:

- Use the command: g++ -o bank_management_system bank_management_system.cpp
(Replace bank_management_system.cpp with the actual filename.)

3) Run:

- Execute the compiled file: ./bank_management_system
- Follow the instructions on the console.

4) Data Files:

- Customer data is saved in a binary file named bank.txt. Ensure this file is accessible in the same directory as the executable.

## Menu Options
Upon running the program, the following menu is displayed:

1) Add a customer
2) Account-to-account transfer
3) Account statement generation
4) Cash deposit
5) Cash withdrawal
6) Modify customer details
7) Delete customer details
8) Display all customers
9) Display customers with balances above a threshold
10) Exit

## Limitations
- The system does not implement user authentication.
- File storage is not encrypted, posing a security risk for sensitive data.
- No graphical user interface (GUI) or web interface.

## Future Enhancements
- Add a login system for administrators and customers.
- Implement database integration for better data management.
- Develop a graphical user interface (GUI) for ease of use.
- Introduce transaction history tracking.

## Author
**Name**: **Jatin Sareen**

**Roll No**: 16

**Class**: XII B

## Acknowledgments
This project was developed as a learning exercise for implementing OOP concepts, file handling, and system design in C++.