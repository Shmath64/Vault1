`ls` lists directory
- `-l` will show permissions and last edited date
- `-t` sorts newest to oldest
- `-R` recursively lists
- `$ls AlanTuring*` will show everything that starts with AlanTuring
- `$ls *.c` will list all `.c` files
`cd` by itself will take you home
`cd -` will teleport you back to where you just were
`mv` moves files / directories and can rename them
`touch` lets you know characters, words, and lines and can be used to create files
`cp` copies directories
`diff` tells you the difference between two files
`cat` reads file and `tac` reads the file backwards (by line)
`more` reads files starting at the top, and shows one 'screenful' at a time
`less` is like `more` but with more features
`echo` is like a print statement
- `echo -e` enables escape sequences (e.g. `\t` makes a tab)
`head` and `tail` prints the first or last 10 lines of a file
`rev` reverses lines
`sort` prints lines in alphabetical order
`uniq` does NOT print redundant lines
`tree` lists directories in a pretty way
`#` makes a comment 
`man` shows the "manual" for the command placed after `man`
- `man -k zip` gives keywords relevant to `.zip` files
`chmod` changes permissions
- First determine who (`u`, `g`, and/or `o`) 
- Then use either "symbolic" mode (+-=) (rwx) or "numeric mode" (number 0-7) 
	- To use numeric mode, you need to specify 3 numbers!
`grep` searches patterns in each file.
- `$grep -l touch u?.txt` searches for the word "touch"  use `-li` to ignore case
`sed` stream editor
`rm` removes files
`rmdir` removes direcoties
`tee` can be used to save data to a file from a pipe!
- `cat AlanTuringBio80 | head -54 | tee first54LinesFile | tail -1
