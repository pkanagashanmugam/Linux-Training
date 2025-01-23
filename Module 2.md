# Module 2 - The Linux Environment

### Q1. Use the appropriate command to list all files larger than 1 MB in the current directory and save the output to a file.
To view all the files in the current directory along with their file size, the `ls -lh` command is used which displays the list of files along with file metadata in human readable form
In order to view the files that have file size greater than 1MB, we need to filter the files that have the file size in MB,this is carried out using `match()` function along with `awk` command.
If the matching files have a size greater than 1, the whole metadata is printed
```
$ ls -lh | awk 'match($5,/M/) {if($5>1) print $0}'
```
![Screenshot (694)](https://github.com/user-attachments/assets/88968494-2ca0-4b3c-9e2b-07dc5d05a110)
![Screenshot (695)](https://github.com/user-attachments/assets/0198c6fb-2169-4973-8f60-dea9680c141f)

### Q2. Replace all occurrences of "localhost" with "127.0.0.1" in a configuration file named config.txt, and save the updated file as updated_config.txt.
A config.txt file is created using the commands `touch` and `vi`. The created file can be viewed using the following command:
```
$ cat config.txt
```
![module2-qn2](https://github.com/user-attachments/assets/62ec7616-4d1c-4e2c-9270-42aaac243879)

Stream editor, `sed` command is used for text manipulation and in order to replace the string "localhost" with "127.0.0.1" on all occurances,the following command is given:
```
$ sed 's/localhost/127.0.0.1/g' config.txt > updated_config.txt
$ cat updated_config.txt
```
![module2-qn21](https://github.com/user-attachments/assets/625a0395-ea91-4dd9-9848-c3da572858f8)

### Q3. Use the appropriate command to search for lines containing the word "ERROR" in a log file but exclude lines containing "DEBUG". Save the results to a file named filtered_log.txt.
In order to search for a string in the entire file, `grep` command can be used. To ignore case sensitivity, -i option is included in the grep command and the resulting output is redirected to filtered_log.txt which can be viewed using `cat` command
```
$ cat log.txt | grep -i "ERROR" > filtered_log.txt 
```
![Screenshot (697)](https://github.com/user-attachments/assets/1eb69550-cc9d-473b-8677-693b0455bc30)

### Q4. Write a code to identify the process with the highest memory usage and then terminate it.
Out of the available memory management commands, `top` gives the output of CPU and Memory usage. By identifying the process id of process utilizing the highest memory, `kill` command can be used to kill that particular process.
```
$ top
$ kill 21582
$ top
```
![Screenshot (698)](https://github.com/user-attachments/assets/289d293a-8ff4-46e0-b6dc-b129bb6f5f8f)
![Screenshot (699)](https://github.com/user-attachments/assets/5429ea1d-63c3-44ef-8d76-c549cb17ced8)

### Q5. Use the networking tool command and print all the gateway available in a sorted manner.
Using `netstat` command and -rn option which shows a table of Destination,Gateways and Interface details, the `awk` command can be used to display only the gateways since the data is present in tabular format.
The below given command searches for ip address by searcing presence of of period(.) in the second column of the `netstat` command which displays the Gateways. The answer produced is piped and sorted in order to get the required answer.
```
$ netstat -rn | awk 'match($2,/\./) {print $2}' | sort
```
![Screenshot (700)](https://github.com/user-attachments/assets/fc7d42c1-da4e-4582-b1aa-3d37d05802e3)
