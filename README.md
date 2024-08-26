# Readme
### Code Documentation with Comments

```python
# Importing abstract base classes to define interfaces
from abc import ABC, abstractmethod

# Interface for student data repository (ISP and DIP)
class StudentRepository(ABC):
    @abstractmethod
    def add_student(self, student):
        pass
    
    @abstractmethod
    def remove_student(self, student_id):
        pass
    
    @abstractmethod
    def find_student_by_id(self, student_id):
        pass
    
    @abstractmethod
    def get_all_students(self):
        pass

# Class representing a Student (SRP)
class Student:
    def __init__(self, id, name, age, major):
        # Initialize student with basic information
        self.id = id
        self.name = name
        self.age = age
        self.major = major

    # Method to update student information (SRP)
    def update(self, name=None, age=None, major=None):
        if name:
            self.name = name
        if age:
            self.age = age
        if major:
            self.major = major

    # String representation of the student, used for display
    def __str__(self):
        return f"ID: {self.id}, Name: {self.name}, Age: {self.age}, Major: {self.major}"

# Concrete implementation of the StudentRepository (DIP)
class InMemoryStudentRepository(StudentRepository):
    def __init__(self):
        # In-memory storage for student objects
        self.students = []

    def add_student(self, student):
        # Add a student to the repository
        self.students.append(student)

    def remove_student(self, student_id):
        # Find and remove a student by ID
        student = self.find_student_by_id(student_id)
        if student:
            self.students.remove(student)

    def find_student_by_id(self, student_id):
        # Find a student by their ID
        for student in self.students:
            if student.id == student_id:
                return student
        return None

    def get_all_students(self):
        # Return a list of all students
        return self.students

# Main class for the Student Management System (SRP, OCP, DIP)
class StudentManagementSystem:
    def __init__(self, repository: StudentRepository):
        # Injecting a repository dependency (DIP)
        self.repository = repository

    def add_new_student(self, id, name, age, major):
        # Create and add a new student to the system
        student = Student(id, name, age, major)
        self.repository.add_student(student)

    def delete_student(self, student_id):
        # Delete a student from the system by ID
        self.repository.remove_student(student_id)

    def update_student_info(self, student_id, name=None, age=None, major=None):
        # Update student information by ID
        student = self.repository.find_student_by_id(student_id)
        if student:
            student.update(name, age, major)

    def show_all_students(self):
        # Display all students in the system
        students = self.repository.get_all_students()
        for student in students:
            print(student)

# Function to display the text-based menu for user interaction
def display_menu():
    print("\nStudent Management System Menu")
    print("1. Add a new student")
    print("2. Delete a student")
    print("3. Update student information")
    print("4. View all students")
    print("5. Exit")

# Main function to run the system
def main():
    # Initialize the student management system with an in-memory repository
    system = StudentManagementSystem(InMemoryStudentRepository())
    
    while True:
        # Display the menu and prompt user for a choice
        display_menu()
        choice = input("Enter your choice: ")
        
        if choice == "1":
            # Add a new student
            id = int(input("Enter student ID: "))
            name = input("Enter student name: ")
            age = int(input("Enter student age: "))
            major = input("Enter student major: ")
            system.add_new_student(id, name, age, major)
            print("Student added successfully.")
        
        elif choice == "2":
            # Delete a student
            id = int(input("Enter student ID to delete: "))
            system.delete_student(id)
            print("Student deleted successfully.")
        
        elif choice == "3":
            # Update a student's information
            id = int(input("Enter student ID to update: "))
            name = input("Enter new name (leave blank to keep current): ")
            age = input("Enter new age (leave blank to keep current): ")
            major = input("Enter new major (leave blank to keep current): ")
            system.update_student_info(id, name or None, int(age) if age else None, major or None)
            print("Student information updated successfully.")
        
        elif choice == "4":
            # Display all students
            system.show_all_students()
        
        elif choice == "5":
            # Exit the system
            print("Exiting the system.")
            break
        
        else:
            # Handle invalid menu choices
            print("Invalid choice. Please try again.")

# Entry point of the application
if __name__ == "__main__":
    main()
```

### README File for GitHub Repository

```markdown
# Student Management System

## Overview

The Student Management System is a simple Python application that allows users to manage student data for a small educational institution. It provides functionality to add, delete, update, and view students. The system is designed following best practices and principles like SOLID, DRY, KISS, and YAGNI, ensuring that the code is maintainable, scalable, and easy to understand.

## Key Features

- **Add a New Student:** Input student details to add them to the system.
- **Delete a Student:** Remove a student from the system by their ID.
- **Update Student Information:** Modify the details of an existing student.
- **View All Students:** Display the list of all students currently in the system.

## Design Principles

The system was refactored to adhere to several important software design principles:

- **Single Responsibility Principle (SRP):** Each class has a single responsibility, making the code easier to maintain and extend.
- **Open/Closed Principle (OCP):** The system is designed to be open for extension but closed for modification, allowing new features to be added without altering existing code.
- **Liskov Substitution Principle (LSP):** Although inheritance isn't used here, the code design ensures that if subclasses were introduced, they could be used interchangeably without issues.
- **Interface Segregation Principle (ISP):** The code uses interfaces to ensure that clients only depend on the methods they actually use.
- **Dependency Inversion Principle (DIP):** The `StudentManagementSystem` depends on abstractions rather than concrete implementations, making it more flexible and easier to test.
- **DRY (Don't Repeat Yourself):** Common logic, such as finding a student by ID, is encapsulated in a single method to avoid redundancy.
- **KISS (Keep It Simple Stupid):** The code is simple, easy to understand, and free of unnecessary complexity.
- **YAGNI (You Ain't Gonna Need It):** The design avoids adding features that are not necessary for the current use cases.

## How to Run the System

1. **Clone the Repository:**
   ```
   git clone https://github.com/your-username/student-management-system.git
   ```
   
2. **Navigate to the Project Directory:**
   ```
   cd student-management-system
   ```

3. **Run the Application:**
   ```
   python student_management_system.py
   ```

4. **Interact with the Menu:**
   - You will be presented with a text-based menu.
   - Choose options to add, delete, update, or view students.
   - The system will respond based on your inputs.

## Example Usage

```
Student Management System Menu
1. Add a new student
2. Delete a student
3. Update student information
4. View all students
5. Exit
```

Follow the on-screen prompts to interact with the system.

## Changes Made in Refactoring

- **Separated Concerns:** The `Student` class now only handles student data, while operations are handled by `StudentManagementSystem` and `StudentRepository`.
- **Implemented Repository Pattern:** The `StudentDatabase` has been replaced by an in-memory repository that implements a `StudentRepository` interface.
- **Simplified Code:** The code has been refactored to remove redundancy and ensure it adheres to the KISS principle.
- **Added a Menu System:** A simple text-based menu was introduced to make the system user-friendly.

## Future Enhancements

- **Persistent Storage:** Implement a database or file storage system to replace the in-memory repository.
- **User Authentication:** Add user roles and authentication for added security.
- **GUI:** Develop a graphical user interface for easier interaction.
