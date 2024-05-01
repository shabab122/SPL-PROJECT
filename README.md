# SPL-PROJECT
School Management System

#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

struct student {

    char name[500];
    char cls[100];
    int roll;
    char date[12];
};

FILE *fp;
long size = sizeof(struct student);

void Input();
void Display();
void Search();
void searchByRoll();
void searchByName();
void Modify();
void Delete();
void Sort();
int DoesRollExist(int);

int main() {

    int choice;

    while (1) {

        system("Color 70");

        system("cls");

        printf("\n\n\t\t\t <==>  SCHOOL MANAGEMENT SYSTEM  <==>\n");
        printf("\n Welcome to Our Website. Please Choose an Option Between 1-6 for Further Proceedings and if you are Looking for an Exit then Choose Option 0.\n");
        printf("\n\t 1. Take Student Admission.\n");
        printf("\n\t 2. Student Information.\n");
        printf("\n\t 3. Search by Information.\n");
        printf("\n\t 4. Modify Student Data.\n");
        printf("\n\t 5. Delete Student Data.\n");
        printf("\n\t 6. Sort Student Data by Roll.\n");
        printf("\n\t 0. Exit.\n\n");
        printf("Enter your Choice :");

        scanf("%d", &choice);

        switch (choice) {
            case 0:
                exit(0);

            case 1:
                Input();
                break;

            case 2:
                Display();
                break;

            case 3:
                Search();
                break;

            case 4:
                Modify();
                break;

            case 5:
                Delete();
                break;

            case 6:
                Sort();
                break;

            default:
                printf("Invalid Choice.\n");
        }
        printf("\n\n\n\n Press any Key to Continue... ");

        getchar();
        getchar();
    }

    printf("\t\t\t THE PROGRAM RAN SUCCESSFULLY. ");
    printf("\n\t\t Thank you.");

    return 0;
}

void Input() {

    struct student S;
    char myDate[12];
    time_t t = time(NULL);
    struct tm *tm = localtime(&t);
    sprintf(myDate, "%02d/%02d/%d", tm->tm_mday, tm->tm_mon + 1, tm->tm_year + 1900);
    strcpy(S.date, myDate);

    printf("\n\t Enter Student Name :");
    scanf(" %499[^\n]",S.name);
    printf("\n\t Enter Class :");
     scanf(" %99[^\n]",S.cls);
    printf("\n\t Enter Roll :");
    scanf("%d", &S.roll);

    if (DoesRollExist(S.roll))
    {
         printf("\n\t\t Roll Already Exists.");
    }
    else{

        fp = fopen("stud.txt", "ab");
        if (fp == NULL) {
            printf("Error Opening File.\n");
            return;
        }
        fwrite(&S, size, 1, fp);
        printf("\n\t\t Record Saved Successfully....");
     fclose(fp);
    }

}

int DoesRollExist(int r1) {

    fp = fopen("stud.txt", "rb");
    if (fp == NULL) {
        printf("Error Opening File.\n");
        return 0;
    }

    struct student S;

    int roll_exist=0;
    while (fread(&S, size, 1, fp) == 1) {
        if(S.roll == r1)
        {
            roll_exist=1;
        }
    }
    fclose(fp);
    return roll_exist;
}

void Display() {

    system("cls");

    printf("\t\t <<<   STUDENT INFORMATION  >>>\n\n");
    printf("%-30s %-20s %-10s %s\n", "Name", "Class", "Roll", "Date");
    fp = fopen("stud.txt", "rb");

    if (fp == NULL) {
        printf("Error Opening File.\n");
        return;
    }

    struct student S;

    while (fread(&S, size, 1, fp) == 1) {
        printf("%-30s %-20s %-10d %s\n", S.name, S.cls, S.roll, S.date);
    }
    fclose(fp);
}

void Search() {
    int choice;

    while (1) {
        system("cls");

        printf("\n\t\t\t <<<   SEARCH INFORMATION  >>>\n");
        printf("\t\n 1. Search by Roll.\n");
        printf("\t\n 2. Search by Name.\n");
        printf("\t\n 0. Back to Main Menu.\n");

        printf("\n\n Enter your Choice : ");
        scanf("%d", &choice);

        switch (choice) {
            case 0:
                return;

            case 1:
                searchByRoll();
                break;

            case 2:
                searchByName();
                break;

            default:
                printf("Invalid Choice.\n");
        }

        getchar();
        printf("\n\n Press Enter to Continue... ");
        getchar();
    }
}

void searchByRoll() {

    int r1, flag = 0;
    printf("\n\t Enter Roll to Search :");
    scanf("%d", &r1);
    printf("%-30s %-20s %-10s %s\n", "Name", "Class", "Roll", "Date");

    fp = fopen("stud.txt", "rb");
    if (fp == NULL) {
        printf("Error Opening File.\n");
        return;
    }

    struct student S;

    while (fread(&S, size, 1, fp) == 1) {
        if (r1 == S.roll) {
            flag = 1;
            printf("%-30s %-20s %-10d %s\n", S.name, S.cls, S.roll, S.date);
            break;
        }
    }
    fclose(fp);

    if (flag == 0) {
        printf("\n\t Record not found.\n");
    } else {
        printf("\n\t Record found successfully.\n");
    }
}

void searchByName() {

    struct student S;

    char name[500];
    int flag = 0;
    printf("\n\t Enter Name to Search :");
    scanf(" %499[^\n]",S.name);

    printf("%-30s %-20s %-10s %s\n", "Name", "Class", "Roll", "Date");

    fp = fopen("stud.txt", "rb");
    if (fp == NULL) {
        printf("Error Opening File.\n");
        return;
    }

    while (fread(&S,size, 1, fp) == 1) {
        if (strcmp(name, S.name) == 0) {
            flag = 1;
            printf("%-30s %-20s %-10d %s\n", S.name, S.cls, S.roll, S.date);
            break;
        }
    }
    fclose(fp);

    if (flag == 0) {
        printf("\n\t Record not found.\n");
    } else {
        printf("\n\t Record found successfully.\n");
    }
}

void Modify() {

    int r1, flag = 0;
    printf("\n\t Enter Roll to Modify :");
    scanf("%d", &r1);

    fp = fopen("stud.txt", "rb+");
    if (fp == NULL) {
        printf("Error opening file.\n");
        return;
    }

    struct student S;

    while (fread(&S, size, 1, fp) == 1) {
        if (r1 == S.roll) {
            flag = 1;
            printf("\n\t Enter New Student Name :");
            scanf(" %499[^\n]",S.name);
            printf("\n\t Enter New Class :");
             scanf(" %99[^\n]",S.cls);
            printf("\n\t Enter New Roll :");
            scanf("%d", &S.roll);
            fseek(fp, -size, SEEK_CUR);
            fwrite(&S, size, 1, fp);
            fclose(fp);
        }
    }
    if (flag == 0) {
        printf("\n\t Record not found.\n");
    } else {
        printf("\n\t Record Modified successfully.\n");
    }
}

void Delete() {

    int r1, flag = 0;
    printf("\n\t Enter Roll to Delete :");
    scanf("%d", &r1);

    FILE *ft;

    fp = fopen("stud.sa", "rb");
    if (fp == NULL) {
        printf("Error Opening File.\n");
        return;
    }
    ft = fopen("temp.sa", "ab");
    if (ft == NULL) {
        printf("Error Opening File.\n");
        fclose(fp);
        return;
    }

    struct student S;

    while (fread(&S, size, 1, fp) == 1) {
        if (r1 == S.roll) {
            flag = 1;
        } else {
            fwrite(&S, size, 1, ft);
        }
    }

    fclose(fp);
    fclose(ft);
    remove("stud.txt");
    rename("temp.txt", "stud.txt");

    if (flag == 0) {
        printf("\n\t Record not found.\n");
    } else {
        printf("\n\t Record Deleted successfully.\n");
    }
}

void Sort() {

    int i, j, c = 0;
    struct student S1[100], temp;

    system("cls");
    printf("\t<<<   SORT BY ROLL   >>>\n\n");
    printf("%-30s %-20s %-10s %s\n", "Name", "Class", "Roll", "Date");

    fp = fopen("stud.txt", "rb");
    if (fp == NULL) {
        printf("\n\n\t Error Opening File.\n");
        return;
    }

    struct student S;

    while (fread(&S, size, 1, fp) == 1) {
        S1[c++] = S;
    }
    fclose(fp);

    for (i = 0; i < c - 1; i++) {
        for (j = i + 1; j < c; j++) {
            if (S1[i].roll > S1[j].roll) {
                temp = S1[i];
                S1[i] = S1[j];
                S1[j] = temp;
            }
        }
    }
    for (i = 0; i < c; i++) {
        printf("%-30s %-20s %-10d %s\n", S1[i].name, S1[i].cls, S1[i].roll, S1[i].date);
    }
}


