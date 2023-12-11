<html>
<title>deadpool</title>
<body>

 <br>Write a C program to send SIGALRM signal by child process to parent process and parent <br>
process make a provision to catch the signal and display alarm is fired.(Use Kill, fork, signal <br>
and sleep system call)  <br>
ANS
#include <stdio.h> <br>
#include <stdlib.h> <br>
#include <signal.h> <br>
#include <sys/types.h> <br>
#include <unistd.h> <br>

void alarmHandler(int signum) { <br>
    printf("Alarm is fired!\n"); <br>
    exit(0); <br>
} <br>

int main() { <br>
    pid_t childPid; <br>

    
    signal(SIGALRM, alarmHandler); <br>

   
    if ((childPid = fork()) == -1) { <br>
        perror("fork"); <br>
        exit(EXIT_FAILURE); <br>
    } <br>

    if (childPid == 0) {  <br>
        sleep(3); <br>
        printf("Child process sending SIGALRM to parent (PID: %d)\n", getppid()); <br>
        kill(getppid(), SIGALRM); // Send SIGALRM signal to the parent <br>
        exit(0); <br>
    } else { // Parent process <br>
        printf("Parent process waiting for SIGALRM...\n"); <br>
        // Sleep indefinitely, waiting for the SIGALRM signal <br>
        while (1) { <br>
            sleep(1); <br>
        } <br>
    } <br>

    return 0; <br>
} <br>
 Take multiple files as Command Line Arguments and print their inode numbers and file types <br>
 #include <stdlib.h> <br>
#include <stdio.h> <br>
#include <string.h> <br>

int main(int argc, char *argv[]) { <br>
    char command[50]; <br>

    if (argc == 2) { <br>
        memset(command, 0, sizeof(command)); <br>
        strcat(command, "ls -i "); <br>
        strcat(command, argv[1]); <br>

        int result = system(command); <br>

        if (result == -1) { <br>
            perror("system error"); <br>
            return EXIT_FAILURE; <br>
        } else if (result == 127) { <br>
            fprintf(stderr, "Command execution failed\n"); <br>
            return EXIT_FAILURE; <br>
        } <br>
    } else { <br>
        printf("Invalid number of inputs\n"); <br>
        return EXIT_FAILURE; <br>
    } <br>

    return EXIT_SUCCESS; <br>
} <br>
 Take multiple files as Command Line Arguments and print their inode numbers and file types  <br>
#include <stdlib.h> <br>
#include <stdio.h> <br>
#include <string.h> <br>

int main(int argc, char *argv[]) { <br>
    char command[50]; <br>

    if (argc == 2) { <br>
        memset(command, 0, sizeof(command)); <br>
        strcat(command, "ls -i "); <br>
        strcat(command, argv[1]); <br>

        int result = system(command); <br>

        if (result == -1) { <br>
            perror("system error"); <br>
            return EXIT_FAILURE; <br>
        } else if (result == 127) { <br> 
            fprintf(stderr, "Command execution failed\n"); <br>
            return EXIT_FAILURE; <br>
        }
    } else { <br>
        printf("Invalid number of inputs\n"); <br>
        return EXIT_FAILURE; <br>
    }

    return EXIT_SUCCESS; <br>
} <br>
 Write a C program to find file properties such as inode number, number of hard link, File <br>
 permissions, File size, File access and modification time and so on of a given file using stat() <br>
 system call <br>
ans: <br>
#include <stdio.h> <br>
#include <unistd.h> <br>
#include <sys/stat.h> <br>
#include <sys/types.h> <br>
#include <time.h> <br>
#include <stdlib.h> <br>

int main(int argc, char *argv[]) { <br>
    struct stat info; <br>

    if (argc != 2) { <br>
        fprintf(stderr, "Usage: %s <filename>\n", argv[0]); <br>
        return 1; <br>
    } <br>

    if (stat(argv[1], &info) == -1) { <br>
        perror("stat error"); <br>
        exit(EXIT_FAILURE); <br>
    } <br>

    printf("Inode number: %lu\n", (unsigned long)info.st_ino); <br>
    printf("Size: %lld bytes\n", (long long)info.st_size); <br>
    printf("Last file access: %s", ctime(&info.st_atime)); <br>
    printf("Notification time: %s", ctime(&info.st_mtime)); <br>
    printf("No of Hardlink: %lu\n", (unsigned long)info.st_nlink); <br>

    printf("File Permissions: \n"); <br>
    printf((info.st_mode & S_IRUSR) ? "r" : "-"); <br>
    printf((info.st_mode & S_IWUSR) ? "w" : "-"); <br>
    printf((info.st_mode & S_IXUSR) ? "x" : "-"); <br>
    printf("\n"); <br>

    return 0; <br>
}
 Write a C program that catches the ctrl-c (SIGINT) signal for the first time and display the <br>
appropriate message and exits on pressing ctrl-c again. <br>
ANS <br>
#include <stdio.h> <br>
#include <unistd.h> <br>
#include <stdlib.h> <br>
#include <signal.h> <br>

void sigfun(int sig) { <br>
    printf("You have pressed Ctrl-C, please press again to exit\n"); <br>
    (void) signal(SIGINT, SIG_DFL); <br>
} <br>

int main() { <br>
    (void) signal(SIGINT, sigfun); <br>

    while (1) { <br>
        printf("Hello World!\n"); <br>
        sleep(1); <br>
    } <br>

    return 0; <br>
} <br>
 Print the type of file and inode number where file name accepted through Command Line <br>
ANS <br>
#include <stdio.h> <br>
#include <stdlib.h> <br>
#include <string.h> <br>

int main(int argc, char *argv[]) { <br>
    char command[50]; <br>

    if (argc == 2) { <br>
        memset(command, 0, sizeof(command)); <br>
        snprintf(command, sizeof(command), "ls -i %s", argv[1]);
        <br>
        int result = system(command); <br>

        if (result == -1) { <br>
            perror("system error"); <br>
            return EXIT_FAILURE; <br>
        } else if (result == 127) { <br>
            fprintf(stderr, "Command execution failed\n"); <br>
            return EXIT_FAILURE; <br>
        } <br>
    } else { <br>
        fprintf(stderr, "Invalid number of inputs\n"); <br>
        return EXIT_FAILURE; <br>
    }
    <br>
    return EXIT_SUCCESS; <br>
} <br>
 Write a C program which creates a child process to run linux/ unix command or any user defined <br>
program. The parent process set the signal handler for death of child signal and Alarm signal. If a child <br>
process does not complete its execution in 5 second then parent process kills child process. <br>
ANS <br>
#include <signal.h> <br>
#include <stdio.h> <br>
#include <stdlib.h> <br>
#include <sys/types.h> <br>
#include <unistd.h> <br>


void sighup(); <br>
void sigint(); <br>
void sigquit();
<br>

int main() <br>
{ <br>
    int pid; <br>

   
    if ((pid = fork()) < 0)<br>
    {
        perror("fork");<br>
        exit(1);<br>
    }<br>

    if (pid == 0)<br>
    { /* child */<br>
        signal(SIGHUP, sighup);<br>
        signal(SIGINT, sigint);<br>
        signal(SIGQUIT, sigquit);<br>
        for (;;)<br>
            ;<br>
    }<br>
    else <br>
    { <br>
        printf("\nPARENT: sending SIGHUP\n\n");<br>
        kill(pid, SIGHUP);<br>
        sleep(3); /* pause for 3 secs */<br>
        printf("\nPARENT: sending SIGINT\n\n");<br>
        kill(pid, SIGINT);<br>
        sleep(3); /* pause for 3 secs */<br>
        printf("\nPARENT: sending SIGQUIT\n\n");<br>
        kill(pid, SIGQUIT);<br>
        sleep(3);<br>
    }<br>

    return 0;<br>
}<br>

// sighup() function definition<br>
void sighup()<br>
{
    signal(SIGHUP, sighup); /* reset signal */<br>
    printf("CHILD: I have received a SIGHUP\n");<br>
}
<br>
// sigint() function definition<br>
void sigint()<br>
{
    signal(SIGINT, sigint); /* reset signal */<br>
    printf("CHILD: I have received a SIGINT\n");<br>
}<br>

// sigquit() function definition<br>
void sigquit()<br>
{
    printf("My DADDY has Killed me!!!\n");<br>
    exit(0);<br>
}
 Write a C program to find whether a given files passed through command line arguments are present in<br>
current directory or not<br>
ANS<br>
#include<stdio.h><br>
#include<unistd.h><br>
int main(int argc,char *argv[])<br>
{<br>
if(access(argv[1],F_OK)==0)<br>
printf("File %s exists.",argv[1]);<br>
else<br>
printf("File not exists.");<br>
return 0;<br>
}<br>
 Write a C program which creates a child process and child process catches a signal SIGHUP, SIGINT<br>
and SIGQUIT. The Parent process send a SIGHUP or SIGINT signal after every 3 seconds, at the end<br>
of 15 second parent send SIGQUIT signal to child and child terminates by displaying message "My<br>
Papa has Killed me!!!”. <br>
ANS<br>
// C program to implement sighup(), sigint()<br>
// and sigquit() signal functions<br>
#include <signal.h><br>
#include <stdio.h><br>
#include <stdlib.h><br>
#include <sys/types.h><br>
#include <unistd.h><br>
// function declaration<br>
void sighup();<br>
void sigint();<br>
void sigquit();<br>
// driver code<br>
void main()<br>
{<br>
int pid;<br>
/* get child process */<br>
if ((pid = fork()) < 0) {<br>
perror("fork");<br>
exit(1);<br>
}<br>
if (pid == 0) { /* child */<br>
signal(SIGHUP, sighup);<br>
signal(SIGINT, sigint);<br>
signal(SIGQUIT, sigquit);<br>
for (;;)<br>
; /* loop for ever */<br>
}<br>
else /* parent */<br>
{ /* pid hold id of child */<br>
printf("\nPARENT: sending SIGHUP\n\n");<br>
kill(pid, SIGHUP);<br>
sleep(3); /* pause for 3 secs */<br>
printf("\nPARENT: sending SIGINT\n\n");<br>
kill(pid, SIGINT);<br>
sleep(3); /* pause for 3 secs */<br>
printf("\nPARENT: sending SIGQUIT\n\n");<br>
kill(pid, SIGQUIT);<br>
sleep(3);<br>
}<br>
}<br>
// sighup() function definition<br>
void sighup()<br>
{<br>
signal(SIGHUP, sighup); /* reset signal */<br>
printf("CHILD: I have received a SIGHUP\n");<br>
}<br>
// sigint() function definition<br>
void sigint()<br>
{
signal(SIGINT, sigint); /* reset signal */<br>
printf("CHILD: I have received a SIGINT\n");<br>
}
// sigquit() function definition<br>
void sigquit()<br>
{<br>
printf("My DADDY has Killed me!!!\n");<br>
exit(0);<br>
}<br>
 Write a C program to create an unnamed pipe. The child process will write following three messages to<br>
pipe and parent process display it. Message1 = “Hello World” Message2 = “Hello SPPU” Message3 =<br>
“Linux is Funny” <br>
ans
#include<stdio.h><br>
#include<unistd.h><br>
int main() {<br>
int pipefds[2];<br>
int returnstatus;<br>
char writemessages[3][20]={"Hello World", "Hello SPPU","Linux is Funny"};<br>
char readmessage[20];<br>
returnstatus = pipe(pipefds);<br>
if (returnstatus == -1) {<br>
printf("Unable to create pipe\n");<br>
return 1;<br>
}<br>
int child = fork();<br>
if(child==0){<br>
printf("Child is Writing to pipe - Message 1 is %s\n", writemessages[0]);<br>
write(pipefds[1], writemessages[0], sizeof(writemessages[0]));<br>
printf("Child is Writing to pipe - Message 2 is %s\n", writemessages[1]);<br>
write(pipefds[1], writemessages[1], sizeof(writemessages[1]));<br>
printf("Child is Writing to pipe - Message 3 is %s\n", writemessages[2]);<br>
write(pipefds[1], writemessages[2], sizeof(writemessages[2]));<br>
}<br>
else<br>
{<br>
read(pipefds[0], readmessage, sizeof(readmessage));<br>
printf("Parent Process is Reading from pipe – Message 1 is %s\n",<br>
readmessage);<br>
read(pipefds[0], readmessage, sizeof(readmessage));<br>
printf("Parent Process is Reading from pipe – Message 2 is %s\n",<br>
readmessage);<br>
read(pipefds[0], readmessage, sizeof(readmessage));<br>
printf("Parent Process is Reading from pipe – Message 3 is %s\n",<br>
readmessage);<br>
}<br>
return 0;<br>
}<br>
 Display all the files from current directory which are created in particular month <br>
ans<br>
#include <stdio.h><br>
#include <dirent.h><br>
#include <string.h><br>
#include <sys/stat.h><br>
#include <time.h><br>
#include <stdlib.h><br>

int main(int argc, char *argv[]) {<br>
    if (argc != 2) {<br>
        fprintf(stderr, "Usage: %s <month>\n", argv[0]);<br>
        exit(EXIT_FAILURE);<br>
    }<br>

    char mon[100];<br>
    DIR *dp;<br>
    struct dirent *ep;<br>
    struct stat sb;<br>

    dp = opendir("./");<br>

    if (dp != NULL) {<br>
        while ((ep = readdir(dp)) != NULL) {<br>
            if (stat(ep->d_name, &sb) == -1) {<br>
                perror("stat");<br>
                exit(EXIT_SUCCESS);<br>
            }<br>

            if (S_ISREG(sb.st_mode)) { // Check if it's a regular file<br>
                strcpy(mon, ctime(&sb.st_ctime));<br>
                char *ch = strtok(mon, " ");<br>
                ch = strtok(NULL, ",");<br>
                char *ch1 = strtok(ch, " ");<br>

                if ((strcmp(ch1, argv[1])) == 0) {<br>
                    char path[512];<br>
                    snprintf(path, sizeof(path), "./%s", ep->d_name);<br>
                    printf("%s\t\t%s", path, ctime(&sb.st_ctime));<br>
                }<br>
            }<br>
        }<br>
        (void)closedir(dp);<br>
    }<br>

    return 0;<br>
}<br>
Write a C program to create n child processes. When all n child processes terminates, Display total<br>
cumulative time children spent in user and kernel mode<br>
ans<br>
#include<sys/types.h><br>
#include<sys/wait.h><br>
#include<unistd.h><br>
#include<time.h><br>
#include<sys/times.h><br>
#include<stdio.h><br>
#include<stdlib.h><br>
int main(void)<br>
{<br>
int i, status;<br>
pid_t pid;<br>
time_t currentTime;<br>
struct tms cpuTime;<br>
if((pid = fork())==-1) //start child process<br>
{<br>
perror("\nfork error");<br>
exit(EXIT_FAILURE);<br>
}<br>
else if(pid==0) //child process<br>
{<br>
time(&currentTime);<br>
printf("\nChild process started at %s",ctime(&currentTime));<br>
for(i=0;i<5;i++)<br>
{<br>
printf("\nCounting= %dn",i); //count for 5 seconds<br>
sleep(1);<br>
}<br>
time(&currentTime);<br>
printf("\nChild process ended at %s",ctime(&currentTime));<br>
exit(EXIT_SUCCESS);<br>
}<br>
else<br>
{ //Parent process<br>
time(&currentTime); // gives normal time<br>
printf("\nParent process started at %s ",ctime(&currentTime));<br>
if(wait(&status)== -1) //wait for child process<br>
perror("\n wait error");<br>
if(WIFEXITED(status))<br>
printf("\nChild process ended normally");<br>
else<br>
printf("\nChild process did not end normally");<br>
if(times(&cpuTime)<0)<br>
perror("\nTimes error");<br>
else<br>
{ // _SC_CLK_TCK: system configuration time: seconds clock tick<br>
printf("\nParent process user time= %fn",((double)<br>
cpuTime.tms_utime));<br>
printf("\nParent process system time = %fn",((double)<br>
cpuTime.tms_stime));<br>
printf("\nChild process user time = %fn",((double)<br>
cpuTime.tms_cutime));<br>
printf("\nChild process system time = %fn",((double)<br>
cpuTime.tms_cstime));<br>
}<br>
time(&currentTime);
printf("\nParent process ended at %s",ctime(&currentTime));<br>
exit(EXIT_SUCCESS);<br>
}<br>
}<br>
 Write a C Program that demonstrates redirection of standard output to a file <br>
ans<br>
#include <stdio.h><br>
#include <stdlib.h><br>
#include <string.h><br>

int main(int argc, char *argv[]) {<br>
    if (argc == 2) {<br>
        char command[50];<br>
        memset(command, 0, sizeof(command));<br>

       
        snprintf(command, sizeof(command), "ls > %s", argv[1]);<br>

       
        system(command);<br>
    } else {<br>
        fprintf(stderr, "\nInvalid number of inputs\n");<br>
        return EXIT_FAILURE;<br>
    }<br>

    return EXIT_SUCCESS;<br>
}<br>
Implement the following unix/linux command (use fork, pipe and exec system call) ls –l | wc –l<br>
// C code to implement ls | wc command <br>
ans<br>
#include <stdio.h><br>
#include <stdlib.h><br>
#include <fcntl.h><br>
#include<errno.h><br>
#include<sys/wait.h><br>
#include <unistd.h><br>
int main(){<br>
 
 
 int a[2];<br>

 
 pipe(a);<br>

 if(!fork())<br>
 {<br>
 
 close(1);<br>

 // making stdout same as a[1]<br>
 dup(a[1]);<br>
 <br>
 // closing reading part of pipe<br>
 // we don't need of it at this time <br>
 close(a[0]);<br>

 // executing ls<br>
 execlp("ls","ls",NULL);<br>
 }<br>
 else<br>
 {<br>
 // closing normal stdin<br>
 close(0);<br>

 // making stdin same as a[0]<br>
 dup(a[0]);<br>

 
 close(a[1]);<br>


 execlp("wc","wc",NULL); <br>
 }<br>
}<br>
 Write a C program that redirects standard output to a file output.txt. (use of dup and open system call). <br>
ans<br>
#include <stdio.h><br>
#include <stdlib.h><br>
#include <unistd.h><br>
#include <fcntl.h><br>
int main(void) {<br>
int number1, number2, sum;<br>
int input_fds = open("./input.txt", O_RDONLY);<br>
if(dup2(input_fds, STDIN_FILENO) < 0) {<br>
printf("Unable to duplicate file descriptor.");<br>
exit(EXIT_FAILURE);<br>
}<br>
scanf("%d %d", &number1, &number2);<br>
sum = number1 + number2;<br>
printf("%d + %d = %d\n", number1, number2, sum);<br>
return EXIT_SUCCESS;<br>
}<br>
Generate parent process to write unnamed pipe and will read from it<br>
ans<br>
#include <stdio.h><br>
#include <stdlib.h><br>
#include <unistd.h><br>
#include <string.h><br>
    <br>
int main() {
    int pipe_fd[2]; // File descriptors for the pipe<br>

    // Create the pipe<br>
    if (pipe(pipe_fd) == -1) {<br>
        perror("Pipe creation failed");<br>
        exit(EXIT_FAILURE);<br>
    }<br>

    pid_t child_pid = fork(); // Fork a child process<br>

    if (child_pid == -1) {<br>
        perror("Fork failed");<br>
        exit(EXIT_FAILURE);<br>
    }<br>

    if (child_pid == 0) {<br>
        // Child process<br>
        close(pipe_fd[1]); <br>
        char buffer[100];<br>
        ssize_t bytes_read = read(pipe_fd[0], buffer, sizeof(buffer));<br>
        close(pipe_fd[0]); <br>

        if (bytes_read == -1) {<br>
            perror("Read from pipe failed");<br>
            exit(EXIT_FAILURE);<br>
        }<br>

        printf("Child received: %.*s\n", (int)bytes_read, buffer);<br>
    } else {<br>
        // Parent process<br>
        close(pipe_fd[0]);<br><br>
        const char *message = "Hello from parent!";<br>
        ssize_t bytes_written = write(pipe_fd[1], message, strlen(message));<br>
        close(pipe_fd[1]); // Close the write end of the pipe in the parent process<br>

        if (bytes_written == -1) {<br>
            perror("Write to pipe failed");<br>
            exit(EXIT_FAILURE);<br>
        }<br>

        printf("Parent sent: %s\n", message);<br>
    }<br>

    return 0;<br>
}
 Write a C program to Identify the type (Directory, character device, Block device, Regular file, FIFO or
pipe, symbolic link or socket) of given file using stat() system call.<br>
ans
#include <stdio.h><br>
#include <sys/types.h><br>
#include <sys/stat.h><br>
#include <unistd.h><br>
void identifyFileType(const char *path) {<br>
 struct stat fileStat;<br>
 // Use stat() to get information about the file<br>
 if (stat(path, &fileStat) == -1) {<br>
 perror("Error in stat");<br>
 return;<br>
 }<br>
 // Check the file type using st_mode field in the stat structure<br>
 if (S_ISREG(fileStat.st_mode)) {<br>
 printf("%s is a regular file.\n", path);<br>
 } else if (S_ISDIR(fileStat.st_mode)) {<br>
 printf("%s is a directory.\n", path);<br>
 } else if (S_ISCHR(fileStat.st_mode)) {<br>
 printf("%s is a character device.\n", path);<br>
 } else if (S_ISBLK(fileStat.st_mode)) {<br>
 printf("%s is a block device.\n", path);<br>
 } else if (S_ISFIFO(fileStat.st_mode)) {<br>
 printf("%s is a FIFO or pipe.\n", path);<br>
 } else if (S_ISLNK(fileStat.st_mode)) {<br>
 printf("%s is a symbolic link.\n", path);<br>
 } else if (S_ISSOCK(fileStat.st_mode)) {<br>
 printf("%s is a socket.\n", path);<br>
 } else {<br>
 printf("%s is of unknown type.\n", path);<br>
 }<br>
}
int main(int argc, char *argv[]) {<br>
 if (argc != 2) {<br>
 fprintf(stderr, "Usage: %s <filename>\n", argv[0]);<br>
 return 1;<br>
 }<br>
 const char *filename = argv[1];<br>
 identifyFileType(filename);<br>
 return 0;<br>
}<br>
 Generate parent process to write unnamed pipe and will write into it. Also generate child process which<br>
will read from pipe <br>
ans<br>
#include <stdio.h><br>
#include <stdlib.h><br>
#include <unistd.h><br>
#include <string.h><br>

int main() {<br>
    int pipe_fd[2]; // File descriptors for the pipe<br>

    // Create the pipe<br>
    if (pipe(pipe_fd) == -1) {<br>
        perror("Pipe creation failed");<br>
        exit(EXIT_FAILURE);<br>
    }<br>
    <br>
    pid_t child_pid = fork(); // Fork a child process<br>

    if (child_pid == -1) {<br>
        perror("Fork failed");<br>
        exit(EXIT_FAILURE);<br>
    }<br>

    if (child_pid == 0) {<br>
        // Child process<br>
        close(pipe_fd[1]); // Close the write end of the pipe in the child process<br>
        char buffer[100];<br>
        ssize_t bytes_read = read(pipe_fd[0], buffer, sizeof(buffer));<br>
        close(pipe_fd[0]); // Close the read end of the pipe in the child process<br>

        if (bytes_read == -1) {<br>
            perror("Read from pipe failed");<br>
            exit(EXIT_FAILURE);<br>
        }<br>

        printf("Child received: %.*s\n", (int)bytes_read, buffer);<br>
    } else {<br>
        // Parent process<br>
        close(pipe_fd[0]); // Close the read end of the pipe in the parent process<br>
        const char *message = "Hello from parent!";<br>
        ssize_t bytes_written = write(pipe_fd[1], message, strlen(message));<br>
        close(pipe_fd[1]); // Close the write end of the pipe in the parent process<br>

        if (bytes_written == -1) {<br>
            perror("Write to pipe failed");<br>
            exit(EXIT_FAILURE);<br>
        }<br>

        printf("Parent sent: %s\n", message);<br>
    }<br>

    return 0;<br>
}<br>
Write a C program to get and set the resource limits such as files, memory associated with a process<br>
ans<br>
#include <stdio.h><br>
#include <stdlib.h><br>
#include <sys/resource.h><br>
void printLimits() {<br>
 struct rlimit limits;<br>
 // Get the current resource limits for file descriptors<br>
 if (getrlimit(RLIMIT_NOFILE, &limits) == -1) {<br>
 perror("getrlimit for RLIMIT_NOFILE failed");<br>
 exit(EXIT_FAILURE);<br>
 }<br>
 printf("Current maximum file descriptors limit: %ld\n", (long)limits.rlim_cur);<br>
 // Get the current resource limits for stack size<br>
 if (getrlimit(RLIMIT_STACK, &limits) == -1) {<br>
 perror("getrlimit for RLIMIT_STACK failed");<br>
 exit(EXIT_FAILURE);<br>
 }<br>
 printf("Current maximum stack size limit: %ld bytes\n", (long)limits.rlim_cur);<br>
}<br>
void setLimits() {<br>
 struct rlimit new_limits;<br>
 // Set the new maximum file descriptors limit<br>
 new_limits.rlim_cur = 1024; // Set to the desired value<br>
 new_limits.rlim_max = 1024; // Set to the same value as rlim_cur for simplicity<br>
 if (setrlimit(RLIMIT_NOFILE, &new_limits) == -1) {<br>
 perror("setrlimit for RLIMIT_NOFILE failed");<br>
 exit(EXIT_FAILURE);<br>
 }<br>
 printf("New maximum file descriptors limit set: %ld\n", (long)new_limits.rlim_cur);<br>
 // Set the new maximum stack size limit<br>
 new_limits.rlim_cur = 1024 * 1024 * 2; // Set to the desired value (2 MB)<br>
 new_limits.rlim_max = 1024 * 1024 * 2; // Set to the same value as rlim_cur for <br>
 if (setrlimit(RLIMIT_STACK, &new_limits) == -1) {<br>
 perror("setrlimit for RLIMIT_STACK failed");<br>
 exit(EXIT_FAILURE);<br>
 }<br>
 printf("New maximum stack size limit set: %ld bytes\n", (long)new_limits.rlim_cur);<br>
}<br>
int main() {<br>
 printf("Before setting limits:\n");<br>
 printLimits();<br>
 printf("\nSetting new limits:\n");<br>
 setLimits();<br>
 printf("\nAfter setting limits:\n");<br>
 printLimits();<br>
 return 0;<br>
}<br>
Write a C program that redirects standard output to a file output.txt. (use of dup and open system call). <br>
ans<br>
#include <stdio.h><br>
#include <stdlib.h><br>
#include <fcntl.h><br><br>
#include <unistd.h><br>
int main() {<br>
 // Open the file for writing (create if it doesn't exist, truncate to zero if it does)<br>
 int fileDescriptor = open("output.txt", O_WRONLY | O_CREAT | O_TRUNC, 0666);<br>
 if (fileDescriptor == -1) {<br>
 perror("Error opening file");<br>
 exit(EXIT_FAILURE);<br>
 }<br>
 // Duplicate file descriptor to standard output (file descriptor 1)<br>
 if (dup2(fileDescriptor, STDOUT_FILENO) == -1) {<br>
 perror("Error duplicating file descriptor");<br>
 close(fileDescriptor);<br>
 exit(EXIT_FAILURE);<br>
 }<br>
 // Close the original file descriptor (since it's duplicated to standard output)<br>
 close(fileDescriptor);<br>
 // Now, anything written to standard output will go to "output.txt"<br>
 printf("This will be written to output.txt.\n");<br>
 // Restore standard output to the console (file descriptor 1)<br>
 if (dup2(STDOUT_FILENO, 1) == -1) {<br>
 perror("Error restoring standard output");<br>
 exit(EXIT_FAILURE);<br>
 }<br>
 // The following will be printed to the console, not "output.txt"<br>
 printf("This will be printed to the console.\n");<br>
 return 0;<br>
}<br>

(separate folder is made with output init)<br>
 Write a C program that print the exit status of a terminated child process<br>
ans<br>
#include <stdio.h><br><br>
#include <stdlib.h><br>
#include <unistd.h><br>
#include <sys/types.h><br>
#include <sys/wait.h><br>

int main(void) {<br>
    pid_t pid = fork();<br>

    if (pid == 0) {<br>
        /* Child process */<br>
        execl("/bin/echo", "echo", "Hello, child process!", NULL);<br>
        perror("execl");  // This line will be reached only if execl fails<br>
        exit(EXIT_FAILURE);<br>
    }<br>

    int status;<br>
    waitpid(pid, &status, 0);<br>

    if (WIFEXITED(status)) {<br>
        int exit_status = WEXITSTATUS(status);<br>
        printf("Exit status of the child was %d\n", exit_status);<br>
    } else {<br>
        printf("Child process did not terminate normally.\n");<br>
    }<br>

    return 0;<br>
}<br>
Write a C program which receives file names as command line arguments and display those filenames<br>
in ascending order according to their sizes. I) (e.g $ a.out a.txt b.txt c.txt, …)<br>
ans<br>
#include <stdio.h><br>
#include <stdlib.h><br>
#include <sys/stat.h><br>
// Structure to hold file information<br>
struct FileInfo {<br>
 char *name;<br>
 off_t size;<br>
};<br>
// Function to compare two FileInfo structures by size<br>
int compareFileInfo(const void *a, const void *b) {<br>
 return ((struct FileInfo *)a)->size - ((struct FileInfo *)b)->size;<br>
}<br>
int main(int argc, char *argv[]) {<br>
 // Check if at least one file name is provided<br>
 if (argc < 2) {<br>
 fprintf(stderr, "Usage: %s file1 file2 file3 ...\n", argv[0]);<br>
 return EXIT_FAILURE;<br>
 }<br>
 // Allocate an array of FileInfo structures<br>
 struct FileInfo *files = (struct FileInfo *)malloc((argc - 1) * sizeof(struct FileInfo));<br>
 // Populate the array with file names and sizes<br>
 for (int i = 1; i < argc; ++i) {<br>
 files[i - 1].name = argv[i];<br>
 struct stat fileStat;<br>
 if (stat(argv[i], &fileStat) == -1) {<br>
 perror("Error getting file size");<br>
 return EXIT_FAILURE;<br>
 }<br>
 files[i - 1].size = fileStat.st_size;<br>
 }<br>
 // Sort the array of FileInfo structures based on file size<br>
 qsort(files, argc - 1, sizeof(struct FileInfo), compareFileInfo);<br>
 // Display the sorted file names<br>
 printf("File names in ascending order based on size:\n");<br>
 for (int i = 0; i < argc - 1; ++i) {<br>
 printf("%s - %ld bytes\n", files[i].name, (long)files[i].size);<br>
 }<br>
 // Free the allocated memory<br>
 free(files);<br>
 return EXIT_SUCCESS;<br>
}<br>
. Write a C program that illustrates suspending and resuming processes using signals<br>
. Write a C program that illustrates suspending and resuming processes using signals <br>
ans<br>
#include <stdio.h><br>
#include <unistd.h><br>
#include <pthread.h><br>

void* child_function(void* arg) {<br>
    while (1) { // Loop forever.<br>
        printf("Child loop\n");<br>
        sleep(1);<br>
    }<br>
    return NULL; // Will never execute.<br>
}<br>

int main() {<br>
    pthread_t childThread;<br>

    // Create the child thread<br>
    if (pthread_create(&childThread, NULL, child_function, NULL) != 0) {<br>
        perror("Error creating child thread");<br>
        return 1;<br>
    }<br>

    sleep(4);<br>
    printf("Child suspended\n");<br>
    

    
    sleep(4);<br>
    printf("Child resumed\n");<br>

 
    
    sleep(4);<br>
    printf("Child terminated\n");<br>


    printf("Parent finished\n");<br>

    return 0;<br>
}<br><br>
 Write a C program that a string as an argument and return all the files that begins with that name in the<br>
current directory. For example > ./a.out foo will return all file names that begins with foo
ans<br>
#include<stdio.h><br>
#include<dirent.h><br>
int main(void)<br>
{<br>
 DIR *d;<br>
 struct dirent *dir;<br>
 d = opendir(".");<br>
 if (d)<br>
 {<br><br>
 while ((dir = readdir(d)) != NULL)<br>
 {<br>
 printf("%s\n", dir->d_name);<br>
 }<br>
 closedir(d);<br>
 }<br>
 return(0);<br>
}<br><br>
 Display all the files from current directory whose size is greater that n Bytes Where n is accept from<br>
user. <br>
ans<br>
use this command " $ find . -size 1M" if conde does not work<br>


#include <stdio.h><br>
#include <stdlib.h><br>
#include <dirent.h><br>
#include <sys/stat.h><br>
#include <string.h>  <br>

void listFilesWithSize(char *path, off_t sizeThreshold) {<br>
    DIR *dir;<br>
    struct dirent *entry;<br>
    struct stat fileStat;<br>

    
    dir = opendir(path);<br>
    if (dir == NULL) {<br>
        perror("Error opening directory");<br>
        exit(EXIT_FAILURE);<br>
    }<br>

    printf("Files larger than %ld Bytes in %s:\n", (long)sizeThreshold, path);<br>

  
    while ((entry = readdir(dir)) != NULL) {<br>
        // Ignore "." and ".."<br>
        if (strcmp(entry->d_name, ".") == 0 || strcmp(entry->d_name, "..") == 0) {<br>
            continue;<br>
        }<br>

      
        char filePath[256];<br>
        snprintf(filePath, sizeof(filePath), "%s/%s", path, entry->d_name);
        <br>
        // Get file information<br>
        if (stat(filePath, &fileStat) == -1) {<br>
            perror("Error getting file information");<br>
            exit(EXIT_FAILURE);<br>
        }<br>

        // Check if the file size is greater than the threshold<br>
        if (S_ISREG(fileStat.st_mode) && fileStat.st_size > sizeThreshold) {<br>
            printf("%s - %ld Bytes\n", entry->d_name, (long)fileStat.st_size);<br>
        }<br>
    }<br>

 
    closedir(dir);<br>
}<br>

int main() {<br>
    char path[256];<br>
    off_t sizeThreshold;<br>

  
    printf("Enter the directory path: ");<br>
    scanf("%s", path);<br>

    // Get the size threshold from the user<br>
    printf("Enter the size threshold in Bytes: ");<br>
    scanf("%ld", &sizeThreshold);<br>

   
    listFilesWithSize(path, sizeThreshold);<br>

    return 0;<br>
}<br><br>

Write a C program to find file properties such as inode number, number of hard link, File permissions,<br>
File size, File access and modification time and so on of a given file using stat() system call.<br>
ans<br>
#include<stdio.h><br>
#include<unistd.h><br>
#include<dirent.h><br>
#include<string.h><br>
#include<time.h><br>
#include<stdlib.h><br>
#include<sys/stat.h><br>
#include<sys/types.h><br>

int main(int argc, char* argv[]) {<br>
    struct stat info;<br>

    if (argc != 2) {<br>
        printf("Enter a filename: ");<br>
        scanf("%s", argv[1]);<br>
    }<br>

    if (stat(argv[1], &info) == -1) {<br>
        printf("stat error\n");<br>
        exit(0);<br>
    }<br>

    printf("inode number = %ld\n", (long)info.st_ino);<br>
    printf("size = %ld\n", (long)info.st_size);<br>
    printf("last file access = %s", ctime(&info.st_atime));<br>
    printf("modification time = %s", ctime(&info.st_mtime));<br>
    printf("No of Hardlink = %ld\n", (long)info.st_nlink);<br>

    printf("File Permissions : \n");<br>
    printf((info.st_mode & S_IRUSR) ? "r" : "-");<br>
    printf((info.st_mode & S_IWUSR) ? "w" : "-");<br>
    printf((info.st_mode & S_IXUSR) ? "x" : "-");<br>

    return 0;<br>
}<br>





</body>
</html>
