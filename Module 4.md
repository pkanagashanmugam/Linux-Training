# Module 4 - Different Operators / Conditions in Shell scripting

### Q. For the attached file, write a bash script which should take the file as input and have to go through it line by line and need to generate an output file (say output.txt) with listings of following parameters fetched from the input file. 
### Params expected to be fetched from input.txt file : "frame.time", "wlan.fc.type", "wlan.fc.subtype"
The presence of output file "output.txt" is checked and if it is not present, then it is created using the `touch` command.
```
#Check if the output file is already existent, if not create the output file
if [ ! -f "output.txt" ];then
        touch "output.txt"
fi
```
The given input file "input.txt" contains approximately 3.2M lines. In order to traverse the file line by line, a while loop is used by reading the file line by line using `read -r` command.

Using the `awk` command, the key value of each line is obtained by using ":" as the Field Sepearator and retrieving the 1st column. 
This key value is checked against the given parameters and if it matches any one of the parameters, it is appended to the output file using the redirection `>>` operator.
This iteratively continues for each of the 32 lakh lines present in the file. The input to the loop is given by using a modification of the redirection operator, `<`.
```
#Read the file line by line using while loop
while read -r line; do
    #Retrieving the key value using awk command using ":" as the field seperator
    keyword=$(echo $line  | awk -F ":" '{print $1}')
    #Checks if the key value matches any one of the given parameters,if matched, it appends the whole line to the output.txt file
    if [[ $keyword == "\"frame.time\"" || $keyword == "\"wlan.fc.type\"" || $keyword == "\"wlan.fc.subtype\"" ]]; then
        echo "$line" >> "output.txt" 
    fi
#Input text file is given using redirection 
done < "input.txt"
```
