# Module 3 - Linux distributions and Scripting Language

## Q. Scenario: Automating file backup and Reporting to the system. Create a shell script called "backup_manager.sh" that performs the following tasks incorporating the concepts suggested :

### Command-line Arguments and Quoting :
The script must accept three arguments: Source directory: A directory containing files to back up. Backup directory: The destination where files will be backed up. File extension: A specific file extension to filter (e.g., .txt).

In order to handle the command line arguements, we assign three variables to handle source folder, destination folder and the required file type extention.
```
if [ $# -eq 3 ];then
	source_directory=$1
	destination_directory=$2
	file_type=$3
else
	echo "Positional Arguements passed does not correspond to the required format, Please Try Again!"
fi
```
### Globbing :
The script should use globbing to find all files in the source directory matching the provided file extension.

Globbing is performed by using asterik (*). Files that are of the specified extentions in the source directory are printed.
```
echo "Files that match the given extention are:"
ls $source_directory/*$file_type
```
### Export Statements :
Use export to set an environment variable BACKUP_COUNT, which tracks the total number of files backed up during the script execution. In addition, a variable,total_size_backedup is set to zero to keep track of the total file size that are backed up.
```
export BACKUP_COUNT=0
total_size_backedup=0
```
### Array Operations:
Store the list of files to be backed up in an array.

By using globbing, the result is passed on the to array declared using the `declare -a` command.
```
declare -a backup_files
backup_files=($source_directory/*$file_type)
```
Print the names of these files along with their sizes before performing the backup.

Using a for loop and `stat` command which by using the `--format="%s"` option provides the size of file passed as the next arguement in bytes.
```
echo "Files to be backed up:"
for file in "${backup_files[@]}";
do
	file_size=$(stat --format="%s" "$file")
    	echo "$file ($file_size bytes)"
done
```
### Conditional Execution:
If the source directory is empty or contains no files matching the extension, exit with a message.

The command `${#backup_files[@]}` gives the length of the array and if this results in zero then there are no files of the required format in the source directory for backup
```
if [ ${#backup_files[@]} -eq 0 ];then
	echo "No files are present in $source_directory of $file_type extention"
	exit 1
fi
```
If the backup directory does not exist, create it. If creation fails, exit with an error.

`! -d $destination_directory` command is used to check if the destination directory does not exist already and creates is using `mkdir` and `-p` option which denotes prompt and checks for the exit status using `$?` command for successful creation of the directory
```
if [ ! -d $destination_directory ];then
	#Creating Backup Directory if already non-existent 
	mkdir -p $destination_directory
	if [ $? -ne 0 ];then
		echo "Failed to create directory : $destination_directory"
		exit 1
	fi
fi
```
If a file already exists in the backup directory with the same name, only overwrite it if it is older than the source file (compare timestamps).

```
for file in "${backup_files[@]}";
do
	#To remove the Directory name associated in the file and extract only file name
	file_name=$(echo "$file" | awk -F "/" '{print $NF}')
	#Backup File
	current_backup_file="$destination_directory/$file_name"
	#Getting the size of the file
	file_size=$(stat --format="%s" "$file")
	#Checks if the current file to backup is already present in the backup destination
	if [ -f "$current_backup_file" ];then
		#Compare the timestamps of file in the directory with backup file
		if [ "$file" -nt "$current_backup_file" ];then
			old_file_size=$(stat --format="%s" "$current_backup_file")
			total_size_backedup=$((total_size_backedup-old_file_size))
			#Overwrite the old file with newer contents
			cp "$file" "$current_backup_file"
			#Calculating total size of files backed up
			total_size_backedup=$((total_size_backedup+file_size))
		fi
	else
		#If file is not already present in the backup folder, backup the file
		cp "$file" "$current_backup_file"
		#Calculatinf total size of the files backed up
		total_size_backedup=$((total_size_backedup+file_size))
	fi
	BACKUP_COUNT=$(($BACKUP_COUNT+1))
done
```
### Output Report:
After the backup, generate a summary report displaying:
Total files processed.
Total size of files backed up.
The path to the backup directory.
The report should be saved in the backup directory as backup_report.log.

```
backup_report_log="$destination_directory/backup_report.log"
if [ ! -f "$backup_report_log" ];then
	touch "$backup_report_log"
fi
{
echo ""
echo "Back Up Report : "
echo "Date of Backup : `date`"
echo "*************************************"
echo "Total number of files processed : $BACKUP_COUNT"
echo "Total size of files processed : $total_size_backedup"
} >> "$backup_report_log"
```
Printing the final message by checking the exit status.
```
if [ $? -eq 0 ];then
	echo "Automation Script Successfully Completed and Report Log successfully saved"
	exit 0
else
	echo "Failed to save Report Log"
	exit 1
fi
```
### Execution:
Contents of the Downloads Directory:

![Screenshot (704)](https://github.com/user-attachments/assets/7172b7ed-a3fc-4f2e-a612-2ddeaa611db6)

Execution of Shell Script:

![Screenshot (705)](https://github.com/user-attachments/assets/f4341926-74d3-432d-a4aa-f948e91743f8)

[Shell Script](https://github.com/Sharath15eUR/kanaga/blob/main/Kanaga%20Shanmugam_Linux%20Training/Module%203/backup_manager.sh)

[Backup Directory created by execution of Shell Script](https://github.com/Sharath15eUR/kanaga/blob/main/Kanaga%20Shanmugam_Linux%20Training/Module%203/module3_directory.zip)

[Backup Report created by execution of Shell Script](https://github.com/Sharath15eUR/kanaga/blob/main/Kanaga%20Shanmugam_Linux%20Training/Module%203/backup_report.log)
