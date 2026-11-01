# Linux-IPC--Pipes
Linux-IPC-Pipes


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls


```c

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main()
{
    int fd[2];
    char message[] = "Hello from Parent";
    char buffer[100];

    pipe(fd);

    if (fork() == 0)
    {
        close(fd[1]);
        read(fd[0], buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
        close(fd[0]);
    }
    else
    {
        close(fd[0]);
        write(fd[1], message, sizeof(message));
        close(fd[1]);
        wait(NULL);
    }

    return 0;
}

```



## OUTPUT
<img width="733" height="85" alt="image" src="https://github.com/user-attachments/assets/badca502-d249-4f82-9481-4b96cc8bc6e0" />


## C Program that illustrate communication between two process using named pipes using Linux API system calls

```c

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <fcntl.h>

int main()
{
    int fd;
    char message[] = "Hello through Named Pipe";
    char buffer[100];

    mkfifo("myfifo", 0666);

    if (fork() == 0)
    {
        fd = open("myfifo", O_RDONLY);
        read(fd, buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
        close(fd);
    }
    else
    {
        fd = open("myfifo", O_WRONLY);
        write(fd, message, sizeof(message));
        close(fd);
        wait(NULL);
        unlink("myfifo");
    }

    return 0;
}

```



## OUTPUT
<img width="717" height="120" alt="image" src="https://github.com/user-attachments/assets/baa4ac50-9b3e-4025-8ce4-2df796f60c9a" />


# RESULT:
The program is executed successfully.
