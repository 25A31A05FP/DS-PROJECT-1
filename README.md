# Library-Management-System-
#include <iostream>
#include <fstream>
using namespace std;

class Library {
    int book_id;
    char book_name[50];
    char author[50];
    int issued;

public:
    void addBook() {
        ofstream file("library.dat", ios::app);

        cout << "Enter Book ID: ";
        cin >> book_id;

        cout << "Enter Book Name: ";
        cin >> book_name;

        cout << "Enter Author Name: ";
        cin >> author;

        issued = 0;

        file.write((char*)this, sizeof(*this));
        file.close();

        cout << "Book added successfully\n";
    }

    void showBooks() {
        ifstream file("library.dat");

        while (file.read((char*)this, sizeof(*this))) {
            if (issued == 0) {
                cout << "\nBook ID: " << book_id;
                cout << "\nBook Name: " << book_name;
                cout << "\nAuthor: " << author;
                cout << "\n-------------------";
            }
        }

        file.close();
    }

    void issueBook(int id) {
        fstream file("library.dat", ios::in | ios::out);

        while (file.read((char*)this, sizeof(*this))) {
            if (book_id == id && issued == 0) {
                issued = 1;
                int pos = file.tellg();
                file.seekp(pos - sizeof(*this));
                file.write((char*)this, sizeof(*this));
                cout << "Book Issued Successfully\n";
                break;
            }
        }

        file.close();
    }

    void returnBook(int id) {
        fstream file("library.dat", ios::in | ios::out);

        while (file.read((char*)this, sizeof(*this))) {
            if (book_id == id && issued == 1) {
                issued = 0;
                int pos = file.tellg();
                file.seekp(pos - sizeof(*this));
                file.write((char*)this, sizeof(*this));
                cout << "Book Returned Successfully\n";
                break;
            }
        }

        file.close();
    }
};

int main() {
    Library lib;
    int choice, id;

    while (1) {
        cout << "\n\nLibrary Management System";
        cout << "\n1. Add Book";
        cout << "\n2. Show Available Books";
        cout << "\n3. Issue Book";
        cout << "\n4. Return Book";
        cout << "\n5. Exit";
        cout << "\nEnter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                lib.addBook();
                break;

            case 2:
                lib.showBooks();
                break;

            case 3:
                cout << "Enter Book ID to Issue: ";
                cin >> id;
                lib.issueBook(id);
                break;

            case 4:
                cout << "Enter Book ID to Return: ";
                cin >> id;
                lib.returnBook(id);
                break;

            case 5:
                return 0;

            default:
                cout << "Invalid choice!";
        }
    }
}
