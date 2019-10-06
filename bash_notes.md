1. [Expansion and globbing](#expansion-and-globbing)
2. [Types of quotes](#types-of-quotes)
3. [Types of braces](#types-of-braces) 
4. [Previous command references](#previous-command-references)
5. [if statement](#if-statement)
6. [Scripting and Debug](#scripting-and-debug)
7. [Filename manipulation](#filename-manipulation)
8. [Output redirection](#output-redirection)
9. [Parameter Expansion](#parameter-expansion)
10. [String Manipulation](#string-manipulation)
11. [Loops](#loops)
12. [Arrays](#arrays)
13. [Associative Arrays](#associative-arrays)
14. [Process Substitution](#process-substitution)
15. [Useful Command Examples and Design Patterns](#useful-command-examples-and-design-patterns)

### Expansion and globbing  

##### `$(asdf)` 
Performs command substitution 


### Types of quotes:  
##### Back Ticks  `` ` ` ``  
Used for command substitution.  `$()` notation is preferred.

##### Double Quotes `" "`   
##### Single Quotes `' '`    


### Types of braces:  
##### Parentheses `( )`  

##### Double Parentheses `(( ))`  
  Prevents globbing  
  
##### Brackets `[ ]`  
Conditional expansion:   
```bash
if [ -d /tmp ]; then
  echo "/tmp is a dir"
fi
```

`[ ... ]` and `test ...` can both be used for conditionals. Both are builtins.  
  
  
##### Double Brackets `[[ ]]`  
Also conditional expansion.  
No pathname expansion or word splitting takes place between `[[` and `]]`  
`[[ ... ]]` allows for regular expression matching where `[ ... ]` does not.  
Double brackets also allow you to use `&&` and `||` instead of `-a` and `-o`.  
Regular expression matching operator is `=~`.  
  
Example:  
```bash
x='name with space.sh'
[ $x == *.sh ]          # Expands $x to 3 separate arguments, and *.sh to
                        #  all matching files in the current dir
[[ $x == *.sh ]]        # Compares 'name with space.sh' to *.sh, matches
```
Note that `[ ... ]` is a builtin and executes at runtime, whereas `[[ ... ]]` is a syntax feature of the shell lanugage and is parsed up front.  See (this oilshell blog post)[http://www.oilshell.org/blog/2016/10/12.html] for an in-depth discussion of these differences.  
  
##### Braces `{ }`  
Allow variable manipulation:  
* Truncate the contents of a variable
```bash
$ var="abcde"; echo ${var%d*}
abc
```

* Make substitutions similar to sed
```bash
$ var="abcde"; echo ${var/de/12}
abc12
``` 

* Use a default value
```bash
$ default="hello"; unset var; echo ${var:-$default}
hello
``` 

* and several more

  
### Previous command references:  
##### Bang Bang `!!`  
  Last command  
  
##### Bang Dollar `!$`  
  Last arg of last command  
  
##### Dollar `$`  
  Expand variable  
  
##### Dollar Hash `$#`   
  Number of args to this script  
  
##### Dollar Question `$?`    
  The exit code of the last command  
  
##### Dollar Number 0 `$0`  
  This command  
  
##### Dollar Number `$1`   
  First arg to this script  
  


### if statement:  
(Comprehensive syntax)[http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html]   
(Other numerical comparison
operators)[http://www.tldp.org/LDP/abs/html/comparison-ops.html]   
  
##### `-e FILE`  
  True if FILE exists  
  
##### `-z STRING`  
  True if length of STRING is zero  

##### Examples:
```
if [ ! -d "aFolder" ]; then
  mkdir "aFolder"
elif [ -d "bFolder" ]; then
  rmdir "bFolder"
else
  rmdir "aFolder"
fi
```

### Scripting and Debug
##### `set -x`
  Put in the beginning of a bash script to output everything the script does, or run script with `bash -x script.sh`

##### `#!`
  Indicate script should use bash to run with `#!/bin/bash` or `#!/usr/bin/bash`  



### Read in a file:  
```bash  
value=$(<config.txt)  
```  

### Filename Manipulation
To strip the path from a string and leave just the filename:  
`$(basename "$pathspec")`

To strip the filename from a string and leave just the path:  
`$(dirname "$pathspec")`

To expand `~` in a variable `var` to full home directory path:
`var="${var/#\~/$HOME}"`

### Output Redirection
Redirect stdout to a log file:  
`myprogram > out.log`  

Suppress stderr:  
`myprogram 2> /dev/null`  

Suppress both stdout and stderr:
```
myprogram &> out.log      # New bash syntax
myprogram > out.log 2>&1  # Older sh syntax
```

### Parameter Expansion
Write this up:
http://wiki.bash-hackers.org/syntax/pe 

#### Indirect Parameter Expansion 
Say you have a variable that contains the name of another variable:
```
foo="1234"
bar="foo"
```
If you try `echo $bar` you will get `foo`. 
If you want to evaluate the varable whose name it contains, you can use indirect expansion:
```
echo ${!bar}
```
This will return `1234`.

#### Nameref
A variable can be created to point to another variable.
Interactive: 
```
x="1234"
declare -n foo=x
echo $foo
```

In a function:
```
x="1234"
local -n foo=x
echo $foo
```

This is useful for manipulating variables names, e.g.
```
version="2_3"
local -n build="rev_$version"
```
`build` will now point to the value of the variable `rev_2_3`. You can assign a value to that variable using the nameref:
```
build="foo"
echo $rev_2_3
```

If you use indirect expansion for a nameref, you will get the name of the variable it points to.
```
echo "${!build}"
```
will return `rev_2_3`.

### String Manipulation
Concatenate variables with strings:
`foo="${bar}world"`

Delete shortest match of `$substring` from front of `$string`:
`${string#substring}`

Delete longest match of `$substring` from front of `$string`:
`${string##substring}`

Delete shortest match of `$substring` from back of `$string`:
`${string%substring}`

Delete longest match of `$substring` from back of `$string`:
`${string%%substring}`

[More string manipulation](https://www.tldp.org/LDP/abs/html/string-manipulation.html)

### Loops
#### For Loop
```
for i in $( ls ); do
    echo item: $i
  done
```

Loop through array:
```
arr=("a", "b", "c")
for i in "${arr[@]}"
do
   echo "$i"
done
```


### Arrays
Append to an array:
`arr+=(4)`

Print all values of an array:
`echo "{arr[@]}"`

### Associative Arrays
Good overview at [https://www.artificialworlds.net/blog/2012/10/17/bash-associative-array-examples/](https://www.artificialworlds.net/blog/2012/10/17/bash-associative-array-examples/)

#### Pass an Associative Array into a Function
```
printKeys () {
  var=$(declare -p "$1")
  eval "declare -A arr="${var#*=}
  
  echo "${!arr[@]}"
 }
 
declare -A aa='([bar]="bard" [foo]="food" )'
printKeys "aa"
```

### Process Substitution
Used to pipe the `stdout` of one command into the `stdin` of another, or vise versa
`<(command)`
`>(command)`

### Useful Command Examples and Design Patterns
Rename all files in a folder:  
`for file in *.png; do mv "$file" "${file/_h.png/_half.png}"; done`
  For more substition explanation, go [here](https://www.tldp.org/LDP/abs/html/parameter-substitution.html)

#### Check if called from script
Add a main function that's only called if the script is being called directly from the command line:
```
main() {
    xyz
    echo "Entering script1's main()"
}

if [[ "${BASH_SOURCE[0]}" == "${0}" ]]; then
    main "$@"
fi
```

#### Recursive glob
Recurses through the given directory and its subdirectories, returning a list of files that match the glob string
```
 proc ::globr { baseDir pattern } {
   set dirs [ glob -nocomplain -type d [ file join $baseDir * ] ]
   set files {}
   foreach dir $dirs {
     lappend files {*}[ globr $dir $pattern ]
   }
   lappend files {*}[ glob -nocomplain -type f [ file join $baseDir $pattern ] ]
   return $files
 }
 ```
 
#### Create a directory if it doesn't exist
```
[ -d "foobar" ] || mkdir foobar
```

#### Main function 
```
# Run main function only if script is executed with ./<script>.sh, not when it is sourced
if [[ "${BASH_SOURCE[0]}" != "${0}" ]]; then
  main $*
fi
```

#### Read csv
Echo line 2, column 5 from file.csv:  
`echo $(awk -F, '{ if(NR==2){ print $5; } }' file.csv)`

#### Run a command using the contents of a file as its argument
mycommand $(< file.txt)

#### Parallel Rsync with no dependencies
`find . -maxdepth 1 -type d -print0 | xargs -0 -P 5 -I % rsync -ahvPz % dest/dir`
`find` produces a file list. 
The `-print0` option uses a null character as a separator, and is used with the `-0` option to `xargs`. 
The `-P 5` option to `xargs` kicks off 5 processes in parallel. 
The `-I %` is used to insert the inputs into a specific location (denoted by the `%` symbol) into the `rsync` command.



