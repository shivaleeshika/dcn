# OOP Practice Problems – Complete Solutions

> **Environment:** Ubuntu 22.04+ | Java 17+ | g++ (C++17)

---

## Prerequisites – One-Time Setup

```bash
# Install Java
sudo apt update
sudo apt install default-jdk -y
java -version

# Install G++ (C++ compiler)
sudo apt install g++ -y
g++ --version
```

---

## How to Run (General Pattern)

### Java
```bash
# Compile
javac FileName.java

# Run
java ClassName

# For packages (run from parent directory)
javac converter/UnitConverter.java Q3.java
java Q3
```

### C++
```bash
# Compile
g++ -o output_name filename.cpp

# Run
./output_name
```

---

## Table of Contents

| # | Topic | Language |
|---|-------|----------|
| Q1 | Two Threads – Uppercase & Lowercase | Java |
| Q2 | Inheritance – Employee → Manager & Engineer | C++ |
| Q3 | Package – Unit Converter | Java |
| Q4/Q17 | Operator Overloading – Complex Number (Menu-Driven) | C++ |
| Q5 | Exception Handling – Student with File I/O | C++ |
| Q6 | Template – Generic Vector | C++ |
| Q7 | Exception Handling – Employee with File I/O | C++ |
| Q8/Q16 | Inheritance – Book → Textbook & Novel | C++ |
| Q9 | Inheritance – Shape → Rectangle & Triangle | Java |
| Q10 | Two Threads – Even & Odd Numbers | Java |
| Q11 | Operator Overloading – Complex Number (same as Q4) | C++ |
| Q12 | Interface – Sortable (BubbleSort & SelectionSort) | Java |
| Q13 | Inheritance – Vehicle → Car & Bike | Java |
| Q14 | Package – Calculator | Java |
| Q15 | Template – Generic ScoreList | C++ |
| Q19 | Virtual Functions – Employee Salary Polymorphism | C++ |
| Q20 | File Handling – Product Binary File | C++ |
| Q21 | Operator Overloading – Matrix (+, -, *) | C++ |
| Q22 | Exception Handling – BankAccount | C++ |
| Q23 | Template – Generic Stack | C++ |
| Q24 | Package – Hospital with Access Modifiers | Java |
| Q25 | Interface – Payment with Abstract Transaction | Java |
| Q26 | Two Threads – Primes & Fibonacci | Java |

---

---

# MULTITHREADING (Java)

---

## Q1 – Two Threads: Uppercase & Lowercase Letters

**Concept:** Extending `Thread` class, running two threads simultaneously.

```java
// File: Q1.java

class UpperCase extends Thread {
    public void run() {
        for (char c = 'A'; c <= 'Z'; c++) {
            System.out.print(c + " ");
        }
    }
}

class LowerCase extends Thread {
    public void run() {
        for (char c = 'a'; c <= 'z'; c++) {
            System.out.print(c + " ");
        }
    }
}

public class Q1 {
    public static void main(String[] args) {
        new UpperCase().start();
        new LowerCase().start();
    }
}
```

**Run:**
```bash
javac Q1.java
java Q1
```

---

## Q10 – Two Threads: Even & Odd Numbers (1–20)

**Concept:** Two threads printing even and odd numbers concurrently.

```java
// File: Q10.java

class EvenThread extends Thread {
    public void run() {
        for (int i = 2; i <= 20; i += 2) {
            System.out.println("Even: " + i);
        }
    }
}

class OddThread extends Thread {
    public void run() {
        for (int i = 1; i <= 20; i += 2) {
            System.out.println("Odd: " + i);
        }
    }
}

public class Q10 {
    public static void main(String[] args) {
        new EvenThread().start();
        new OddThread().start();
    }
}
```

**Run:**
```bash
javac Q10.java
java Q10
```

---

## Q26 – Two Threads: Prime Numbers (1–50) & Fibonacci (10 terms)

**Concept:** Two threads with `Thread.sleep()` to observe interleaving.

```java
// File: Q26.java

class PrimeThread extends Thread {
    public void run() {
        for (int n = 2; n <= 50; n++) {
            boolean prime = true;
            for (int i = 2; i <= n / 2; i++) {
                if (n % i == 0) { prime = false; break; }
            }
            if (prime) {
                System.out.println("Prime: " + n);
                try { Thread.sleep(500); } catch (InterruptedException e) {}
            }
        }
    }
}

class FibThread extends Thread {
    public void run() {
        int a = 0, b = 1;
        for (int i = 0; i < 10; i++) {
            System.out.println("Fib: " + a);
            int temp = a + b; a = b; b = temp;
            try { Thread.sleep(500); } catch (InterruptedException e) {}
        }
    }
}

public class Q26 {
    public static void main(String[] args) {
        new PrimeThread().start();
        new FibThread().start();
    }
}
```

**Run:**
```bash
javac Q26.java
java Q26
```

---

---

# INHERITANCE

---

## Q2 – Employee → Manager & Engineer (C++)

**Concept:** Single-level inheritance with `public` base class.

```cpp
// File: q2.cpp

#include <iostream>
using namespace std;

class Employee {
public:
    string name;
    int id;
    float salary;

    void input() {
        cout << "Name: ";
        cin >> name;
        cout << "ID: ";
        cin >> id;
        cout << "Salary: ";
        cin >> salary;
    }

    void display() {
        cout << "Name: " << name
             << " | ID: " << id
             << " | Salary: " << salary
             << endl;
    }
};

class Manager : public Employee {
public:
    string dept;
    int subordinates;

    void input() {
        Employee::input();
        cout << "Department: ";
        cin >> dept;
        cout << "Number of Subordinates: ";
        cin >> subordinates;
    }

    void display() {
        Employee::display();
        cout << "Department: " << dept
             << " | Subordinates: " << subordinates
             << endl;
    }
};

class Engineer : public Employee {
public:
    string project;
    string language;

    void input() {
        Employee::input();
        cout << "Project: ";
        cin >> project;
        cout << "Programming Language: ";
        cin >> language;
    }

    void display() {
        Employee::display();
        cout << "Project: " << project
             << " | Language: " << language
             << endl;
    }
};

int main() {
    Manager m;
    cout << "\n--- Manager Details ---\n";
    m.input();
    m.display();

    Engineer e;
    cout << "\n--- Engineer Details ---\n";
    e.input();
    e.display();

    return 0;
}
```

**Run:**
```bash
g++ -o q2 q2.cpp
./q2
```

---

## Q8 / Q16 – Book → Textbook & Novel (C++)

**Concept:** Inheritance with additional derived class members.

```cpp
// File: q8.cpp

#include <iostream>
using namespace std;

class Book {
public:
    string title;
    string isbn;
    float price;

    void input() {
        cout << "Title: ";
        cin >> title;
        cout << "ISBN: ";
        cin >> isbn;
        cout << "Price: ";
        cin >> price;
    }

    void display() {
        cout << "Title: " << title
             << " | ISBN: " << isbn
             << " | Price: Rs." << price
             << endl;
    }
};

class Textbook : public Book {
public:
    string subject;
    string level;

    void input() {
        Book::input();
        cout << "Subject: ";
        cin >> subject;
        cout << "Academic Level (e.g. Undergraduate): ";
        cin >> level;
    }

    void display() {
        Book::display();
        cout << "Subject: " << subject
             << " | Level: " << level
             << endl;
    }
};

class Novel : public Book {
public:
    string genre;
    string author;

    void input() {
        Book::input();
        cout << "Genre: ";
        cin >> genre;
        cout << "Author: ";
        cin >> author;
    }

    void display() {
        Book::display();
        cout << "Genre: " << genre
             << " | Author: " << author
             << endl;
    }
};

int main() {
    Textbook t;
    cout << "\n--- Textbook Details ---\n";
    t.input();
    t.display();

    Novel n;
    cout << "\n--- Novel Details ---\n";
    n.input();
    n.display();

    return 0;
}
```

**Run:**
```bash
g++ -o q8 q8.cpp
./q8
```

---

## Q9 – Shape → Rectangle & Triangle (Java)

**Concept:** Method overriding using inheritance.

```java
// File: Q9.java

class Shape {
    double dim1;
    double dim2;

    Shape(double a, double b) {
        dim1 = a;
        dim2 = b;
    }

    double area() {
        return 0;
    }
}

class Rectangle extends Shape {
    Rectangle(double a, double b) {
        super(a, b);
    }

    @Override
    double area() {
        return dim1 * dim2;
    }
}

class Triangle extends Shape {
    Triangle(double a, double b) {
        super(a, b);
    }

    @Override
    double area() {
        return 0.5 * dim1 * dim2;
    }
}

public class Q9 {
    public static void main(String[] args) {
        Shape r = new Rectangle(5, 3);
        Shape t = new Triangle(5, 3);

        System.out.println("Rectangle Area: " + r.area());
        System.out.println("Triangle Area : " + t.area());
    }
}
```

**Run:**
```bash
javac Q9.java
java Q9
```

---

## Q13 – Vehicle → Car & Bike (Java)

**Concept:** Method overriding with additional delay logic in subclasses.

```java
// File: Q13.java

class Vehicle {
    double speed;
    double distance;

    Vehicle(double s, double d) {
        speed = s;
        distance = d;
    }

    double travelTime() {
        return distance / speed;
    }
}

class Car extends Vehicle {
    Car(double s, double d) {
        super(s, d);
    }

    @Override
    double travelTime() {
        // Adds 0.5 hour traffic delay
        return super.travelTime() + 0.5;
    }
}

class Bike extends Vehicle {
    Bike(double s, double d) {
        super(s, d);
    }

    @Override
    double travelTime() {
        // Adds 1.0 hour for fuel stops/breaks
        return super.travelTime() + 1.0;
    }
}

public class Q13 {
    public static void main(String[] args) {
        Vehicle car  = new Car(60, 120);
        Vehicle bike = new Bike(40, 120);

        System.out.println("Car  travel time: " + car.travelTime()  + " hrs");
        System.out.println("Bike travel time: " + bike.travelTime() + " hrs");
    }
}
```

**Run:**
```bash
javac Q13.java
java Q13
```

---

---

# PACKAGES & ACCESS MODIFIERS (Java)

---

## Q3 – Unit Converter Package

**Concept:** Creating and importing user-defined packages.

```java
// File: converter/UnitConverter.java

package converter;

public class UnitConverter {

    public double kmToMiles(double km) {
        return km * 0.621371;
    }

    public double celsiusToFahrenheit(double c) {
        return (c * 9.0 / 5.0) + 32;
    }

    public double kgToPounds(double kg) {
        return kg * 2.20462;
    }

    public double litersToGallons(double liters) {
        return liters * 0.264172;
    }
}
```

```java
// File: Q3.java  (place this OUTSIDE the converter folder)

import converter.UnitConverter;
import java.util.Scanner;

public class Q3 {
    public static void main(String[] args) {
        UnitConverter uc = new UnitConverter();
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter distance in km: ");
        double km = sc.nextDouble();
        System.out.println(km + " km = " + uc.kmToMiles(km) + " miles");

        System.out.print("Enter temperature in Celsius: ");
        double c = sc.nextDouble();
        System.out.println(c + " C = " + uc.celsiusToFahrenheit(c) + " F");

        System.out.print("Enter weight in kg: ");
        double kg = sc.nextDouble();
        System.out.println(kg + " kg = " + uc.kgToPounds(kg) + " lbs");

        sc.close();
    }
}
```

**Run:**
```bash
# Folder structure required:
# .
# ├── Q3.java
# └── converter/
#     └── UnitConverter.java

mkdir converter
# Place UnitConverter.java inside converter/

javac converter/UnitConverter.java
javac Q3.java
java Q3
```

---

## Q14 – Calculator Package

**Concept:** Arithmetic operations inside a package.

```java
// File: calc/Calculator.java

package calc;

public class Calculator {

    public double add(double a, double b) {
        return a + b;
    }

    public double subtract(double a, double b) {
        return a - b;
    }

    public double multiply(double a, double b) {
        return a * b;
    }

    public double divide(double a, double b) {
        if (b == 0) {
            System.out.println("Error: Division by zero!");
            return 0;
        }
        return a / b;
    }
}
```

```java
// File: Q14.java

import calc.Calculator;
import java.util.Scanner;

public class Q14 {
    public static void main(String[] args) {
        Calculator c = new Calculator();
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter first number: ");
        double a = sc.nextDouble();
        System.out.print("Enter second number: ");
        double b = sc.nextDouble();

        System.out.println("Add      : " + c.add(a, b));
        System.out.println("Subtract : " + c.subtract(a, b));
        System.out.println("Multiply : " + c.multiply(a, b));
        System.out.println("Divide   : " + c.divide(a, b));

        sc.close();
    }
}
```

**Run:**
```bash
mkdir calc
# Place Calculator.java inside calc/

javac calc/Calculator.java
javac Q14.java
java Q14
```

---

## Q24 – Hospital Package with Access Modifiers

**Concept:** `protected` members accessible in subclass from another package.

```java
// File: hospital/Patient.java

package hospital;

public class Patient {
    protected String name;
    protected int age;
    protected String ailment;

    public Patient(String n, int a, String ail) {
        name    = n;
        age     = a;
        ailment = ail;
    }

    public void display() {
        System.out.println("Patient Name  : " + name);
        System.out.println("Age           : " + age);
        System.out.println("Ailment       : " + ailment);
    }
}
```

```java
// File: Q24.java

import hospital.Patient;

class Doctor extends Patient {
    String specialization;

    Doctor(String n, int a, String ail, String s) {
        super(n, a, ail);
        specialization = s;
    }

    @Override
    public void display() {
        super.display();
        System.out.println("Specialization: " + specialization);
    }
}

public class Q24 {
    public static void main(String[] args) {
        Doctor d = new Doctor("Dr. Mehta", 45, "Fever", "General Medicine");
        d.display();
    }
}
```

**Run:**
```bash
mkdir hospital
# Place Patient.java inside hospital/

javac hospital/Patient.java
javac Q24.java
java Q24
```

---

---

# OPERATOR OVERLOADING (C++)

---

## Q4 / Q11 / Q17 – Complex Number with operator+ (Menu-Driven)

> Q4, Q11, and Q17 are identical in logic. One solution covers all three.

**Concept:** Constructor overloading + `operator+` overloading with a menu loop.

```cpp
// File: q4.cpp

#include <iostream>
using namespace std;

class Complex {
public:
    float real;
    float imag;

    // Default constructor: 0 + 0i
    Complex(float r = 0, float i = 0) : real(r), imag(i) {}

    // Overload + operator
    Complex operator+(const Complex& c) {
        return Complex(real + c.real, imag + c.imag);
    }

    void display() {
        if (imag >= 0)
            cout << real << " + " << imag << "i" << endl;
        else
            cout << real << " - " << -imag << "i" << endl;
    }
};

int main() {
    Complex c1, c2, result;
    int choice;

    do {
        cout << "\n===== Complex Number Menu =====\n";
        cout << "1. Enter two complex numbers\n";
        cout << "2. Add them\n";
        cout << "3. Display both numbers\n";
        cout << "0. Exit\n";
        cout << "Choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter C1 (real imag): ";
                cin >> c1.real >> c1.imag;
                cout << "Enter C2 (real imag): ";
                cin >> c2.real >> c2.imag;
                break;

            case 2:
                result = c1 + c2;
                cout << "Sum: ";
                result.display();
                break;

            case 3:
                cout << "C1: "; c1.display();
                cout << "C2: "; c2.display();
                break;

            case 0:
                cout << "Exiting...\n";
                break;

            default:
                cout << "Invalid option!\n";
        }
    } while (choice != 0);

    return 0;
}
```

**Run:**
```bash
g++ -o q4 q4.cpp
./q4
```

---

## Q21 – 2×2 Matrix with +, -, * Overloading

**Concept:** Operator overloading for matrix arithmetic.

```cpp
// File: q21.cpp

#include <iostream>
using namespace std;

class Matrix {
    int m[2][2];

public:
    // Initialize all elements to 0
    Matrix() {
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                m[i][j] = 0;
    }

    void input() {
        cout << "Enter 4 elements row by row:\n";
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                cin >> m[i][j];
    }

    void display() {
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++)
                cout << m[i][j] << " ";
            cout << endl;
        }
    }

    Matrix operator+(const Matrix& b) {
        Matrix r;
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                r.m[i][j] = m[i][j] + b.m[i][j];
        return r;
    }

    Matrix operator-(const Matrix& b) {
        Matrix r;
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                r.m[i][j] = m[i][j] - b.m[i][j];
        return r;
    }

    Matrix operator*(const Matrix& b) {
        Matrix r;
        for (int i = 0; i < 2; i++)
            for (int j = 0; j < 2; j++)
                for (int k = 0; k < 2; k++)
                    r.m[i][j] += m[i][k] * b.m[k][j];
        return r;
    }
};

int main() {
    Matrix a, b;

    cout << "--- Matrix A ---\n";
    a.input();

    cout << "--- Matrix B ---\n";
    b.input();

    cout << "\nAddition:\n";
    (a + b).display();

    cout << "\nSubtraction:\n";
    (a - b).display();

    cout << "\nMultiplication:\n";
    (a * b).display();

    return 0;
}
```

**Run:**
```bash
g++ -o q21 q21.cpp
./q21
```

---

---

# EXCEPTION HANDLING (C++)

---

## Q5 – Student Class with DOB Validation + File I/O

**Concept:** `throw`, `try-catch`, file read/write with `fstream`.

```cpp
// File: q5.cpp

#include <iostream>
#include <fstream>
using namespace std;

class Student {
public:
    string name;
    int roll;
    int day, month, year;

    void input() {
        cout << "Name: ";
        cin >> name;
        cout << "Roll Number: ";
        cin >> roll;
        cout << "Date of Birth (day month year): ";
        cin >> day >> month >> year;

        if (day < 1 || day > 31)
            throw runtime_error("Invalid day! Must be between 1 and 31.");
        if (month < 1 || month > 12)
            throw runtime_error("Invalid month! Must be between 1 and 12.");
    }

    void saveToFile() {
        ofstream f("students.txt", ios::app);
        f << name << " " << roll << " "
          << day << "/" << month << "/" << year << "\n";
        cout << "Record saved successfully.\n";
    }

    void display() {
        cout << "Name : " << name
             << " | Roll: " << roll
             << " | DOB: " << day << "/" << month << "/" << year
             << endl;
    }
};

void loadFromFile() {
    ifstream f("students.txt");
    string line;
    cout << "\n--- Student Records from File ---\n";
    while (getline(f, line)) {
        cout << line << endl;
    }
}

int main() {
    Student s;
    try {
        s.input();
        s.display();
        s.saveToFile();
    } catch (const exception& e) {
        cout << "Error: " << e.what() << endl;
    }

    loadFromFile();
    return 0;
}
```

**Run:**
```bash
g++ -o q5 q5.cpp
./q5
# students.txt will be created in the same directory
```

---

## Q7 – Employee Class with DOB Validation + File I/O

**Concept:** Same as Q5 but with mobile number instead of roll number.

```cpp
// File: q7.cpp

#include <iostream>
#include <fstream>
using namespace std;

class Employee {
public:
    string name;
    long long mobile;
    int day, month, year;

    void input() {
        cout << "Name: ";
        cin >> name;
        cout << "Mobile Number: ";
        cin >> mobile;
        cout << "Date of Birth (day month year): ";
        cin >> day >> month >> year;

        if (day < 1 || day > 31)
            throw runtime_error("Invalid day! Must be between 1 and 31.");
        if (month < 1 || month > 12)
            throw runtime_error("Invalid month! Must be between 1 and 12.");
    }

    void saveToFile() {
        ofstream f("employees.txt", ios::app);
        f << name << " " << mobile << " "
          << day << "/" << month << "/" << year << "\n";
        cout << "Record saved successfully.\n";
    }

    void display() {
        cout << "Name  : " << name
             << " | Mobile: " << mobile
             << " | DOB: " << day << "/" << month << "/" << year
             << endl;
    }
};

void loadFromFile() {
    ifstream f("employees.txt");
    string line;
    cout << "\n--- Employee Records from File ---\n";
    while (getline(f, line)) {
        cout << line << endl;
    }
}

int main() {
    Employee e;
    try {
        e.input();
        e.display();
        e.saveToFile();
    } catch (const exception& ex) {
        cout << "Error: " << ex.what() << endl;
    }

    loadFromFile();
    return 0;
}
```

**Run:**
```bash
g++ -o q7 q7.cpp
./q7
# employees.txt will be created in the same directory
```

---

## Q22 – BankAccount with Exception Handling

**Concept:** Throwing exceptions from constructor and member functions.

```cpp
// File: q22.cpp

#include <iostream>
using namespace std;

class BankAccount {
public:
    int accountNumber;
    string ownerName;
    double balance;

    BankAccount(int acc, string name, double deposit) {
        if (deposit < 500)
            throw runtime_error("Initial deposit must be at least Rs. 500!");
        accountNumber = acc;
        ownerName     = name;
        balance       = deposit;
        cout << "Account created for " << ownerName
             << " with balance Rs. " << balance << endl;
    }

    void withdraw(double amount) {
        if (amount > balance)
            throw runtime_error("Insufficient balance!");
        balance -= amount;
        cout << "Withdrawn: Rs. " << amount
             << " | Remaining Balance: Rs. " << balance << endl;
    }

    void displayBalance() {
        cout << "Balance for " << ownerName << ": Rs. " << balance << endl;
    }
};

int main() {
    // Test 1: Valid account with valid withdrawal
    try {
        BankAccount acc1(101, "Priya", 1000);
        acc1.withdraw(300);
        acc1.withdraw(900);  // will throw insufficient balance
    } catch (const exception& e) {
        cout << "Error: " << e.what() << endl;
    }

    cout << endl;

    // Test 2: Invalid initial deposit
    try {
        BankAccount acc2(102, "Rahul", 100);  // will throw
    } catch (const exception& e) {
        cout << "Error: " << e.what() << endl;
    }

    return 0;
}
```

**Run:**
```bash
g++ -o q22 q22.cpp
./q22
```

---

---

# FILE HANDLING (C++)

---

## Q20 – Product Binary File Operations

**Concept:** `fstream` binary read/write, in-place record update.

> **Note:** Uses `char name[30]` (not `string`) because `string` has variable size and can't be reliably binary-written as a fixed-size block.

```cpp
// File: q20.cpp

#include <iostream>
#include <fstream>
#include <cstring>
using namespace std;

class Product {
public:
    int  productID;
    char name[30];
    int  stockQuantity;
};

void addProduct(Product p) {
    ofstream f("products.bin", ios::binary | ios::app);
    f.write((char*)&p, sizeof(p));
    cout << "Product added: " << p.name << endl;
}

void searchAndUpdate(int id, int newQty) {
    fstream f("products.bin", ios::binary | ios::in | ios::out);
    Product p;

    while (f.read((char*)&p, sizeof(p))) {
        if (p.productID == id) {
            p.stockQuantity = newQty;
            f.seekp(-(int)sizeof(p), ios::cur);
            f.write((char*)&p, sizeof(p));
            cout << "Stock updated for Product ID " << id
                 << " to " << newQty << endl;
            return;
        }
    }
    cout << "Product ID " << id << " not found.\n";
}

void displayOutOfStock() {
    ifstream f("products.bin", ios::binary);
    Product p;
    cout << "\n--- Out of Stock Products ---\n";
    bool found = false;
    while (f.read((char*)&p, sizeof(p))) {
        if (p.stockQuantity == 0) {
            cout << "ID: " << p.productID
                 << " | Name: " << p.name << endl;
            found = true;
        }
    }
    if (!found) cout << "None.\n";
}

void displayAll() {
    ifstream f("products.bin", ios::binary);
    Product p;
    cout << "\n--- All Products ---\n";
    while (f.read((char*)&p, sizeof(p))) {
        cout << "ID: " << p.productID
             << " | Name: " << p.name
             << " | Stock: " << p.stockQuantity
             << endl;
    }
}

int main() {
    Product p1, p2, p3;

    p1.productID = 1; p1.stockQuantity = 0; strncpy(p1.name, "Pen",      29);
    p2.productID = 2; p2.stockQuantity = 5; strncpy(p2.name, "Notebook", 29);
    p3.productID = 3; p3.stockQuantity = 0; strncpy(p3.name, "Ruler",    29);

    addProduct(p1);
    addProduct(p2);
    addProduct(p3);

    displayAll();
    searchAndUpdate(2, 10);
    displayAll();
    displayOutOfStock();

    return 0;
}
```

**Run:**
```bash
g++ -o q20 q20.cpp
./q20
# products.bin will be created in the same directory
```

---

---

# INTERFACES (Java)

---

## Q12 – Sortable Interface: BubbleSort & SelectionSort

**Concept:** Interface implementation with two sorting algorithms.

```java
// File: Q12.java

interface Sortable {
    void sort(int[] arr);
}

class BubbleSort implements Sortable {
    @Override
    public void sort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j]   = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
    }
}

class SelectionSort implements Sortable {
    @Override
    public void sort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            int minIdx = i;
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIdx])
                    minIdx = j;
            }
            int temp    = arr[i];
            arr[i]      = arr[minIdx];
            arr[minIdx] = temp;
        }
    }
}

public class Q12 {
    static void printArray(int[] arr) {
        for (int x : arr)
            System.out.print(x + " ");
        System.out.println();
    }

    public static void main(String[] args) {
        int[] a = {5, 2, 8, 1, 9};
        int[] b = {5, 2, 8, 1, 9};

        new BubbleSort().sort(a);
        System.out.print("BubbleSort:    ");
        printArray(a);

        new SelectionSort().sort(b);
        System.out.print("SelectionSort: ");
        printArray(b);
    }
}
```

**Run:**
```bash
javac Q12.java
java Q12
```

---

## Q18 – Searchable Interface: Linear & Binary Search

**Concept:** Interface with two different search algorithm implementations.

```java
// File: Q18.java

interface Searchable {
    void search(int[] arr, int key);
}

class LinearSearch implements Searchable {
    @Override
    public void search(int[] arr, int key) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == key) {
                System.out.println("Linear Search: Found " + key + " at index " + i);
                return;
            }
        }
        System.out.println("Linear Search: " + key + " not found.");
    }
}

class BinarySearch implements Searchable {
    @Override
    public void search(int[] arr, int key) {
        int lo = 0, hi = arr.length - 1;

        while (lo <= hi) {
            int mid = (lo + hi) / 2;
            if (arr[mid] == key) {
                System.out.println("Binary Search: Found " + key + " at index " + mid);
                return;
            } else if (arr[mid] < key) {
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }
        System.out.println("Binary Search: " + key + " not found.");
    }
}

public class Q18 {
    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 7, 9};

        new LinearSearch().search(arr, 5);
        new LinearSearch().search(arr, 4);

        new BinarySearch().search(arr, 7);
        new BinarySearch().search(arr, 6);
    }
}
```

**Run:**
```bash
javac Q18.java
java Q18
```

---

## Q25 – Payment Interface with Abstract Transaction

**Concept:** Interface + abstract class + composition.

```java
// File: Q25.java

interface Payment {
    void processPayment(double amount);
}

class CreditCardPayment implements Payment {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing Credit Card payment of Rs. " + amount);
    }
}

class UPIPayment implements Payment {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing UPI payment of Rs. " + amount);
    }
}

abstract class Transaction {
    String  transactionID;
    Payment payment;

    Transaction(String id, Payment p) {
        transactionID = id;
        payment       = p;
    }

    void execute(double amount) {
        System.out.print("Transaction ID: " + transactionID + " -> ");
        payment.processPayment(amount);
    }
}

class OnlineTransaction extends Transaction {
    OnlineTransaction(String id, Payment p) {
        super(id, p);
    }
}

public class Q25 {
    public static void main(String[] args) {
        new OnlineTransaction("TXN001", new CreditCardPayment()).execute(1500.0);
        new OnlineTransaction("TXN002", new UPIPayment()).execute(350.0);
    }
}
```

**Run:**
```bash
javac Q25.java
java Q25
```

---

---

# TEMPLATES (C++)

---

## Q6 – Generic Vector Class Template

**Concept:** Class templates with type parameter `T`.

```cpp
// File: q6.cpp

#include <iostream>
using namespace std;

template <class T>
class Vector {
    T   arr[100];
    int size;

public:
    Vector(int s) : size(s) {}

    void create() {
        cout << "Enter " << size << " elements:\n";
        for (int i = 0; i < size; i++) {
            cout << "Element [" << i << "]: ";
            cin >> arr[i];
        }
    }

    void modify(int index, T value) {
        if (index >= 0 && index < size)
            arr[index] = value;
        else
            cout << "Index out of range!\n";
    }

    void multiply(T scalar) {
        for (int i = 0; i < size; i++)
            arr[i] *= scalar;
    }

    void display() {
        cout << "Vector: [ ";
        for (int i = 0; i < size; i++)
            cout << arr[i] << " ";
        cout << "]\n";
    }
};

int main() {
    Vector<int> v(4);
    v.create();
    cout << "\nAfter creation: ";
    v.display();

    v.modify(0, 99);
    cout << "After modify index 0 to 99: ";
    v.display();

    v.multiply(2);
    cout << "After multiplying by 2: ";
    v.display();

    return 0;
}
```

**Run:**
```bash
g++ -o q6 q6.cpp
./q6
```

---

## Q15 – Generic ScoreList Class Template

**Concept:** Same structure as Q6 but for score lists (demonstrates with `float`).

```cpp
// File: q15.cpp

#include <iostream>
using namespace std;

template <class T>
class ScoreList {
    T   arr[100];
    int size;

public:
    ScoreList(int s) : size(s) {}

    void create() {
        cout << "Enter " << size << " scores:\n";
        for (int i = 0; i < size; i++) {
            cout << "Score [" << i + 1 << "]: ";
            cin >> arr[i];
        }
    }

    void modify(int index, T value) {
        if (index >= 0 && index < size)
            arr[index] = value;
        else
            cout << "Index out of range!\n";
    }

    void multiply(T scalar) {
        for (int i = 0; i < size; i++)
            arr[i] *= scalar;
    }

    void display() {
        cout << "Scores: [ ";
        for (int i = 0; i < size; i++)
            cout << arr[i] << " ";
        cout << "]\n";
    }
};

int main() {
    ScoreList<float> s(3);
    s.create();
    cout << "\nOriginal: ";
    s.display();

    s.modify(1, 95.5f);
    cout << "After modifying index 1 to 95.5: ";
    s.display();

    s.multiply(1.1f);
    cout << "After multiplying by 1.1 (bonus): ";
    s.display();

    return 0;
}
```

**Run:**
```bash
g++ -o q15 q15.cpp
./q15
```

---

## Q23 – Generic Stack Class Template

**Concept:** Stack data structure with templates — works for `int` and `string`.

```cpp
// File: q23.cpp

#include <iostream>
using namespace std;

template <class T>
class Stack {
    T   arr[100];
    int top;

public:
    Stack() : top(-1) {}

    void push(T val) {
        if (top >= 99) {
            cout << "Stack Overflow!\n";
            return;
        }
        arr[++top] = val;
        cout << val << " pushed.\n";
    }

    void pop() {
        if (top < 0) {
            cout << "Stack is empty!\n";
            return;
        }
        cout << arr[top--] << " popped.\n";
    }

    bool isEmpty() {
        return top == -1;
    }

    void display() {
        if (isEmpty()) {
            cout << "Stack is empty.\n";
            return;
        }
        cout << "Stack (top -> bottom): ";
        for (int i = top; i >= 0; i--)
            cout << arr[i] << " ";
        cout << endl;
    }
};

int main() {
    cout << "--- Integer Stack ---\n";
    Stack<int> si;
    si.push(10);
    si.push(20);
    si.push(30);
    si.display();
    si.pop();
    si.display();

    cout << "\n--- String Stack ---\n";
    Stack<string> ss;
    ss.push("hello");
    ss.push("world");
    ss.push("OOP");
    ss.display();
    ss.pop();
    ss.display();

    return 0;
}
```

**Run:**
```bash
g++ -o q23 q23.cpp
./q23
```

---

---

# RUNTIME POLYMORPHISM / VIRTUAL FUNCTIONS (C++)

---

## Q19 – Employee Salary with Virtual Functions

**Concept:** Pure virtual function, base class pointer, runtime polymorphism.

```cpp
// File: q19.cpp

#include <iostream>
using namespace std;

class Employee {
public:
    string name;

    virtual float calculateSalary() = 0;  // pure virtual

    void display() {
        cout << "Employee: " << name
             << " | Salary: Rs. " << calculateSalary()
             << endl;
    }

    virtual ~Employee() {}
};

class FullTimeEmployee : public Employee {
    float monthlySalary;

public:
    FullTimeEmployee(string n, float s) {
        name          = n;
        monthlySalary = s;
    }

    float calculateSalary() override {
        return monthlySalary;
    }
};

class PartTimeEmployee : public Employee {
    float hours;
    float rate;

public:
    PartTimeEmployee(string n, float h, float r) {
        name  = n;
        hours = h;
        rate  = r;
    }

    float calculateSalary() override {
        return hours * rate;
    }
};

int main() {
    Employee* e1 = new FullTimeEmployee("Anjali", 55000);
    Employee* e2 = new PartTimeEmployee("Ravi", 80, 250);

    e1->display();
    e2->display();

    delete e1;
    delete e2;

    return 0;
}
```

**Run:**
```bash
g++ -o q19 q19.cpp
./q19
```

---

---

## Quick Reference – All Run Commands

```bash
# Java Questions
javac Q1.java  && java Q1
javac Q9.java  && java Q9
javac Q10.java && java Q10
javac Q12.java && java Q12
javac Q13.java && java Q13
javac Q18.java && java Q18
javac Q25.java && java Q25
javac Q26.java && java Q26

# Java Package Questions (run from parent folder)
mkdir converter && javac converter/UnitConverter.java && javac Q3.java  && java Q3
mkdir calc      && javac calc/Calculator.java         && javac Q14.java && java Q14
mkdir hospital  && javac hospital/Patient.java        && javac Q24.java && java Q24

# C++ Questions
g++ -o q2  q2.cpp  && ./q2
g++ -o q4  q4.cpp  && ./q4
g++ -o q5  q5.cpp  && ./q5
g++ -o q6  q6.cpp  && ./q6
g++ -o q7  q7.cpp  && ./q7
g++ -o q8  q8.cpp  && ./q8
g++ -o q15 q15.cpp && ./q15
g++ -o q19 q19.cpp && ./q19
g++ -o q20 q20.cpp && ./q20
g++ -o q21 q21.cpp && ./q21
g++ -o q22 q22.cpp && ./q22
g++ -o q23 q23.cpp && ./q23
```

---

*Generated for Semester 4 – OOP Lab (Java & C++)*
