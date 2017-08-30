1. [Expansion and globbing](#expansion-and-globbing)
2. [Types of quotes](#types-of-quotes)
3. [Types of braces](#types-of-braces) 
4. [Previous command references](#previous-command-references)
5. [if statement](#if-statement)
6. [Scripting and Debug](#scripting-and-debug)

### Expansion and globbing  

##### `$(asdf)` 
Performs command substitution 


### Types of quotes:  
##### Back Ticks  `` ` ` ``  
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

TODO add examples


### Scripting and Debug
##### `set -x`
  Put in the beginning of a bash script to output everything the script does

##### `#!`
  Indicate script should use bash to run with `#!/bin/bash` or `#!/usr/bin/bash`  



### Read in a file:  
```bash  
value=$(<config.txt)  
```  