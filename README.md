- 👋 Hi, I’m @Krishnapriyaok99
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
Krishnapriyaok99/Krishnapriyaok99 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Structure to represent a class
struct Class {
    int standard;
    char division;
};

// Structure to represent a student
struct Student {
    char name[50];
    int roll_no;
    char mobile_no[15];
    struct Class class_info;
    struct Student* next;
};

// Node structure for the linked list
struct Node {
    union {
        struct Student student_data;
        struct Class class_data;
    } data;
    int is_student; // Indicates whether the node contains student or class data
    struct Node* next;
};

// Function to create a new node (either student or class)
struct Node* createNode(void* data, int is_student) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    if (newNode != NULL) {
        if (is_student) {
            memcpy(&(newNode->data.student_data), data, sizeof(struct Student));
        } else {
            memcpy(&(newNode->data.class_data), data, sizeof(struct Class));
        }
        newNode->is_student = is_student;
        newNode->next = NULL;
    }
    return newNode;
}

// Function to print student data
void printStudent(struct Student* student) {
    printf("Name: %s\n", student->name);
    printf("Roll No: %d\n", student->roll_no);
    printf("Mobile No: %s\n", student->mobile_no);
    printf("Class: %d%c\n", student->class_info.standard, student->class_info.division);
}

// Function to print class data
void printClass(struct Class* class_info) {
    printf("Standard: %d\n", class_info->standard);
    printf("Division: %c\n", class_info->division);
}

// Function to create a student
struct Node* createStudent(char name[], int roll_no, char mobile_no[], int standard, char division) {
    struct Student newStudent;
    snprintf(newStudent.name, sizeof(newStudent.name), "%s", name);
    newStudent.roll_no = roll_no;
    snprintf(newStudent.mobile_no, sizeof(newStudent.mobile_no), "%s", mobile_no);
    newStudent.class_info.standard = standard;
    newStudent.class_info.division = division;

    return createNode(&newStudent, 1);
}

// Function to create a class
struct Node* createClass(int standard, char division) {
    struct Class newClass;
    newClass.standard = standard;
    newClass.division = division;

    return createNode(&newClass, 0);
}

// Function to update student's class
void updateStudentClass(struct Node* studentNode, int standard, char division) {
    if (studentNode->is_student) {
        studentNode->data.student_data.class_info.standard = standard;
        studentNode->data.student_data.class_info.division = division;
    }
}

// Function to delete a node (either student or class)
void deleteNode(struct Node* prevNode, struct Node* currentNode) {
    prevNode->next = currentNode->next;
    free(currentNode);
}

// Function to read all students in a class
void readStudentsInClass(struct Node* head, int standard, char division) {
    struct Node* current = head->next;
    while (current != NULL) {
        if (current->is_student &&
            current->data.student_data.class_info.standard == standard &&
            current->data.student_data.class_info.division == division) {
            printStudent(&(current->data.student_data));
        }
        current = current->next;
    }
}

// Function to read all students in a standard
void readStudentsInStandard(struct Node* head, int standard) {
    struct Node* current = head->next;
    while (current != NULL) {
        if (current->is_student && current->data.student_data.class_info.standard == standard) {
            printStudent(&(current->data.student_data));
        }
        current = current->next;
    }
}

int main() {
    // Example Usage
    struct Node* head = createNode(NULL, 0); // Dummy head node

    // Create Students
    struct Node* student1 = createStudent("John Doe", 101, "123-456-7890", 10, 'A');
    struct Node* student2 = createStudent("Jane Smith", 102, "987-654-3210", 11, 'B');

    // Create Classes
    struct Node* class1 = createClass(10, 'A');
    struct Node* class2 = createClass(11, 'B');

    // Link nodes
    head->next = student1;
    student1->next = student2;
    student2->next = class1;
    class1->next = class2;

    // Read all students in a class
    printf("\nStudents in Class 10A:\n");
    readStudentsInClass(head, 10, 'A');

    // Read all students in a standard
    printf("\nStudents in Standard 11:\n");
    readStudentsInStandard(head, 11);

    // Update Student's Class
    updateStudentClass(student1, 12, 'C');

    // Delete Node (Student)
    printf("\nAfter Deleting Student Jane Smith:\n");
    deleteNode(head, student2);

    // Read all students in a class after update and delete
    printf("\nStudents in Class 12C:\n");
    readStudentsInClass(head, 12, 'C');

    return 0;
}
