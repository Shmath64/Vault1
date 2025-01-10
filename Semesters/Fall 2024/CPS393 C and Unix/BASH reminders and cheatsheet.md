
Note that when working with arguments that are strings with spaces, use double quotes around parameters like so:
```BASH
#!/bin/bash
#u4Labq01.sh; converts lowercase characters to uppercase
casefold() {
	echo "$1" | tr 'A-Z' 'a-z' #Still works without the quotes here
}

casefold "$1"
```


#### For Loops
```Bash
for i in {0..4}; do
	echo -n "$i: "
	echo ${students[i]}
done
```
