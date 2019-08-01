# Terminal Tips
## Command Editing Shortcuts
- Ctrl + a – go to the start of the command line
- Ctrl + e – go to the end of the command line
- Ctrl + k – delete from cursor to the end of the command line
- Ctrl + u – delete from cursor to the start of the command line
- Ctrl + w – delete from cursor to start of word (i.e. delete backwards one word)
- Ctrl + y – paste word or text that was cut using one of the deletion shortcuts (such as the one above) after the cursor
- Ctrl + xx – move between start of command line and current cursor position (and back again)

- Alt + b – move backward one word (or go to start of word the cursor is currently on)
- Alt + f – move forward one word (or go to end of word the cursor is currently on)
- Alt + d – delete to end of word starting at cursor (whole word if cursor is at the beginning of word)

- Alt + c – capitalize to end of word starting at cursor (whole word if cursor is at the beginning of word)
- Alt + u – make uppercase from cursor to end of word
- Alt + l – make lowercase from cursor to end of word
- Alt + t – swap current word with previous

- Ctrl + f – move forward one character
- Ctrl + b – move backward one character
- Ctrl + d – delete character under the cursor
- Ctrl + h – delete character before the cursor
- Ctrl + t – swap character under cursor with the previous one

## Command Recall Shortcuts
- Ctrl + r – search the history backwards
- Ctrl + g – escape from history searching mode
- Ctrl + p – previous command in history (i.e. walk back through the command history)
- Ctrl + n – next command in history (i.e. walk forward through the command history)
- Alt + . – use the last word of the previous command

## Command Control Shortcuts
- Ctrl + l – clear the screen
- Ctrl + s – stops the output to the screen (for long running verbose command)
- Ctrl + q – allow output to the screen (if previously stopped using command above)
- Ctrl + c – terminate the command
- Ctrl + z – suspend/stop the command

## Bash Bang (!) Commands
Bash also has some handy features that use the ! (bang) to allow you to do some funky stuff with bash commands.

- !! – run last command
- !blah – run the most recent command that starts with ‘blah’ (e.g. !ls)
- !blah:p – print out the command that !blah would run (also adds it as the latest command in the command history)
- !$ – the last word of the previous command (same as Alt + .)
- !$:p – print out the word that !$ would substitute
- !* – the previous command except for the last word (e.g. if you type ‘find some_file.txt /‘, then !* would give you ‘find some_file.txt‘)
- !*:p – print out what !* would substitute
- ^nanp^nano -> fixing typos, or operating on same argument set. Replace nanp to nano in last cmd

### Expantions

- sudo mv /etc/rc.conf{-old,}
- sudo cp /etc/rc.conf{,-old}
- mkdir myfolder{1,2,3}
- mv /path/to/file.{txt,xml}

0120030

---

## Working With Processes
### Use the following shortcuts to manage running processes.

- Ctrl+C: Interrupt (kill) the current foreground process running in in the terminal. This sends the SIGINT signal to the process, which is technically just a request—most processes will honor it, but some may ignore it.
- Ctrl+Z: Suspend the current foreground process running in bash. This sends the SIGTSTP signal to the process. To return the process to the foreground later, use the fg process_name command.
- Ctrl+D: Close the bash shell. This sends an EOF (End-of-file) marker to bash, and bash exits when it receives this marker. This is similar to running the exit command.

---

## Controlling the Screen
### The following shortcuts allow you to control what appears on the screen.

- Ctrl+L: Clear the screen. This is similar to running the “clear” command.
- Ctrl+S: Stop all output to the screen. This is particularly useful when running commands with a lot of long, verbose output, but you don’t want to stop the command itself with Ctrl+C.
- Ctrl+Q: Resume output to the screen after stopping it with Ctrl+S.

## Fixing Typos
### These shortcuts allow you to fix typos and undo your key presses.

- Alt+T: Swap the current word with the previous word.
- Ctrl+T: Swap the last two characters before the cursor with each other. You can use this to quickly fix typos when you type two characters in the wrong order.
- `Ctrl+_`: Undo your last key press. You can repeat this to undo multiple times.


## Cutting and Pasting
### Bash includes some basic cut-and-paste features.

- Ctrl+W: Cut the word before the cursor, adding it to the clipboard.
- Ctrl+K: Cut the part of the line after the cursor, adding it to the clipboard.
- Ctrl+U: Cut the part of the line before the cursor, adding it to the clipboard.
- Ctrl+Y: Paste the last thing you cut from the clipboard. The y here stands for “yank”.

## Capitalizing Characters
### The bash shell can quickly convert characters to upper or lower case:

- Alt+U: Capitalize every character from the cursor to the end of the current word, converting the characters to upper case.
- Alt+L: Uncapitalize every character from the cursor to the end of the current word, converting the characters to lower case.
- Alt+C: Capitalize the character under the cursor. Your cursor will move to the end of the current word.

## Advanced Usage of find and ls
find . -ls | sort -k11,11 -> Sort the results on 11th Column
ls -S -> Sorted on filesize
ls -X -> Grouped on filetype
diff -u .bashrc <(ssh remote cat .bashrc)
find . -exec file {} +

grep -lR someVar | while IFS= read -r file; do
    head "$file"
done

find . -name '*.c' — Find files with names matching a shell-style pattern. Use -iname for a case-insensitive search.
find . -path '*test*' — Find files with paths matching a shell-style pattern. Use -ipath for a case-insensitive search.
find . -mtime -5 — Find files edited within the last five days. You can use +5 instead to find files edited before five days ago.
find . -newer server.c — Find files more recently modified than server.c.
find . -type d — Find directories. For files, use -type f; for symbolic links, use -type l.

