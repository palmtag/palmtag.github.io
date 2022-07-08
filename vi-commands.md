# Vi Commands

July 2022

# Introduction

Vi is a text editor installed on almost all Unix workstations.
The editor is different from editors.  Instead of having a GUI interface,
it is written to use "shortcut" keys.   Once you know the shortcuts,
it is a very powerful editor, but there is a learning curve to know
the shortcuts.

The first thing to notice about Vi is that there are two modes.
One first mode is the "command mode" where you input commands to modify the text.
The second mode is the "insert mode" where you are modifying commands.
To get out of the insert mode, hit the <esc> key.

There are many, many Vi commands that you can use.  The following is a list of 
commands that I use fairly regularly.

# Exiting Vi

The first problem that new Vi users encounter is how to exit Vi.
You can usually do this by hitting the \<esc> key to get into command mode,
then issue the command ":q" or ":q!" to quit.

Sometimes you will get thrown into the help menu.  In this case, type ":q"
to get out fo the help menu, then ":q" or ":q!" to exit the program.

# Commands 

## Exit and Save

* :q  quit - this will give a warning if you have unsaved changes
* :q! really quit - this will quit and ignore any unsaved changes
* :w  write file
* :wq write file and quite

## Simple Text editing

* i     insert - insert text at cursor position.  Hit \<esc> when finished.
* r     replace - replace text at cursor position.  Hit \<esc> when finished.
* x     delete - delete one character
* dd    delete line - delete entire line
* Y     yank - yank text - described below
* P     put - put yanked text - described below
* "."   repeat previous command
* u     undo - undo previous command.  You can press this more than once to delete multiple commands.

Note that "i" and "r" will place you in insert mode.  Hit \<esc> to exit insert mode.

You can precede "x" and "dd" with integers to delete multiple characters or lines.
"5x" will delete 5 characters, "10dd" will delete 10 lines.

## Moving Around in Document

* \<arrow keys>  will move around file
* \<enter>   move down one line
* -     Move up one line
* 0     Move to start of line
* $     move to end of line
* G     move to last line in file
* \<n>G  move to line \<n>, where \<n> is an integer

## Searching

* /string  - will search file for "string" (case sensitive)
* n        - after finding string, this will move to next occurance
* N        - find next occurrance, but move up in file

Note that you can use special characters in search.
* "." will match with any character.
* "*" will match any number of characters
* "$" will match end of file
* "^" will match beginning of file
* "[0-9]" will match any digit 0 to 9

If you want to use these special characters are regular occurances, 
you must precede them with a backslash  (like \., \[, etc.)

Examples
* "/ $" will find lines ending with blank
* "/^2" will find lines starting with 2
* "/c[0-9]\.inp"  will find all occurrences like c1.inp, c2.inp, c5.inp, etc.
* "/$^B.*\.txt$"  will find all lines that start with "B" and end with ".txt"

## Search and Replace

* :%s/old/new/g   will replace all occurances of the string "old" with "new" (global search and replace).

Searching can get very complicated (but powerful).  Refer to advanced texts for more information.

## Moving text

When you delete text or "yank" text, it is moved to a buffer.
You can then place this text in other places using the "P" (put) command.

For example, you can delete 10 lines with "10dd", move to a new location and put them with "P".
If you don't want to delete the original lines, you can use "10Y" to yank them without deleting.












