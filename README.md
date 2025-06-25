# 도서관 시스템
이 프로젝트는 간단한 도서관 관리 시스템을 구현한 프로그램입니다.  
`Manager`, `Customer`, `Book` 클래스를 중심으로 책 대여/반납 및 사용자 관리 기능을 제공합니다.

## 클래스 구조

### 1. `Book` 클래스
- **속성**
  - `name`: 책 이름
  - `bookID`: 책 ID
- **메서드**
  - `get_name()`, `set_name(newName)`
  - `get_bookID()`, `set_bookID(newbookID)`

### 2. `User` 클래스 (부모 클래스)
- **속성**
  - `name`: 사용자 이름
  - `userID`: 사용자 ID
- **메서드**
  - `get_name()`, `set_name(newName)`
  - `get_userID()`, `set_userID(newuserID)`

### 3. `Customer` 클래스 (User 상속)
- **속성**
  - `booklist`: 대여한 책 리스트
- **메서드**
  - `get_booklist()`, `set_booklist(newbooklist)`
  - `borrowBook(Book)`: 책 대여
  - `returnBook(Book)`: 책 반납
  - `show_booklist()`: 대여한 책 목록 출력

### 4. `Manager` 클래스 (User 상속)
- **속성**
  - `managerID`: 관리자 고유 ID
- **메서드**
  - `add_customer(name, userID)`
  - `add_manager(name, userID, managerID)`
  - `add_book(name, bookID)`
  - 위의 사용자(고객과 매니저) 및 책 추가 기능 제공

---

## 주요 기능

### 관리자 기능
- 고객 대출 도서 목록 확인
- 고객 추가
- 관리자 추가
- 책 추가

### 고객 기능
- 본인이 빌린 책 목록 확인
- 책 대여
- 책 반납

---

## 프로그램 실행 흐름

1. 사용자에게 ID 입력 요청
2. ID에 따라 `Manager` 또는 `Customer`로 로그인
3. 사용자 유형에 따라 다른 메뉴 제공
4. 각 메뉴에서 입력에 따라 기능 실행

---

## 코드드

```
class Book():
    def __init__(self, name, bookID):
        self.name=name
        self.bookID=bookID
    def get_name(self):
        return self.name
    def set_name(self, newName):
        self.name=newName
    def get_bookID(self):
        return self.bookID
    def set_bookID(self, newbookID):
        self.bookID=newbookID

class User():
    def __init__(self, name, userID):
        self.name=name
        self.userID=userID
    def get_name(self):
        return self.name
    def set_name(self, newName):
        self.name=newName
    def get_userID(self):
        return self.userID
    def set_userID(self, newuserID):
        self.userID=newuserID

class Manager(User):
    def __init__(self, name, userID, managerID):
        super().__init__(name, userID)
        self.managerID=managerID
    def get_managerID(self):
        return self.managerID
    def set_managerID(self, newmanagerID):
        self.managerID=newmanagerID
    def add_customer(self, name, userID):
        new_customer = Customer(name, userID)
        customer_list[userID] = new_customer
        print(f"Customer '{name}' with ID '{userID}' added successfully.")
    def add_manager(self, name, userID, managerID):
        new_manager = Manager(name, userID, managerID)
        manager_list[userID] = new_manager
        print(f"Manager '{name}' with ID '{userID}' added successfully.")
    def add_book(self, name, bookID):
        new_book = Book(name, bookID)
        book_list[bookID] = new_book
        print(f"Book '{name}' with ID '{bookID}' added successfully.")

class Customer(User):
    def __init__(self, name, userID):
        super().__init__(name, userID)
        self.booklist=[]
    def get_booklist(self):
        return self.booklist
    def set_booklist(self, newbooklist):
        self.userID=newbooklist
    def borrowBook(self,Book):
        self.booklist.append(Book)
    def returnBook(self, Book):
        self.booklist.remove(Book)
    def show_booklist(self):
        for book in self.booklist:
            print(f"{book.get_name()} ({book.get_bookID()})")

book_list = {}
customer_list = {}
manager_list = {}

manager1 = Manager("Manager", "manager1", "001")
manager_list[manager1.get_userID()] = manager1

customer1 = Customer("Customer", "customer1")
customer_list[customer1.get_userID()] = customer1

book1 = Book("Cinderella", "0001")
book2 = Book("Snow White", "0002")
book_list[book1.get_bookID()] = book1
book_list[book2.get_bookID()] = book2


while (1):
    ID=input("Enter your ID : ")

    if ID in manager_list:
        current_manager = manager_list[ID]
        print(f"Welcome, {current_manager.get_name()}.")
        while(1):
            managerAction=input("Show Customer Booklist(1) / Add Customer(2) / Add Manager(3) / Add Book(4) / Logout(5) : ")
            if managerAction == "1":
                print("Customer ID list :", ', '.join(customer_list.keys()))
                selectCustomerID = input("Enter Customer ID: ")
                if selectCustomerID in customer_list:
                    selected_customer = customer_list[selectCustomerID]
                    print(f"Name: {selected_customer.get_name()}\nBorrowed Book List:")
                    selected_customer.show_booklist()
                else:
                    print("You have entered the wrong customer ID. Please enter again correctly.")
            elif managerAction=="2":
                name = input("Enter new customer name: ")
                userID = input("Enter new customer ID: ")
                if userID in customer_list:
                    print(f"User ID '{userID}' already exists.")
                else:
                    current_manager.add_customer(name, userID)
            elif managerAction=="3":
                name = input("Enter new manager name: ")
                userID = input("Enter new manager ID: ")
                managerID = input("Enter manager unique ID: ")
                if userID in manager_list:
                    print(f"Manager ID '{userID}' already exists.")
                else:
                    current_manager.add_manager(name, userID, managerID)
            elif managerAction == "4":
                name = input("Enter new book name: ")
                bookID = input("Enter new book ID: ")
                if bookID in book_list:
                    print("Book ID already exists.")
                else:
                    current_manager.add_book(name, bookID)
            elif managerAction=="5":
                break
            else:
                print("Invalid input. Please try again.")

    elif ID in customer_list:
        current_customer = customer_list[ID]
        print(f"Welcome, {current_customer.get_name()}.")
        while (1):
            customerAction=input("Show Booklist(1) / Borrow Book(2) / Return Book(3) / Logout(4): ")
            if customerAction=="1":
                current_customer.show_booklist()
            elif customerAction=="2":
                print("[Available Books:]")
                for book in book_list.values():
                    print(f"- {book.get_name()} ({book.get_bookID()})")
                book_name = input("Enter book name: ")
                found = False
                for book in book_list.values():
                    if book.get_name() == book_name:
                        current_customer.borrowBook(book)
                        print("Book successfully borrowed.")
                        found = True
                        break
                if not found:
                    print("You have entered the wrong book name. Please enter again correctly.")
            elif customerAction == "3":
                current_customer.show_booklist()
                book_name = input("Enter book name: ")
                for book in current_customer.get_booklist():
                    if book.get_name() == book_name:
                        current_customer.returnBook(book)
                        print("Book successfully returned.")
                        break
                else:
                    print("You have entered the wrong book name. Please enter again correctly.")
            elif customerAction == "4":
                break
            else:
                print("Invalid input. Please try again.")
    else:
        print("It seems that you have entered the wrong ID. Please try again")
```
