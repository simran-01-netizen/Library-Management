# Library-Management
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Book {
int id;
char title[100];
char author[100];
};

void addBook() {
struct Book b;
FILE *fp = fopen("library.dat", "ab");
printf("Enter Book ID: ");
scanf("%d", &b.id);
printf("Enter Book Title: ");
getchar(); // clear newline
fgets(b.title, sizeof(b.title), stdin);
printf("Enter Book Author: ");
fgets(b.author, sizeof(b.author), stdin);
fwrite(&b, sizeof(b), 1, fp);
fclose(fp);
printf("Book added successfully!\n");
}

void displayBooks() {
struct Book b;
FILE *fp = fopen("library.dat", "rb");
printf("\n--- Book List ---\n");
while (fread(&b, sizeof(b), 1, fp)) {
printf("ID: %d\nTitle: %sAuthor: %s\n", b.id, b.title, b.author);
}
fclose(fp);
}

void searchBook() {
int id;
struct Book b;
FILE *fp = fopen("library.dat", "rb");
printf("Enter Book ID to search: ");
scanf("%d", &id);
int found = 0;
while (fread(&b, sizeof(b), 1, fp)) {
if (b.id == id) {
printf("Book Found:\nID: %d\nTitle: %sAuthor: %s\n", b.id, b.title, b.author);
found = 1;
break;
}
}
if (!found) printf("Book not found.\n");
fclose(fp);
}

void deleteBook() {
int id;
struct Book b;
FILE *fp = fopen("library.dat", "rb");
FILE *temp = fopen("temp.dat", "wb");
printf("Enter Book ID to delete: ");
scanf("%d", &id);
int found = 0;
while (fread(&b, sizeof(b), 1, fp)) {
if (b.id != id) {
fwrite(&b, sizeof(b), 1, temp);
} else {
found = 1;
}
}
fclose(fp);
fclose(temp);
remove("library.dat");
rename("temp.dat", "library.dat");
if (found)
printf("Book deleted successfully.\n");
else
printf("Book not found.\n");
}

int main() {
int choice;
do {
printf("\nLibrary Management System\n");
printf("1. Add Book\n2. Display Books\n3. Search Book\n4. Delete Book\n5. Exit\n");
printf("Enter your choice: ");
scanf("%d", &choice);
switch (choice) {
case 1: addBook(); break;
case 2: displayBooks(); break;
case 3: searchBook(); break;
case 4: deleteBook(); break;
case 5: printf("Exiting...\n"); break;
default: printf("Invalid choice.\n");
}
} while (choice != 5);
return 0;
}
