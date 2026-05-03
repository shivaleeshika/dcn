# oops
# OOP Practice Problems – Solutions

---

## 🧵 MULTITHREADING (Java)

### Q1 – Two Threads: Uppercase & Lowercase Letters

```java
class UpperCase extends Thread {
    public void run() {
        for (char c = 'A'; c <= 'Z'; c++)
            System.out.print(c + " ");
    }
}

class LowerCase extends Thread {
    public void run() {
        for (char c = 'a'; c <= 'z'; c++)
            System.out.print(c + " ");
    }
}

public class Q1 {
    public static void main(String[] args) {
        new UpperCase().start();
        new LowerCase().start();
    }
}
```

---

### Q10 – Two Threads: Even & Odd Numbers (1–20)

```java
class EvenThread extends Thread {
    public void run() {
        for (int i = 2; i <= 20; i += 2)
            System.out.println("Even: " + i);
    }
}

class OddThread extends Thread {
    public void run() {
        for (int i = 1; i <= 20; i += 2)
            System.out.println("Odd: " + i);
    }
}

public class Q10 {
    public static void main(String[] args) {
        new EvenThread().start();
        new OddThread().start();
    }
}
```

---

### Q26 – Two Threads: Primes (1–50) & Fibonacci (10 terms)

```java
class PrimeThread extends Thread {
    public void run() {
        for (int n = 2; n <= 50; n++) {
            boolean prime = true;
            for (int i = 2; i <= n / 2; i++)
                if (n % i == 0) { prime = false; break; }
            if (prime) System.out.println("Prime: " + n);
            try { Thread.sleep(500); } catch (Exception e) {}
        }
    }
}

class FibThread extends Thread {
    public void run() {
        int a = 0, b = 1;
        for (int i = 0; i < 10; i++) {
            System.out.println("Fib: " + a);
            int temp = a + b; a = b; b = temp;
            try { Thread.sleep(500); } catch (Exception e) {}
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

---

## 🔗 INHERITANCE

### Q2 – Employee → Manager & Engineer (C++)

```cpp
#include <iostream>
using namespace std;

class Employee {
public:
    string name; int id; float salary;
    void input() { cout << "Name: "; cin >> name; cout << "ID: "; cin >> id; cout << "Salary: "; cin >> salary; }
    void display() { cout << "Name: " << name << ", ID: " << id << ", Salary: " << salary << endl; }
};

class Manager : public Employee {
public:
    string dept; int subordinates;
    void input() { Employee::input(); cout << "Dept: "; cin >> dept; cout << "Subordinates: "; cin >> subordinates; }
    void display() { Employee::display(); cout << "Dept: " << dept << ", Subordinates: " << subordinates << endl; }
};

class Engineer : public Employee {
public:
    string project, language;
    void input() { Employee::input(); cout << "Project: "; cin >> project; cout << "Language: "; cin >> language; }
    void display() { Employee::display(); cout << "Project: " << project << ", Language: " << language << endl; }
};

int main() {
    Manager m; m.input(); m.display();
    Engineer e; e.input(); e.display();
}
```

---

### Q8 / Q16 – Book → Textbook & Novel (C++)

```cpp
#include <iostream>
using namespace std;

class Book {
public:
    string title, isbn; float price;
    void input() { cout << "Title: "; cin >> title; cout << "ISBN: "; cin >> isbn; cout << "Price: "; cin >> price; }
    void display() { cout << "Title: " << title << ", ISBN: " << isbn << ", Price: " << price << endl; }
};

class Textbook : public Book {
public:
    string subject, level;
    void input() { Book::input(); cout << "Subject: "; cin >> subject; cout << "Level: "; cin >> level; }
    void display() { Book::display(); cout << "Subject: " << subject << ", Level: " << level << endl; }
};

class Novel : public Book {
public:
    string genre, author;
    void input() { Book::input(); cout << "Genre: "; cin >> genre; cout << "Author: "; cin >> author; }
    void display() { Book::display(); cout << "Genre: " << genre << ", Author: " << author << endl; }
};

int main() {
    Textbook t; t.input(); t.display();
    Novel n; n.input(); n.display();
}
```

---

### Q9 – Shape → Rectangle & Triangle (Java)

```java
class Shape {
    double dim1, dim2;
    Shape(double a, double b) { dim1 = a; dim2 = b; }
    double area() { return 0; }
}

class Rectangle extends Shape {
    Rectangle(double a, double b) { super(a, b); }
    double area() { return dim1 * dim2; }
}

class Triangle extends Shape {
    Triangle(double a, double b) { super(a, b); }
    double area() { return 0.5 * dim1 * dim2; }
}

public class Q9 {
    public static void main(String[] args) {
        Shape r = new Rectangle(5, 3);
        Shape t = new Triangle(5, 3);
        System.out.println("Rectangle Area: " + r.area());
        System.out.println("Triangle Area: " + t.area());
    }
}
```

---

### Q13 – Vehicle → Car & Bike (Java)

```java
class Vehicle {
    double speed, distance;
    Vehicle(double s, double d) { speed = s; distance = d; }
    double travelTime() { return distance / speed; }
}

class Car extends Vehicle {
    Car(double s, double d) { super(s, d); }
    double travelTime() { return super.travelTime() + 0.5; } // traffic delay
}

class Bike extends Vehicle {
    Bike(double s, double d) { super(s, d); }
    double travelTime() { return super.travelTime() + 1.0; } // fuel stop
}

public class Q13 {
    public static void main(String[] args) {
        Vehicle car = new Car(60, 120);
        Vehicle bike = new Bike(40, 120);
        System.out.println("Car travel time: " + car.travelTime() + " hrs");
        System.out.println("Bike travel time: " + bike.travelTime() + " hrs");
    }
}
```

---

## 📦 PACKAGES & ACCESS MODIFIERS (Java)

### Q3 – Unit Converter Package

```java
// File: converter/UnitConverter.java
package converter;

public class UnitConverter {
    public double kmToMiles(double km)          { return km * 0.621371; }
    public double celsiusToFahrenheit(double c) { return c * 9/5 + 32; }
}

// File: Q3.java (outside package)
import converter.UnitConverter;
import java.util.Scanner;

public class Q3 {
    public static void main(String[] args) {
        UnitConverter uc = new UnitConverter();
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter km: ");
        System.out.println(sc.nextDouble() + " km = " + uc.kmToMiles(10) + " miles");
        System.out.print("Enter Celsius: ");
        System.out.println(sc.nextDouble() + " C = " + uc.celsiusToFahrenheit(100) + " F");
    }
}
```

---

### Q14 – Calculator Package

```java
// File: calc/Calculator.java
package calc;

public class Calculator {
    public double add(double a, double b)      { return a + b; }
    public double subtract(double a, double b) { return a - b; }
    public double multiply(double a, double b) { return a * b; }
    public double divide(double a, double b)   { return b != 0 ? a / b : 0; }
}

// File: Q14.java
import calc.Calculator;

public class Q14 {
    public static void main(String[] args) {
        Calculator c = new Calculator();
        System.out.println("Add: "      + c.add(10, 5));
        System.out.println("Subtract: " + c.subtract(10, 5));
        System.out.println("Multiply: " + c.multiply(10, 5));
        System.out.println("Divide: "   + c.divide(10, 5));
    }
}
```

---

### Q24 – Hospital Package with Access Modifiers

```java
// File: hospital/Patient.java
package hospital;

public class Patient {
    protected String name;
    protected int age;
    protected String ailment;

    public Patient(String n, int a, String ail) { name = n; age = a; ailment = ail; }
    public void display() { System.out.println("Name: " + name + ", Age: " + age + ", Ailment: " + ailment); }
}

// File: Q24.java
import hospital.Patient;

class Doctor extends Patient {
    String specialization;
    Doctor(String n, int a, String ail, String s) { super(n, a, ail); specialization = s; }
    void display() { super.display(); System.out.println("Specialization: " + specialization); }
}

public class Q24 {
    public static void main(String[] args) {
        Doctor d = new Doctor("Dr. Smith", 45, "Fever", "General");
        d.display();
    }
}
```

---

## ➕ OPERATOR OVERLOADING (C++)

### Q4 / Q17 – Complex Number with operator+ (Menu-Driven)

```cpp
#include <iostream>
using namespace std;

class Complex {
public:
    float real, imag;
    Complex(float r = 0, float i = 0) : real(r), imag(i) {}
    Complex operator+(Complex c) { return Complex(real + c.real, imag + c.imag); }
    void display() { cout << real << " + " << imag << "i" << endl; }
};

int main() {
    Complex c1, c2, c3;
    int choice;
    do {
        cout << "1.Enter  2.Add  3.Display  0.Exit\nChoice: ";
        cin >> choice;
        if (choice == 1) {
            cout << "C1 real imag: "; cin >> c1.real >> c1.imag;
            cout << "C2 real imag: "; cin >> c2.real >> c2.imag;
        } else if (choice == 2) { c3 = c1 + c2; cout << "Sum: "; c3.display(); }
        else if (choice == 3) { c1.display(); c2.display(); }
    } while (choice != 0);
}
```

---

### Q11 – Complex Number (Same as Q4/Q17 — identical logic)

*Refer to Q4/Q17 above — the logic and structure are identical.*

---

### Q21 – Matrix with +, -, * Overloading

```cpp
#include <iostream>
using namespace std;

class Matrix {
    int m[2][2];
public:
    Matrix() { for(int i=0;i<2;i++) for(int j=0;j<2;j++) m[i][j]=0; }
    void input() { cout<<"Enter 4 elements: "; for(int i=0;i<2;i++) for(int j=0;j<2;j++) cin>>m[i][j]; }
    void display() { for(int i=0;i<2;i++){ for(int j=0;j<2;j++) cout<<m[i][j]<<" "; cout<<endl; } }

    Matrix operator+(Matrix b) { Matrix r; for(int i=0;i<2;i++) for(int j=0;j<2;j++) r.m[i][j]=m[i][j]+b.m[i][j]; return r; }
    Matrix operator-(Matrix b) { Matrix r; for(int i=0;i<2;i++) for(int j=0;j<2;j++) r.m[i][j]=m[i][j]-b.m[i][j]; return r; }
    Matrix operator*(Matrix b) {
        Matrix r;
        for(int i=0;i<2;i++) for(int j=0;j<2;j++) for(int k=0;k<2;k++) r.m[i][j]+=m[i][k]*b.m[k][j];
        return r;
    }
};

int main() {
    Matrix a, b;
    a.input(); b.input();
    cout << "Add:\n";    (a+b).display();
    cout << "Subtract:\n"; (a-b).display();
    cout << "Multiply:\n"; (a*b).display();
}
```

---

## 🔌 INTERFACES (Java)

### Q12 – Sortable Interface: BubbleSort & SelectionSort

```java
interface Sortable {
    void sort(int[] arr);
}

class BubbleSort implements Sortable {
    public void sort(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++)
            for (int j = 0; j < arr.length - 1 - i; j++)
                if (arr[j] > arr[j+1]) { int t = arr[j]; arr[j] = arr[j+1]; arr[j+1] = t; }
    }
}

class SelectionSort implements Sortable {
    public void sort(int[] arr) {
        for (int i = 0; i < arr.length - 1; i++) {
            int min = i;
            for (int j = i+1; j < arr.length; j++) if (arr[j] < arr[min]) min = j;
            int t = arr[i]; arr[i] = arr[min]; arr[min] = t;
        }
    }
}

public class Q12 {
    public static void main(String[] args) {
        int[] a = {5, 2, 8, 1, 9};
        new BubbleSort().sort(a);
        for (int x : a) System.out.print(x + " ");
    }
}
```

---

### Q18 – Searchable Interface: Linear & Binary Search

```java
interface Searchable {
    void search(int[] arr, int key);
}

class LinearSearch implements Searchable {
    public void search(int[] arr, int key) {
        for (int i = 0; i < arr.length; i++)
            if (arr[i] == key) { System.out.println("Found at index " + i); return; }
        System.out.println("Not found");
    }
}

class BinarySearch implements Searchable {
    public void search(int[] arr, int key) {
        int lo = 0, hi = arr.length - 1;
        while (lo <= hi) {
            int mid = (lo + hi) / 2;
            if (arr[mid] == key) { System.out.println("Found at index " + mid); return; }
            else if (arr[mid] < key) lo = mid + 1;
            else hi = mid - 1;
        }
        System.out.println("Not found");
    }
}

public class Q18 {
    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 7, 9};
        new LinearSearch().search(arr, 5);
        new BinarySearch().search(arr, 7);
    }
}
```

---

### Q25 – Payment Interface with Abstract Transaction

```java
interface Payment {
    void processPayment(double amount);
}

class CreditCardPayment implements Payment {
    public void processPayment(double amount) { System.out.println("Credit Card payment: Rs " + amount); }
}

class UPIPayment implements Payment {
    public void processPayment(double amount) { System.out.println("UPI payment: Rs " + amount); }
}

abstract class Transaction {
    String transactionID;
    Payment payment;
    Transaction(String id, Payment p) { transactionID = id; payment = p; }
    void execute(double amount) { System.out.print("TxnID: " + transactionID + " -> "); payment.processPayment(amount); }
}

class OnlineTransaction extends Transaction {
    OnlineTransaction(String id, Payment p) { super(id, p); }
}

public class Q25 {
    public static void main(String[] args) {
        new OnlineTransaction("T001", new CreditCardPayment()).execute(500);
        new OnlineTransaction("T002", new UPIPayment()).execute(200);
    }
}
```

---

## ⚠️ EXCEPTION HANDLING

### Q5 – Student Class with DOB Validation + File I/O (C++)

```cpp
#include <iostream>
#include <fstream>
using namespace std;

class Student {
public:
    string name; int roll, day, month, year;

    void input() {
        cout << "Name: "; cin >> name;
        cout << "Roll: "; cin >> roll;
        cout << "DOB (day month year): "; cin >> day >> month >> year;
        if (day < 1 || day > 31)   throw runtime_error("Invalid day!");
        if (month < 1 || month > 12) throw runtime_error("Invalid month!");
    }

    void saveToFile() {
        ofstream f("students.txt", ios::app);
        f << name << " " << roll << " " << day << "/" << month << "/" << year << "\n";
    }

    void display() { cout << name << " | Roll: " << roll << " | DOB: " << day << "/" << month << "/" << year << endl; }
};

void loadFromFile() {
    ifstream f("students.txt");
    string line;
    cout << "\n--- Records ---\n";
    while (getline(f, line)) cout << line << endl;
}

int main() {
    Student s;
    try { s.input(); s.saveToFile(); s.display(); }
    catch (exception& e) { cout << "Error: " << e.what() << endl; }
    loadFromFile();
}
```

---

### Q7 – Employee Class with DOB Validation + File I/O (C++)

```cpp
#include <iostream>
#include <fstream>
using namespace std;

class Employee {
public:
    string name; long long mobile; int day, month, year;

    void input() {
        cout << "Name: "; cin >> name;
        cout << "Mobile: "; cin >> mobile;
        cout << "DOB (day month year): "; cin >> day >> month >> year;
        if (day < 1 || day > 31)   throw runtime_error("Invalid day!");
        if (month < 1 || month > 12) throw runtime_error("Invalid month!");
    }

    void saveToFile() {
        ofstream f("employees.txt", ios::app);
        f << name << " " << mobile << " " << day << "/" << month << "/" << year << "\n";
    }

    void display() { cout << name << " | Mobile: " << mobile << " | DOB: " << day << "/" << month << "/" << year << endl; }
};

void loadFromFile() {
    ifstream f("employees.txt");
    string line;
    cout << "\n--- Records ---\n";
    while (getline(f, line)) cout << line << endl;
}

int main() {
    Employee e;
    try { e.input(); e.saveToFile(); e.display(); }
    catch (exception& e) { cout << "Error: " << e.what() << endl; }
    loadFromFile();
}
```

---

### Q22 – BankAccount with Exception Handling (C++)

```cpp
#include <iostream>
using namespace std;

class BankAccount {
public:
    int accountNumber; string ownerName; double balance;

    BankAccount(int acc, string name, double deposit) {
        if (deposit < 500) throw runtime_error("Initial deposit must be >= 500!");
        accountNumber = acc; ownerName = name; balance = deposit;
    }

    void withdraw(double amount) {
        if (amount > balance) throw runtime_error("Insufficient balance!");
        balance -= amount;
        cout << "Withdrawn: " << amount << " | Balance: " << balance << endl;
    }
};

int main() {
    try {
        BankAccount acc(101, "Alice", 1000);
        acc.withdraw(200);
        acc.withdraw(900); // will throw
    } catch (exception& e) { cout << "Error: " << e.what() << endl; }

    try {
        BankAccount acc2(102, "Bob", 100); // will throw
    } catch (exception& e) { cout << "Error: " << e.what() << endl; }
}
```

---

## 📄 FILE HANDLING (C++)

### Q20 – Product Binary File Operations

```cpp
#include <iostream>
#include <fstream>
using namespace std;

class Product {
public:
    int productID, stockQuantity;
    char name[30];
};

void addProduct(Product p) {
    ofstream f("products.bin", ios::binary | ios::app);
    f.write((char*)&p, sizeof(p));
}

void searchAndUpdate(int id, int qty) {
    fstream f("products.bin", ios::binary | ios::in | ios::out);
    Product p;
    while (f.read((char*)&p, sizeof(p))) {
        if (p.productID == id) {
            p.stockQuantity = qty;
            f.seekp(-(int)sizeof(p), ios::cur);
            f.write((char*)&p, sizeof(p));
            cout << "Updated!" << endl; return;
        }
    }
    cout << "Not found" << endl;
}

void displayOutOfStock() {
    ifstream f("products.bin", ios::binary);
    Product p;
    cout << "\nOut of Stock:\n";
    while (f.read((char*)&p, sizeof(p)))
        if (p.stockQuantity == 0) cout << p.productID << " - " << p.name << endl;
}

int main() {
    Product p1 = {1, 0, "Pen"}, p2 = {2, 5, "Book"}, p3 = {3, 0, "Ruler"};
    addProduct(p1); addProduct(p2); addProduct(p3);
    searchAndUpdate(2, 10);
    displayOutOfStock();
}
```

---

## 🧩 TEMPLATES (C++)

### Q6 – Generic Vector Class Template

```cpp
#include <iostream>
using namespace std;

template <class T>
class Vector {
    T* arr; int size;
public:
    Vector(int s) : size(s) { arr = new T[s]; }

    void create()         { for(int i=0;i<size;i++){ cout<<"Element "<<i+1<<": "; cin>>arr[i]; } }
    void modify(int i, T v) { arr[i] = v; }
    void multiply(T scalar) { for(int i=0;i<size;i++) arr[i] *= scalar; }
    void display()        { for(int i=0;i<size;i++) cout<<arr[i]<<" "; cout<<endl; }
};

int main() {
    Vector<int> v(3);
    v.create();
    v.modify(0, 10);
    v.multiply(2);
    v.display();
}
```

---

### Q15 – Generic ScoreList Class Template

```cpp
#include <iostream>
using namespace std;

template <class T>
class ScoreList {
    T* arr; int size;
public:
    ScoreList(int s) : size(s) { arr = new T[s]; }

    void create()             { for(int i=0;i<size;i++){ cout<<"Score "<<i+1<<": "; cin>>arr[i]; } }
    void modify(int i, T val) { arr[i] = val; }
    void multiply(T scalar)   { for(int i=0;i<size;i++) arr[i] *= scalar; }
    void display()            { for(int i=0;i<size;i++) cout<<arr[i]<<" "; cout<<endl; }
};

int main() {
    ScoreList<float> s(3);
    s.create();
    s.modify(1, 95.5);
    s.multiply(1.1f);
    s.display();
}
```

---

### Q23 – Generic Stack Class Template

```cpp
#include <iostream>
using namespace std;

template <class T>
class Stack {
    T arr[100]; int top = -1;
public:
    void push(T val)  { arr[++top] = val; }
    void pop()        { if(top >= 0) cout<<"Popped: "<<arr[top--]<<endl; else cout<<"Empty!"<<endl; }
    bool isEmpty()    { return top == -1; }
    void display()    { for(int i=top;i>=0;i--) cout<<arr[i]<<" "; cout<<endl; }
};

int main() {
    Stack<int> si;
    si.push(10); si.push(20); si.push(30);
    si.display(); si.pop(); si.display();

    Stack<string> ss;
    ss.push("hello"); ss.push("world");
    ss.display(); ss.pop(); ss.display();
}
```

---

## 🔄 RUNTIME POLYMORPHISM / VIRTUAL FUNCTIONS (C++)

### Q19 – Employee with Virtual calculateSalary()

```cpp
#include <iostream>
using namespace std;

class Employee {
public:
    string name;
    virtual float calculateSalary() = 0;
    void display() { cout << name << " -> Salary: " << calculateSalary() << endl; }
};

class FullTimeEmployee : public Employee {
    float monthlySalary;
public:
    FullTimeEmployee(string n, float s) { name = n; monthlySalary = s; }
    float calculateSalary() { return monthlySalary; }
};

class PartTimeEmployee : public Employee {
    float hours, rate;
public:
    PartTimeEmployee(string n, float h, float r) { name = n; hours = h; rate = r; }
    float calculateSalary() { return hours * rate; }
};

int main() {
    Employee* e1 = new FullTimeEmployee("Alice", 50000);
    Employee* e2 = new PartTimeEmployee("Bob", 80, 200);
    e1->display();
    e2->display();
}
```
