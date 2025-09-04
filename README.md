#include <stdio.h>
#include <stdlib.h>

struct Student {
    int roll;
    char name[50];
    float marks;
};

void addStudent() {
    FILE *fp;
    struct Student st;
    fp = fopen("student.txt", "a");  // append mode
    if (fp == NULL) {
        printf("File could not be opened!\n");
        return;
    }

    printf("Enter Roll Number: ");
    scanf("%d", &st.roll);
    getchar(); // buffer clear

    printf("Enter Name: ");
    fgets(st.name, sizeof(st.name), stdin);

    printf("Enter Marks: ");
    scanf("%f", &st.marks);

    fwrite(&st, sizeof(st), 1, fp);
    fclose(fp);
    printf("✅ Student record added successfully!\n");
}

void displayStudents() {
    FILE *fp;
    struct Student st;
    fp = fopen("student.txt", "r");
    if (fp == NULL) {
        printf("No data found!\n");
        return;
    }

    printf("\n--- Student Records ---\n");
    while (fread(&st, sizeof(st), 1, fp)) {
        printf("Roll: %d\n", st.roll);
        printf("Name: %s", st.name);
        printf("Marks: %.2f\n", st.marks);
        printf("---------------------\n");
    }
    fclose(fp);
}

void searchStudent() {
    FILE *fp;
    struct Student st;
    int roll, found = 0;

    printf("Enter Roll Number to Search: ");
    scanf("%d", &roll);

    fp = fopen("student.txt", "r");
    if (fp == NULL) {
        printf("No data found!\n");
        return;
    }

    while (fread(&st, sizeof(st), 1, fp)) {
        if (st.roll == roll) {
            printf("\n✅ Record Found!\n");
            printf("Roll: %d\n", st.roll);
            printf("Name: %s", st.name);
            printf("Marks: %.2f\n", st.marks);
            found = 1;
            break;
        }
    }
    if (!found)
        printf("❌ Record not found!\n");

    fclose(fp);
}

int main() {
    int choice;
    do {
        printf("\n====== Student Result Management System ======\n");
        printf("1. Add Student Record\n");
        printf("2. Display All Records\n");
        printf("3. Search Student Record\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar();

        switch (choice) {
            case 1: addStudent(); break;
            case 2: displayStudents(); break;
            case 3: searchStudent(); break;
            case 4: printf("Exiting...\n"); break;
            default: printf("Invalid Choice!\n");
        }
    } while (choice != 4);

    return 0;
}
