#include <stdio.h>
#include <string.h>
#include <stdlib.h>

struct Student {
    int rollNo;
    char name[50];
    float marks;
};

struct Student students[100];
int n = 0;
int isSorted = 0;

void insertStudent();
void deleteStudent();
void updateStudent();
void displayStudents();
void searchMenu();
void sortMenu();
void filterStudents();

void linearSearch();
void binarySearch();
void interpolationSearch();

void swap(struct Student *a, struct Student *b);
void bubbleSort();
void selectionSort();
void insertionSort();
int partition(int low, int high);
void quickSort(int low, int high);
void merge(int l, int m, int r);
void mergeSort(int l, int r);

int main() {
    int choice;

    do {
        printf("\n📘 === Student Record Manager === 📘\n");
        printf("1. Insert Student\n");
        printf("2. Delete Student\n");
        printf("3. Update Student\n");
        printf("4. Display All Students\n");
        printf("5. Search for a Student\n");
        printf("6. Sort Student Records\n");
        printf("7. Filter Students by Marks\n");
        printf("8. Exit\n");
        printf("------------------------------------\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                insertStudent();
                break;
            case 2:
                deleteStudent();
                break;
            case 3:
                updateStudent();
                break;
            case 4:
                displayStudents();
                break;
            case 5:
                searchMenu();
                break;
            case 6:
                sortMenu();
                break;
            case 7:
                filterStudents();
                break;
            case 8:
                printf("Exiting program. Goodbye! 👋\n");
                break;
            default:
                printf("❌ Invalid choice! Please try again.\n");
        }
    } while (choice != 8);

    return 0;
}

void insertStudent() {
    if (n >= 100) {
        printf("❌ Error: Database is full. Cannot add more students.\n");
        return;
    }
    printf("\n--- Add New Student ---\n");
    printf("Enter Roll Number: ");
    scanf("%d", &students[n].rollNo);
    printf("Enter Name: ");
    scanf(" %[^\n]s", students[n].name);
    printf("Enter Marks: ");
    scanf("%f", &students[n].marks);

    n++;
    isSorted = 0;
    printf("✅ Student added successfully!\n");
}

void displayStudents() {
    if (n == 0) {
        printf("\nNo records to display. The database is empty.\n");
        return;
    }
    printf("\n--- All Student Records ---\n");
    printf("--------------------------------------------------\n");
    printf("%-10s %-20s %-10s\n", "Roll No", "Name", "Marks");
    printf("--------------------------------------------------\n");
    for (int i = 0; i < n; i++) {
        printf("%-10d %-20s %-10.2f\n", students[i].rollNo, students[i].name, students[i].marks);
    }
    printf("--------------------------------------------------\n");
}

void deleteStudent() {
    if (n == 0) {
        printf("❌ Error: No students to delete.\n");
        return;
    }
    int rollNo, found = -1;
    printf("\nEnter Roll Number of the student to delete: ");
    scanf("%d", &rollNo);

    for (int i = 0; i < n; i++) {
        if (students[i].rollNo == rollNo) {
            found = i;
            break;
        }
    }

    if (found == -1) {
        printf("❌ Student with Roll Number %d not found.\n", rollNo);
    } else {
        for (int i = found; i < n - 1; i++) {
            students[i] = students[i + 1];
        }
        n--;
        printf("✅ Student with Roll Number %d deleted successfully.\n", rollNo);
    }
}

void updateStudent() {
     if (n == 0) {
        printf("❌ Error: No students to update.\n");
        return;
    }
    int rollNo, choice, found = -1;
    printf("\nEnter Roll Number of the student to update: ");
    scanf("%d", &rollNo);

    for (int i = 0; i < n; i++) {
        if (students[i].rollNo == rollNo) {
            found = i;
            break;
        }
    }

    if (found == -1) {
        printf("❌ Student with Roll Number %d not found.\n", rollNo);
    } else {
        printf("Student Found: %s\n", students[found].name);
        printf("What do you want to update?\n");
        printf("1. Name\n");
        printf("2. Marks\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch(choice) {
            case 1:
                printf("Enter new name: ");
                scanf(" %[^\n]s", students[found].name);
                printf("✅ Name updated successfully.\n");
                break;
            case 2:
                printf("Enter new marks: ");
                scanf("%f", &students[found].marks);
                printf("✅ Marks updated successfully.\n");
                break;
            default:
                printf("❌ Invalid choice.\n");
        }
    }
}

void filterStudents() {
    if (n == 0) {
        printf("❌ No records to filter.\n");
        return;
    }
    float minMarks;
    int count = 0;
    printf("\nEnter minimum marks to filter: ");
    scanf("%f", &minMarks);

    printf("\n--- Students with Marks >= %.2f ---\n", minMarks);
    printf("--------------------------------------------------\n");
    printf("%-10s %-20s %-10s\n", "Roll No", "Name", "Marks");
    printf("--------------------------------------------------\n");

    for (int i = 0; i < n; i++) {
        if (students[i].marks >= minMarks) {
            printf("%-10d %-20s %-10.2f\n", students[i].rollNo, students[i].name, students[i].marks);
            count++;
        }
    }

    if (count == 0) {
        printf("No students found with marks >= %.2f\n", minMarks);
    }
    printf("--------------------------------------------------\n");
}

void searchMenu() {
    int choice;
    printf("\n--- Search Menu ---\n");
    printf("1. Linear Search\n");
    printf("2. Binary Search (Requires sorted data by Roll No)\n");
    printf("3. Interpolation Search (Requires sorted data by Roll No)\n");
    printf("Enter your choice: ");
    scanf("%d", &choice);

    switch (choice) {
        case 1:
            linearSearch();
            break;
        case 2:
            if (!isSorted) {
                printf("⚠️ Warning: Data is not sorted by Roll No. Please sort it first for accurate results.\n");
            }
            binarySearch();
            break;
        case 3:
             if (!isSorted) {
                printf("⚠️ Warning: Data is not sorted by Roll No. Please sort it first for accurate results.\n");
            }
            interpolationSearch();
            break;
        default:
            printf("❌ Invalid choice!\n");
    }
}

void sortMenu() {
    int choice;
    printf("\n--- Sort Menu (by Roll Number) ---\n");
    printf("1. Bubble Sort\n");
    printf("2. Selection Sort\n");
    printf("3. Insertion Sort\n");
    printf("4. Quick Sort\n");
    printf("5. Merge Sort\n");
    printf("Enter your choice: ");
    scanf("%d", &choice);

    switch (choice) {
        case 1: bubbleSort(); break;
        case 2: selectionSort(); break;
        case 3: insertionSort(); break;
        case 4: quickSort(0, n - 1); break;
        case 5: mergeSort(0, n - 1); break;
        default: printf("❌ Invalid choice!\n"); return;
    }
    isSorted = 1;
    printf("✅ Records sorted successfully by Roll Number.\n");
    displayStudents();
}

void linearSearch() {
    if (n == 0) { printf("No data to search.\n"); return; }
    int rollNo, found = 0;
    printf("Enter Roll Number to search: ");
    scanf("%d", &rollNo);

    for (int i = 0; i < n; i++) {
        if (students[i].rollNo == rollNo) {
            printf("\n--- Student Found! ---\n");
            printf("Roll No: %d\nName: %s\nMarks: %.2f\n", students[i].rollNo, students[i].name, students[i].marks);
            found = 1;
            break;
        }
    }
    if (!found) {
        printf("❌ Student with Roll Number %d not found.\n", rollNo);
    }
}

void binarySearch() {
    if (n == 0) { printf("No data to search.\n"); return; }
    int rollNo;
    printf("Enter Roll Number to search: ");
    scanf("%d", &rollNo);

    int low = 0, high = n - 1, mid;
    while (low <= high) {
        mid = low + (high - low) / 2;
        if (students[mid].rollNo == rollNo) {
            printf("\n--- Student Found! ---\n");
            printf("Roll No: %d\nName: %s\nMarks: %.2f\n", students[mid].rollNo, students[mid].name, students[mid].marks);
            return;
        }
        if (students[mid].rollNo < rollNo) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    printf("❌ Student with Roll Number %d not found.\n", rollNo);
}

void interpolationSearch() {
    if (n == 0) {
        printf("No data to search.\n");
        return;
    }
    int rollNo;
    printf("Enter Roll Number to search: ");
    scanf("%d", &rollNo);

    int lo = 0, hi = (n - 1);
    while (lo <= hi && rollNo >= students[lo].rollNo && rollNo <= students[hi].rollNo) {
        if (lo == hi) {
            if (students[lo].rollNo == rollNo) {
                 printf("\n--- Student Found! ---\n");
                 printf("Roll No: %d\nName: %s\nMarks: %.2f\n", students[lo].rollNo, students[lo].name, students[lo].marks);
                 return;
            }
            break;
        }
        
        // Corrected line with explicit cast to silence the warning
        int pos = lo + (int)(((double)(hi - lo) / (students[hi].rollNo - students[lo].rollNo)) * (rollNo - students[lo].rollNo));

        if (students[pos].rollNo == rollNo) {
             printf("\n--- Student Found! ---\n");
             printf("Roll No: %d\nName: %s\nMarks: %.2f\n", students[pos].rollNo, students[pos].name, students[pos].marks);
             return;
        }
        if (students[pos].rollNo < rollNo)
            lo = pos + 1;
        else
            hi = pos - 1;
    }
    printf("❌ Student with Roll Number %d not found.\n", rollNo);
}

void swap(struct Student *a, struct Student *b) {
    struct Student temp = *a;
    *a = *b;
    *b = temp;
}

void bubbleSort() {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (students[j].rollNo > students[j + 1].rollNo) {
                swap(&students[j], &students[j + 1]);
            }
        }
    }
}

void selectionSort() {
    int i, j, min_idx;
    for (i = 0; i < n - 1; i++) {
        min_idx = i;
        for (j = i + 1; j < n; j++) {
            if (students[j].rollNo < students[min_idx].rollNo) {
                min_idx = j;
            }
        }
        swap(&students[min_idx], &students[i]);
    }
}

void insertionSort() {
    int i, j;
    struct Student key;
    for (i = 1; i < n; i++) {
        key = students[i];
        j = i - 1;
        while (j >= 0 && students[j].rollNo > key.rollNo) {
            students[j + 1] = students[j];
            j = j - 1;
        }
        students[j + 1] = key;
    }
}

int partition(int low, int high) {
    int pivot = students[high].rollNo;
    int i = (low - 1);
    for (int j = low; j <= high - 1; j++) {
        if (students[j].rollNo < pivot) {
            i++;
            swap(&students[i], &students[j]);
        }
    }
    swap(&students[i + 1], &students[high]);
    return (i + 1);
}

void quickSort(int low, int high) {
    if (low < high) {
        int pi = partition(low, high);
        quickSort(low, pi - 1);
        quickSort(pi + 1, high);
    }
}

void merge(int l, int m, int r) {
    int i, j, k;
    int n1 = m - l + 1;
    int n2 = r - m;
    struct Student L[n1], R[n2];
    for (i = 0; i < n1; i++) L[i] = students[l + i];
    for (j = 0; j < n2; j++) R[j] = students[m + 1 + j];
    i = 0; j = 0; k = l;
    while (i < n1 && j < n2) {
        if (L[i].rollNo <= R[j].rollNo) {
            students[k] = L[i]; i++;
        } else {
            students[k] = R[j]; j++;
        }
        k++;
    }
    while (i < n1) { students[k] = L[i]; i++; k++; }
    while (j < n2) { students[k] = R[j]; j++; k++; }
}

void mergeSort(int l, int r) {
    if (l < r) {
        int m = l + (r - l) / 2;
        mergeSort(l, m);
        mergeSort(m + 1, r);
        merge(l, m, r);
    }
}
