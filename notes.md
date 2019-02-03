# An intro to the shell

## Shell vs (bash|zsh|ksh|fish(yuck!))

## You're always in a directory

Every program is always in a directory and your shell is no different. To check
what directory you're in, us `pwd`

To change directories, use `cd`. What directories are there? Check with `ls`!
Use `ls -l` to see permissions and `chmod` to alter permissions.

There's one more command you need and then you're set! Yes, it's that easy!

`man` is used to see the 'manual' page for any command you want to get more
information about.

## A few more commands might be helpful tho...

`mkdir`
`cp` (mention -t)
`mv` (mention -t)
`rm`

`find` - super useful but I'm no expert
`tree` - simple tree view, but doesn't come installed

## Handy things

Tab completion! Use it! (zsh does it better)

Globbing - * and ?
* matches any number of any characters
? matches one of any character

## Everything is a file (sorta)

Need random data? Read from /dev/urandom: `cat /dev/urandom`

Need a lot of 0's to erase your harddrive? /dev/zero has your back

Hard drives, USBs, CD drives, etc are all 'file's that you can mount into your
filesystem and access just like any other.

## Pipes

[The UNIX philosophy was] summarized by Peter H. Salus in A Quarter-Century of
Unix (1994):

 - Write programs that do one thing and do it well.
 - Write programs to work together.
 - Write programs to handle text streams, because that is a universal interface.

So the shell allows you to make your programs work together. You can send the
output (stdout) of one program to the input (stdin) of another program using the
pipe `|` (on the key above enter on many keyboards).

Let's look into how I use my shell!

We can use the `history` command to see what commands I've run recently.

Now I want to see what directories I cd into. `history | grep "\<cd\>"`

Maybe I want to know what shell commands I use the most?

Let's take it one piece at a time. We only need the command name, not any of the
parameters to the commands. In bash and zsh, `history` includes a number before
the command. So we need the second item on each line of `history`.

`history | awk '{print $2}'` will do it.

Now I want to know how many of each thing there are. We can achieve this with a
combination of `sort` and `uniq`. Note that `sort -u` would get us a sorted list
of unique commands, but we need the count provided by `uniq -c`.

`history | awk '{print $2}' | sort | uniq -c`

Now all that's left is to make it easier to read! Another sort will do it.

`history | awk '{print $2}' | sort | uniq -c | sort -n`

Say we want to remove sudo so that we see what we used sudo with instead.

`history | sed -e 's/sudo//' | awk '{print $2}' | sort | uniq -c | sort -n`

## Redirection

The output from a program can be sent to a file, and a file can be sent to the
input of a program.

`>` or `>>` or `>&` or `>>&`

the `&` includes stderr.

## Scripting

bash is a full fledged language in its own right, with variables, functions,
loops, conditionals, etc. Use that to your advantage! This is a function I have
that makes it so that every time I `cd` it automatically `ls`s for me:

```bash
function cd() {
  emulate -LR zsh
  builtin cd $@ &&
    ls -a
}
```

<!-- vim: set tw=80:-->
