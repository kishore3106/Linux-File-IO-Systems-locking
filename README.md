# Linux-File-IO-Systems-locking
Ex07-Linux File-IO Systems-locking
# AIM:
To Write a C program that illustrates files copying and locking

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux IO Systems locking

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## 1.To Write a C program that illustrates files copying 
```
char block[1024];
int in, out;
ssize_t nread;

// Open source file
in = open(argv[1], O_RDONLY);
if (in == -1) {
    perror("Error opening source file");
    exit(EXIT_FAILURE);
}

// Open destination file
out = open(argv[2], O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR);
if (out == -1) {
    perror("Error opening destination file");
    close(in);
    exit(EXIT_FAILURE);
}

// Copy contents
while ((nread = read(in, block, sizeof(block))) > 0) {
    if (write(out, block, nread) != nread) {
        perror("Error writing to destination file");
        close(in);
        close(out);
        exit(EXIT_FAILURE);
    }
}

if (nread == -1) {
    perror("Error reading source file");
}

close(in);
close(out);
return EXIT_SUCCESS;
```
##OUTPUT
![image](https://github.com/user-attachments/assets/e4e3f124-c7df-4e04-84bc-bf86169a4b77)
![image](https://github.com/user-attachments/assets/6aae4744-2025-4428-a070-56c7bd423656)
![image](https://github.com/user-attachments/assets/f43a10de-f882-4e39-a827-da271d982e87)

## 2.To Write a C program that illustrates files locking
```
char *file = argv[1];
int fd;

printf("Opening %s\n", file);

fd = open(file, O_WRONLY);
if (fd == -1) {
    perror("Error opening file");
    exit(EXIT_FAILURE);
}

// Acquire shared lock
if (flock(fd, LOCK_SH) == -1) {
    perror("Error acquiring shared lock");
    close(fd);
    exit(EXIT_FAILURE);
}
printf("Acquired shared lock using flock\n");
display_lslocks();

sleep(1); // Simulate waiting before upgrading

// Try to upgrade to exclusive lock (non-blocking)
if (flock(fd, LOCK_EX | LOCK_NB) == -1) {
    perror("Error upgrading to exclusive lock");
    flock(fd, LOCK_UN); // Release shared lock if upgrade fails
    close(fd);
    exit(EXIT_FAILURE);
}
printf("Acquired exclusive lock using flock\n");
display_lslocks();

sleep(1); // Simulate waiting before unlocking

// Release lock
if (flock(fd, LOCK_UN) == -1) {
    perror("Error unlocking");
    close(fd);
    exit(EXIT_FAILURE);
}
printf("Unlocked\n");
display_lslocks();

close(fd);
return 0;
```
## OUTPUT
![image](https://github.com/user-attachments/assets/e1e282cb-71f1-45bf-949a-8c9e2f1e5f40)
![image](https://github.com/user-attachments/assets/b5524a34-13b8-4ed6-996e-4111eb2901dc)
![image](https://github.com/user-attachments/assets/8ea81927-a2d8-46c2-b9aa-633498c68899)
![image](https://github.com/user-attachments/assets/bd0a01a1-cd7b-4096-be08-d751a616448a)

# RESULT:
The programs are executed successfully.
