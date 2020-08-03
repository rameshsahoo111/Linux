## What is umask?
As per https://linux.die.net/man/2/umask :
 
The umask is used by open(2), mkdir(2), and other system calls that create files to modify the permissions placed on newly created files or directories. Specifically, permissions in the umask are turned off from the mode argument to open(2) and mkdir(2).
  
The umask command defines the default permissions for newly created files based on the base permissions set defined for files and directories.
 
- Files have a base permissions set of 666, or full read and write access for all users. Execute permissions are not assigned by default because most files are not made to be executed (assigning executable permissions also opens up some security concerns).
 
- Directories have a base permissions set of 777, or read, write, and execute permissions for all users.
 
Umask operates by applying a subtractive **mask** to the base permissions.
 

### umask Example 1  - 002

If we want the owner and members of the owner group to be able to write to the newly created directories  or files,  others would have read & executable access to the directory and only read access to the file, the 002 umask is applicable.
 
#### What is the meaning of **002**?
- In case of file,  0 ==  7 (7 - 0 = 7 )
- In case of dir,  0 == 7 (7 - 0 = 7 )
- In case of file,  2 == 4 (6 - 2 = 4 )
- In case of directory,  2 == 5 (7 - 2 = 5 )
  
Effective permission for a file with 002 :

```bash
  666
- 002
----------
  664
```
   
Effective permission for a directory with 002 :

```bash
  777
- 002
----------
  775
```

```bash
[test@rhel7-c1 test]$ umask 002
[test@rhel7-c1 test]$ umask
0002
 
[test@rhel7-c1 test]$ touch file1
[test@rhel7-c1 test]$ mkdir dir1
 
[test@rhel7-c1 test]$ ls -l
total 0
drwxrwxr-x. 2 test test 6 Aug  3 19:31 dir1
-rw-rw-r--. 1 test test 0 Aug  3 19:31 file1
```
 
### Example 2  - 000


If we want the owner, group and other have full read/write access to files or directories 000 umask is applicable.

```bash
[test@rhel7-c1 test]$ umask 000
[test@rhel7-c1 test]$ umask
0000
 
[test@rhel7-c1 test]$ touch file1
[test@rhel7-c1 test]$ mkdir dir1
 
[test@rhel7-c1 test]$ ls -l
total 0
drwxrwxrwx. 2 test test 6 Aug  3 19:57 dir1
-rw-rw-rw-. 1 test test 0 Aug  3 19:57 file1
```

### Example 3  - 007

If we want the owner, group have full read/write access and other have no access to files or directories 007 umask is applicable.
 
```bash
[test@rhel7-c1 test]$ umask 007
[test@rhel7-c1 test]$ umask
0007
 
[test@rhel7-c1 test]$ touch file1
[test@rhel7-c1 test]$ mkdir dir1
 
[test@rhel7-c1 test]$ ls -l
total 0
drwxrwx---. 2 test test 6 Aug  3 19:58 dir1
-rw-rw----. 1 test test 0 Aug  3 19:58 file1
```
