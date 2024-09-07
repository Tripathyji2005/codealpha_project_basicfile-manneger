#include <iostream>
#include <filesystem>
#include <fstream>
#include <string>

namespace fs = std::filesystem;

void listFiles(const fs::path& path) {
    try {
        if (fs::exists(path) && fs::is_directory(path)) {
            for (const auto& entry : fs::directory_iterator(path)) {
                std::cout << (fs::is_directory(entry.path()) ? "[DIR] " : "[FILE] ") 
                          << entry.path().filename().string() << '\n';
            }
        } else {
            std::cerr << "Path is not a directory or does not exist.\n";
        }
    } catch (const fs::filesystem_error& e) {
        std::cerr << "Error: " << e.what() << '\n';
    }
}

void createDirectory(const::path& path) {
    try {
        if (!fs::exists(path)) {
            if (fs::create_directory(path)) {
                std::cout << "Directory created successfully.\n";
            } else {
                std::cerr << "Failed to create directory.\n";
            }
        } else {
            std::cerr << "Directory already exists.\n";
        }
    } catch (const fs::filesystem_error& e) {
        std::cerr << "Error: " << e.what() << '\n';
    }
}

void copyFile(const fs::path& source, const fs::path& destination) {
    try {
        if (fs::exists(source) && fs::is_regular_file(source)) {
            fs::copy(source, destination, fs::copy_options::overwrite_existing);
            std::cout << "File copied successfully.\n";
        } else {
            std::cerr << "Source file does not exist or is not a regular file.\n";
        }
    } catch (const fs::filesystem_error& e) {
        std::cerr << "Error: " << e.what() << '\n';
    }
}

void moveFile(const fs::path& source, const fs::path& destination) {
    try {
        if (fs::exists(source) && fs::is_regular_file(source)) {
            fs::rename(source, destination);
            std::cout << "File moved successfully.\n";
        } else {
            std::cerr << "Source file does not exist or is not a regular file.\n";
        }
    } catch (const fs::filesystem_error& e) {
        std::cerr << "Error: " << e.what() << '\n';
    }
}

int main() {
    std::string command;
    std::string arg1, arg2;
    
    std::cout << "Simple File Manager\n";
    std::cout << "Available commands:\n";
    std::cout << "  ls <path>        - List files in the directory\n";
    std::cout << "  mkdir <path>     - Create a new directory\n";
    std::cout << "  cp <source> <dest> - Copy a file\n";
    std::cout << "  mv <source> <dest> - Move a file\n";
    std::cout << "  cd <path>        - Change the current directory\n";
    std::cout << "  exit             - Exit the program\n";

    fs::path currentPath = fs::current_path();

    while (true) {
        std::cout << "\nCurrent Path: " << currentPath << "\n";
        std::cout << "Enter command: ";
        std::getline(std::cin, command);

        std::istringstream iss(command);
        std::string cmd;
        iss >> cmd;

        if (cmd == "ls") {
            iss >> arg1;
            listFiles(arg1.empty() ? currentPath : fs::path(arg1));
        } else if (cmd == "mkdir") {
            iss >> arg1;
            createDirectory(fs::path(arg1));
        } else if (cmd == "cp") {
            iss >> arg1 >> arg2;
            copyFile(fs::path(arg1), fs::path(arg2));
        } else if (cmd == "mv") {
            iss >> arg1 >> arg2;
            moveFile(fs::path(arg1), fs::path(arg2));
        } else if (cmd == "cd") {
            iss >> arg1;
            fs::path newPath = fs::path(arg1);
            if (fs::exists(newPath) && fs::is_directory(newPath)) {
                currentPath = fs::canonical(newPath);
            } else {
                std::cerr << "Invalid directory.\n";
            }
        } else if (cmd == "exit") {
            break;
        } else {
            std::cerr << "Unknown command.\n";
        }
    }

    return 0;
}
