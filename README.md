#include <iostream>
#include <fstream>
#include <dirent.h>
#include <sys/stat.h>
#include <string>

using namespace std;

void createDirectory(string dirName) {
    string dirPath = "./" + dirName;
    if (mkdir(dirPath.c_str(), 0777) == -1) {
        cerr << "Error creating directory: " << dirName << endl;
    } else {
        cout << "Directory created: " << dirName << endl;
    }
}

void createFile(string fileName) {
    string filePath = "./" + fileName;
    ofstream file(filePath);
    if (!file.is_open()) {
        cerr << "Error creating file: " << fileName << endl;
    } else {
        cout << "File created: " << fileName << endl;
        file.close();
    }
}

void deleteFile(string fileName) {
    string filePath = "./" + fileName;
    if (remove(filePath.c_str()) == -1) {
        cerr << "Error deleting file: " << fileName << endl;
    } else {
        cout << "File deleted: " << fileName << endl;
    }
}

void listFiles() {
    DIR* dir;
    struct dirent* ent;
    if ((dir = opendir("./")) != NULL) {
        while ((ent = readdir(dir)) != NULL) {
            cout << ent->d_name << endl;
        }
        closedir(dir);
    } else {
        cerr << "Error opening directory" << endl;
    }
}

int main() {
    int choice;
    string fileName, dirName;

    while (true) {
        cout << "File Management Program" << endl;
        cout << "----------------------" << endl;
        cout << "1. Create Directory" << endl;
        cout << "2. Create File" << endl;
        cout << "3. Delete File" << endl;
        cout << "4. List Files" << endl;
        cout << "5. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter directory name: ";
                cin >> dirName;
                createDirectory(dirName);
                break;
            case 2:
                cout << "Enter file name: ";
                cin >> fileName;
                createFile(fileName);
                break;
            case 3:
                cout << "Enter file name: ";
                cin >> fileName;
                deleteFile(fileName);
                break;
            case 4:
                listFiles();
                break;
            case 5:
                return 0;
            default:
                cout << "Invalid choice" << endl;
        }
    }

    return 0;
}
