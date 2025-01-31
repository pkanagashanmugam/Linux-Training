# Module 5 - Advance Topics in a Function

### Q. Create a Bash script 'file_analyzer.sh', to demonstrate the following concepts: 
**SPECIAL PARAMETERS:**

Use parameters like '$0', '$#', '$?' and '$@' to provide meaningful feedback.

```
#Using Special Parameters to check the number of command line arguments passed.
if [ $# -eq 0 ];then
    echo "Invalid format!!"
    echo "Try using --help option for more details"
    exit 1
fi
```
**RECURSIVE FUNCTION AND REDIRECTION [ERROR HANDLING]:**

Write a recursive function to search for files in a directory and its subdirectories containing a specific keyword.

A commmon function is written to perform search of a keyword both, in a file as well as in a directory containing subdirectories. Hence the function *search* is written which accepts three parameters *options,file and keyword*. 

The function checks if the given option is `-d` and if so, it iterates through all the files in the directory by using `for file in "$directory"/*; do` command.If the file is found to be a directory using the `-d filename` command, then the reccursive call takes place by passing the *option,current file and keyword* as the arguments.
The function call happens until it finds a file and the `grep -q` command is used to perform grep operation and not produce any output. The exit status is checked and if it results in 0, then the line along with the file it was found in is appended to the **output.txt** file.

Alternatively if the file is executed with `-f` option, using File descriptors and error handling redirection, `grep -nH` is used to retrieve line number along with file name and if the command results in a error, the error is redirected to *errors.log* file.
```
search(){
    option=$1
    directory=$2
    keyword=$3
    if [ "$option" = "-d" ];then
        for file in "$directory"/*; do
            if [ -d "$file" ]; then
                #Recursive function call to iterate through all the files and subdirectories in the directory
                search "$option" "$file" "$keyword" 
            elif [ -f "$file" ]; then
                grep -q "$keyword" "$file" 
                if [ $? -eq 0 ]; then
                    echo "$keyword found in $file"
                    #File Handling and Error Redirection. Redirecting errors related to file
                    grep -nH "$keyword" "$file">>"output.txt" 2>> errors.log
                fi
            fi
        done
    else
        grep -nH "$keyword" "$directory" > output.txt 2>> errors.log && echo "$keyword found in $directory" 
    fi
}
```
**HERE DOCUMENT:**

The `help` function uses a Here Document and displays the description and information about the shell script which will be displayed to the user when `./file_analyzer.sh --help ` command is executed.
```
#Using Here Document to display the help option
help(){
        cat << --HELP
        NAME:
                file_analyzer.sh
        SYNOPSIS:
                ./file_analyzer.sh [OPTION ...] FILE NAME [OPTION ...] PATTERN
        DESCRIPTION:
                This Shell Script recursively searches for an keyword either in a directory or a file according to the option chosen
        OPTIONS:
                -d,directory search
                         Recursively searches all the contents of the directory and its subdirectories for the given keyword
                -k,keyword
                        Keyword to search
                -f,file search
                        File to search directly
                --help
                        Display the help menu.

--HELP
}
```
**REGULAR EXPRESSION**:

Validate inputs with regular expressions (Check if the file exists and the keyword is not empty and valid).
The inputs are validated using `echo "$4" | grep -qE '^[a-zA-Z0-9]+$'` to allow only non empty alphanumeric values.
According to the option with which the file is executed, appropriate operations are carried over.
```
if [ "$1" = "-d" ] ||[ "$1" = "-f" ];then
        if [ $# -eq 4 ] && [ "$3" = "-k" ];then
                option=$1
                #Using Regular Expression to validate the input keyword
                if ! echo "$4" | grep -qE '^[a-zA-Z0-9]+$'; then
                        echo "Invalid keyword: '$4'. Only non-empty alphanumeric characters are allowed." >> errors.log
                        exit 1
                else
                        word_to_search=$4
                fi
                directory_to_search=$2
                search $option $directory_to_search $word_to_search
        else
                echo "Invalid format!!"
                echo "Try using --help option for more details"
                exit 1
        fi
elif [ "$1" = "--help" ];then
        help
else
        echo "Invalid format!!"
        echo "Try using --help option for more details"
        exit 1
fi
```

![Screenshot (708)](https://github.com/user-attachments/assets/bc5b4497-a24c-4497-ba7e-9bd34192f7e3)

![Screenshot (709)](https://github.com/user-attachments/assets/7a8d5a3c-4bd2-4b81-9d56-e4c2dbda41ae)

[Shell Script](https://github.com/Sharath15eUR/kanaga/blob/main/Kanaga%20Shanmugam_Linux%20Training/Module%205/file_analyzer.sh)

[Output of Shell Script](https://github.com/Sharath15eUR/kanaga/blob/main/Kanaga%20Shanmugam_Linux%20Training/Module%205/output.txt)

[Directory created by Shell Script](https://github.com/Sharath15eUR/kanaga/blob/main/Kanaga%20Shanmugam_Linux%20Training/Module%205/module5_directory.zip)
