# Bash
GNU Bourne-Again SHell
*Try this at home!*

---

## Running Interactive
```bash
god@earth:/root#
```

---

## Running Interactive
```bash
god@earth:/root# mycommand arg1 arg2 arg3
```

---

## Running Interactive
up- and down-arrows navigate last commands
`history` prints the previous executed commands
`ctrl+R` search history
`ctrl+D` exits
`shift+PageDown`/`PageUp` move view

---

## Running Interactive
if login: `~/.bash_profile`
if not login: `~/.bashrc`

---

## Running from a Script
Shebang:
```bash
#!/usr/bin/env bash
```
Make it executeable:
```bash
chmod +x myscript
```
Call it as path or from `$PATH`:
```bash
god@earth:/root# ./myscript
```

---

## Comments
```bash
# I'm a comment!
complicated-command # I'm a comment!
```
Also useful in interactive execution!

---

## Shell Options
activate with `-`, deactivate with `+`
```bash
set -o
```
```bash
#!/usr/bin/env bash -o
```
`-x` for debugging
`-e` for exiting if any command fails

---

## Variables
```bash
something="I'm a String"
SOME_OTHER=42
echo '$something'
echo "$something"
echo $SOME_OTHER
echo "${something}!"
```

---

## Variables
```bash
something="I'm a String"
SOME_OTHER=42
echo '$something' # $something
echo "$something" # I'm a String
echo $SOME_OTHER # 42
echo "${something}!" # I'm a String!
```

---

## Variables
```bash
something="I'm a String"
SOME_OTHER=42
echo '$something' # $something
echo "$something" # I'm a String
echo $SOME_OTHER # 42
echo "${something}!" # I'm a String!
```

All variables are in global scope unless declared with the `local` keyword.

---

## Arguments
`$0` contains the path to the executing programm.
`$1`, `$2`, ... contain the arguments.
```bash
echo hello world
echo "hello world"
```
`shift` deletes the first argument

---

## Special Variables
```bash
$? # exit code of thte preivous program
$# # number of arguments
$$ # pid of the shell
$! # pid of most recent background progress
$- # active shell options
```

---

## Pipelines
```bash
head largefile.txt | sort | nl
```
wire *stdout* to *stdin* of the next command

---

## Redirection
```bash
> # pipes stdout to a file
2> # pipes stderr to a file
2>&1 # pipes stderr to stdout
1>&2 # pipes stderr to stderr
>> # appends
< file # pipes file's content to stdin
```

---

## Lines and Readability
```bash
echo 'one'; echo 'two' # two independet commands
echo \
'three' # this belongs to the command above
```

---

## Conditional Execution
These are *lazy* logical evaluations based on exit code (0 means true):

```bash
command1 && command2
command3 || command4
```

---

## If Statements
```bash
if command1; then
    command2
else
    command3
fi

if (( 1 < 2)); then
    echo '1 is smaller than 2!'
fi

if [ -f 'file.txt' ]; then
    echo 'file.txt is a file!'
fi

if [[ 'asdf' =~ ^[a-z] ]]; then
    echo 'it matches!'
fi
```

---

## Case Statements
```bash
myvar='pink'
case $myvar in
  green)
    echo "It's a frog."
    ;;
  pink|purple)
    echo "It's an elephant!"
    ;;
  *)
    echo "I don't know that."
    ;;
esac
```

---

## Loops
```bash
for item in apple banana clementine; do
  echo $item
done

while true; do
  echo "may I have some loops?"
done

< file.txt | while read line; do
  echo $line
done
```

---

## Functions

```bash
my_function(){
  echo "I'm my_function, called with $1!"
}

function other_function{
  echo "I'm other_function!"
  return 42 # exit code of the function
}

my_function "an argument"
other_function
echo $?
```

---

## Interactive Script Promt
```bash
echo -n 'Are you sure? [y/n]: '
read answer
echo "You answered $answer."
```

---

## Interactive Script Promt
```bash
echo "Which OS do you want?"
select os in linux windows macos; do
  case $os in
    linux)
      echo "Ah, I see you're a man of culture as well."
      break # jump out of this endless loop
      ;;
    *)
      echo "Ok."
      ;;
  esac
done
```

---

## Aliases
```bash
alias godmode="sudo -i"
alias echo="echo God says:"
```

---

## Expansion

```bash
myvar='myvarcontent'
cat ~/{a,$myvar} $(echo "c d") "$((1+1))" /b* <(ls /etc/passwd)
```

---

## Expansion

```bash
myvar='myvarcontent'
cat ~/{a,$myvar} $(echo "c d") "$((1+1))" /b* <(ls /etc/passwd)

# Brace Expansion
cat ~/a ~/$myvar $(echo "c d") "$((1+1))" /b* <(ls /etc/passwd)

# Tilde Expansion
cat /home/god/a /home/god/$myvar $(echo "c d") "$((1+1))" /b* <(ls /etc/passwd)

# Parameter Expansion
cat /home/god/a /home/god/myvarcontent $(echo "c d") "$((1+1))" /b* <(ls /etc/passwd)

# Command Substitution
cat /home/god/a /home/god/myvarcontent "c d" "$((1+1))" /b* <(ls /etc/passwd)
# "c d" means it's one argument

# Arithmetic Expansion
cat /home/god/a /home/god/myvarcontent "c d" "2" /b* <(ls /etc/passwd)
# "c d" means it's one argument

# Process Substitution
cat /home/god/a /home/god/myvarcontent "c d" "2" /b* /dev/fd/42

# Word Splitting
cat /home/god/a /home/god/myvarcontent c d "2" /b* # c and d are now two arguments

# Pathname Expansion
cat /home/god/a /home/god/myvarcontent c d "2" /bin /boot /dev/fd/42

# Quote Removal
cat /home/god/a /home/god/myvarcontent c d 2 /bin /boot /dev/fd/42
```

---

## Expansion

```bash
myvar='myvarcontent'
cat ~/{a,$myvar} $(echo "c d") "$((1+1))" /b* <(ls /etc/passwd)
# Brace Expansion
# Tilde Expansion
# Parameter Expansion
# Command Substitution
# Arithmetic Expansion
# Process Substitution
# Word Splitting
# Pathname Expansion
# Quote Removal
cat /home/god/a /home/god/myvarcontent c d 2 /bin /boot /dev/fd/42
```

---

## Brace Expansion

```bash
echo {1..7..2} # start at 1, until 7, step 2
echo /home/{alice,bob}/a{b,c{1..2}}
```

---

## Globs: Pathname Expansion
Used to match files:
`*` any string
`?` any char
`[1-3a-c]` like regex: one char that is in `123abc`

---

## Subshells
inherits everything from parent shell
does not alter the parent shell
```bash
home='home'
ls "/$(echo "$home")" # command substitution
```

---

## Including Other Scripts
avoid subshell by:
```bash
source myscript
. myscript
```

---

## Handy Commands
`cat cd cp cut du echo find grep head history less ln ls man mkdir mmv mv nl pwd rm rmdir rsync sed sort tail tr uniq watch wc which xargs yes`

---

# Any Questions?

---

# Tasks
*https://github.com/betanummeric/eportfolio-bash*

---

### Tasks
*https://github.com/betanummeric/eportfolio-bash*
1. Remove the blank lines from the puns file.
2. Write a script that takes an argument and searches for that in the puns file.
3. Validate in the script, that the puns file exists.
4. Validate in the script, that an argument was given.
5. Sort the found puns alphabetically.
6. Number the found puns.
7. Limit the result to 3 random puns.