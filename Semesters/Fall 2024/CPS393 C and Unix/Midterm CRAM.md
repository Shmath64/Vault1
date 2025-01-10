
```Bash
#!/bin/bash
# Take 3 command line arguments,
# Two files and an integer,
#If the FIRST N lines of File1 are identical to the LAST N lines of File2,
#   then program prints:
#   First N lines of File1 and last N lines of File2 are identical
#
#   If the FIRST N lines of File1 and the LAST N lines of File2 differ,
#   then program prints:
# First N lines of File1 and last N lines of File2 differ

firstFile="$1"
secondFile="$2"
n="$3"

head -$n "$firstFile" > F1
tail -$n "$secondFile" > F2


if diff F1 F2; then
        echo "First ${n} lines of ${firstFile} and last ${n} lines of ${secondFile} are identical"
fi
```
The "quotes" aren't *strictly* needed but they may prevent an error if spaces/tabs are included

Valid way to work with multiple arguments:
`$*` is list of args, `$#` is num of args,
And curly braces aren't strictly necessary but help isolate variable name from other parts of the string
```Bash
for var in $*; do
        declare -i count=$(factor ${var} | wc -w)
        ((count -= 1))
        echo "${var}: $count prime factors"
done
```
Note the lack of `$` when doing math  
Although, if we were using an assignment operator, we'd need this:
`count=$((count + i))`

Commands in brackets can be called "subshells"

To search files of a directory for a keyword in their contents
```Bash
dirName="$1"
keyword="$2"

if [[ $# != 2 ]]; then
        echo "$# aint the right number of args, I need 2"
        exit 1
fi

cd $dirName
files=$(ls)

for file in $files; do
        fileContent=$(cat $file)
        if [[ $fileContent == *"$keyword"* ]]; then
                echo $file
        fi
done
```

