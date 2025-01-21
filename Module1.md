# Module 1 

### Q1. Create a file and add executable permission to all users (user, group and others).

To create a file, we will be using the command `touch` 
```
$ touch file1
```
File permissions of the created file can be viewed using `ls -l` command
```
$ ls -l
```
File permissions can be changed using the text method of `chmod` command
```
$ chmod a=x file1
```
The changed permissions can be viewed using the `ls -l` command

![Screenshot (689)](https://github.com/user-attachments/assets/c76e8154-d85b-47b7-bd66-b0bb465c2855)

### Q2. Create a file and remove write permission for group user alone.

To create a file, we will be using the command `touch` 
```
$ touch file2
```
File permissions of the created file can be viewed using `ls -l` command
```
$ ls -l
```
Using the text method of `chmod` command, first read and write permissions are granted to all groups for file2 later write permission is removed for group users alone.
```
$ chmod a=wr file2
$ chmod g-w file2
```
The changed permissions can be viewed using the `ls -l` command

![Screenshot (690)](https://github.com/user-attachments/assets/55c1c61f-42bf-4893-8aae-0e435c79923b)

### Q3. Create a file and add a softlink to the file in different directory (Eg : Create a file in dir1/dir2/file and create a softlink for file inside dir1)

Directory according to the given structure is created using combination of `mkdir` and `cd` commands
Softlink of the file in dir1/dir2 is given using `ln -s` command
```
$ ln -s ~/linux_training/module1/dir/dir2/softlink/linux training/module1/dirl/sample
```
To verify the softlink, `ls -l` command is used
```
$ ls -l ~/linux_training/module1/dir/dir2/softlink/linux training/module1/dirl/sample
```

![Screenshot (692)](https://github.com/user-attachments/assets/a0ea2048-194e-451f-a364-fafbe2e10daa)

### Q4. Use ps command with options to display all active process running on the system
To view all the active processes that are running on the system, the `ps` command can be used
```
$ ps
$ ps -e (Lists currently running processes)
```

![Screenshot (691)](https://github.com/user-attachments/assets/1cd02658-18e9-426b-9b75-5820a7349e44)

### Q5. Create 3 files in a dir1 and re-direct the output of list command with sorted by timestamp of the files to a file.
The `ls` command lists existing files in directory _dir1_ and using the __pipe(|) mechanism__, the files listed can be sorted
```
$ ls | sort
```
The contents of this command can be stored in a file using the __redirection(>) mechanism__ and the contents of the file can be viewed using `cat` command.
```
$ ls | sort >  sample1
$ cat sample1
```

![Screenshot (693)](https://github.com/user-attachments/assets/a93835a0-af0a-4e13-95fd-681cb644ccab)
