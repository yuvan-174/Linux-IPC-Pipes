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
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/wait.h>

void server(int rfd, int wfd);
void client(int wfd, int rfd);

int main() {
    int p1[2], p2[2], pid, *waits;
    
    pipe(p1); 
    pipe(p2); 

    pid = fork();
    if (pid == 0) { 
        close(p1[1]); 
        close(p2[0]); 
        server(p1[0], p2[1]); 
        return 0;
    }

    close(p1[0]); 
    close(p2[1]); 
    client(p1[1], p2[0]); 
    wait(waits); 
    return 0;
}


void server(int rfd, int wfd) {
    int n;
    char fname[2000]; 
    char buff[2000];  

 
    n = read(rfd, fname, 2000);
    fname[n] = '\0'; 

    int fd = open(fname, O_RDONLY);
    sleep(10); 

    if (fd < 0) {
        write(wfd, "can't open", 10);
    } else {

        n = read(fd, buff, 2000);
        write(wfd, buff, n); 
        close(fd); 
    }
}
void client(int wfd, int rfd) {
    int n;
    char fname[2000]; // Buffer for file name
    char buff[2000];  // Buffer for received content

    printf("ENTER THE FILE NAME: ");
    scanf("%s", fname); // Read file name from user
    printf("CLIENT SENDING THE REQUEST .... PLEASE WAIT\n");
    sleep(10); // Simulate delay for demonstration

    // Send the file name to the server
    write(wfd, fname, 2000);
    
    // Read the content returned by the server
    n = read(rfd, buff, 2000);
    buff[n] = '\0'; // Null terminate the received content

    // Display the results received from the server
    printf("THE RESULTS OF CLIENTS ARE ...... \n");
    write(1, buff, n); // Write content to standard output
}

```
## hello.txt
```
hello world
to check pipe
```




## OUTPUT
![image](https://github.com/user-attachments/assets/1c1d6366-d713-4939-baa0-41f9f0529293)


## C Program that illustrate communication between two process using named pipes using Linux API system calls
```c
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
int main(){
int res = mkfifo("/tmp/my_fifo", 0777);
if (res == 0)
printf("FIFO created\n");
exit(EXIT_SUCCESS);
}

```




## OUTPUT
![image](https://github.com/user-attachments/assets/501c6b88-a2d7-4f92-892c-adedd05665f4)


# RESULT:
The program is executed successfully.
