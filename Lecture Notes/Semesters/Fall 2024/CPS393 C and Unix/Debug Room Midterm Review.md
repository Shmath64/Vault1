Practice programming in Linux with Vim and BASH.

We can use a `find` command that combines multiple criteria.
`find .name c2 -prune -o -name "m*c" -print`
look for something named c2 (and stop when you find it) OR for find something that is `m*c`. By using `-print`, only files named `m*c` will be printed.

![[CPS393DebugRoom-1.png]]

Seriously got to review `grep` and `cut`

`grep "echo" $(ls *g*)`
`grep "echo {ls *g*}`