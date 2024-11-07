# File_Management_Sys

This project is a simple file organizer script written in C for Linux (Ubuntu). It organizes files in a directory based on their extensions into specific folders (Images, Documents, Videos).

## Step-by-Step Setup Guide

### Step 1: Install GCC Compiler

We need the GCC compiler to compile our C program. Use the following command to install GCC:

```sh
sudo apt install gcc
```

### Step 2: Create the file_organizer.c File

Create a new C file named file_organizer.c and open it with a text editor (e.g., nano):

```sh
nano file_organizer.c
```

### Step 3: Write the C Program

Copy and paste the following code into the file_organizer.c file:

```sh
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <dirent.h>
#include <sys/stat.h>
#include <unistd.h>

// Helper function to create directories if they don't exist
void create_directory(const char *dir_name) {
    struct stat st = {0};
    if (stat(dir_name, &st) == -1) {
        mkdir(dir_name, 0700);
    }
}

// Function to move files based on type
void move_file(const char *file, const char *destination) {
    char command[256];
    snprintf(command, sizeof(command), "mv \"%s\" \"%s\"", file, destination);
    system(command);
}

// Function to organize files based on extension
void organize_files(const char *dir_name) {
    DIR *dir = opendir(dir_name);
    struct dirent *entry;
    if (!dir) {
        printf("Could not open directory %s\n", dir_name);
        return;
    }

    // File extensions and their corresponding folders
    const char *image_ext[] = {".jpg", ".png", ".gif", NULL};
    const char *doc_ext[] = {".txt", ".pdf", ".docx", NULL};
    const char *video_ext[] = {".mp4", ".mkv", ".avi", NULL};

    create_directory("Images");
    create_directory("Documents");
    create_directory("Videos");

    while ((entry = readdir(dir)) != NULL) {
        if (entry->d_type == DT_REG) { // Only regular files
            const char *file_name = entry->d_name;
            const char *ext = strrchr(file_name, '.');

            if (ext) {
                int i = 0;
                while (image_ext[i]) {
                    if (strcmp(ext, image_ext[i]) == 0) {
                        move_file(file_name, "Images");
                        break;
                    }
                    i++;
                }

                i = 0;
                while (doc_ext[i]) {
                    if (strcmp(ext, doc_ext[i]) == 0) {
                        move_file(file_name, "Documents");
                        break;
                    }
                    i++;
                }

                i = 0;
                while (video_ext[i]) {
                    if (strcmp(ext, video_ext[i]) == 0) {
                        move_file(file_name, "Videos");
                        break;
                    }
                    i++;
                }
            }
        }
    }

    closedir(dir);
}

int main() {
    organize_files(".");
    printf("Files organized successfully.\n");
    return 0;
}
```

Save the file and exit the text editor (Ctrl+X, then Y, and Enter for nano).

### Step 4: Compile the Program

Compile the C program using GCC:

```sh
gcc file_organizer.c -o file_organizer
```

### Step 5: Run the Program

Execute the compiled program to organize files in the current directory:

```sh
./file_organizer
```

This will organize the files in the current directory into Images, Documents, and Videos folders.
