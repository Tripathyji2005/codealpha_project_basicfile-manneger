#include <iostream>
#include <fstream>
#include <dirent.h>
#include <string>

using namespace std;

// Function to create a new file
void createFile(const string& filename) {
    ofstream file(filename);
    if (file.is_open()) {
        file << "This is a new file." << endl;
        file.close();
        cout << "File created successfully." << endl;
    } else {
        cout << "Unable to create file." << endl;
    }
}

// Function to delete a file
void deleteFile(const string& filename) {
    if (remove(filename.c_str()) == 0) {
        cout << "File deleted successfully." << endl;
    } else {
        cout << "Unable to delete file." << endl;
    }
}

// Function to list files in the current directory
void listFiles() {
    DIR *dir;
    struct dirent *ent;
    dir = opendir(".");
    if (dir != NULL) {
        while ((ent = readdir(dir)) != NULL) {
            cout << ent->d_name << endl;
        }
        closedir(dir);
    } else {
        cout << "Unable to open directory." << endl;
    }
}

int main() {
    int choice;
    string filename;

    while (true) {
        cout << "File Management Menu:" << endl;
        cout << "1. Create a new file" << endl;
        cout << "2. Delete a file" << endl;
        cout << "3. List files" << endl;
        cout << "4. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter the filename: ";
                cin >> filename;
                createFile(filename);
                break;
            case 2:
                cout << "Enter the filename: ";
                cin >> filename;
                deleteFile(filename);
                break;
            case 3:
                listFiles();
                break;
            case 4:
                return 0;
            default:
                cout << "Invalid choice. Please try again." << endl;
        }
    }

    return 0;
}
