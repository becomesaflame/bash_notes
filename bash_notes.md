

### Expansion and globbing  

`$(asdf)` 



### Types of quotes:  
`` ` ` ``  
`" "`   
`' '`    


### Types of braces:  
`( )`  

`[ ]`  
Conditional expansion:   
```bash
if [ -d /tmp ]; then
  echo "/tmp is a dir"
fi
```
  
  
`[[ ]]`  
Also conditional expansion.  
No pathname expansion or word splitting takes place between `[[` and `]]`  
`[[ ... ]]` allows for regular expression matching where `[ ... ]` does not.  
  
Example:  
```bash
x='name with space.sh'
[ $x == *.sh ]          # Expands $x to 3 separate arguments, and *.sh to
                        #  all matching files in the current dir
[[ $x == *.sh ]]        # Compares 'name with space.sh' to *.sh, matches
```

  
`(( ))`  
  Prevents globbing  
  
`{ }`  


  
### $ things:  
`!!`  
  Last command  
  
`!$`  
  Last arg of last command  
  
`$`  
  Expand variable  
  
`$#`   
  Number of args to this script  
  
`$?`    
  The exit code of the last command  
  
`$0`  
  This command  
  
`$1`   
  First arg to this script  
  


### if statement:  
(Comprehensive syntax)[http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_07_01.html]   
(Other numerical comparison
operators)[http://www.tldp.org/LDP/abs/html/comparison-ops.html]   
  
`-e FILE`  
  True if FILE exists  
  
`-z STRING`  
  True if length of STRING is zero  







### Read in a file:  
```bash  
value=$(<config.txt)  
```  
