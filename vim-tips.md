## Vim movements
- Ctrl+e -- Scroll 1 line down with fixed cursor
- Ctrl+y -- Scroll 1 line up with fixed cursor
- Ctrl+d -- Scroll halfpage down with cursor
- Ctrl+f -- Scroll fullpage down with cursor
- Ctrl+u -- Scroll halfpage up with cursor
- Ctrl+b -- Scroll fullpage up with cursor
- L -- Move Cursor to Last line
- M -- Move Cursor to Mid line
- H -- Move cursor to Top line of the screen

- zz -- Move current line w/ cursor to the middle of screen
- z. -- same as above
- zt -- Move current line w/ cursor to the top of screen
- zb -- Move current line w/ cursor to the bottom of screen
- 90% -- Move cursor to 90% of file

e -- move to end of word
ge -- reverse of above
^ or _ -- move to begining of line, non whitespace.
g_ -- reverse of above, i.e. end of line.
]m -- Go to end of method
% -- Matching brackets
+ -- Go down to line start
- -- Go up to line start
0,$ -- move at begining, end of a line

## Marks
mm -- mark m
'm -- First char of line with mark m
`m -- exactly at m -- ignore next --> `

### use case 
---
qqg_ld$j[^@q -- Macro to delete empty space at enf of the line.

Ctrl+v -- Select cursor (Visual Mode with cursor select)
Ctrl+e -- redo (Opposite of undo)
Ctrl+g -- Show Detils of open windows.
Ctrl+p -- Plugin.
Ctrl+g -- More details about current Line, Column and roq number and % of lines above.
Ctrl+f -- VI command history in command mode. runs in command mode
Ctrl+v I and Ctrl+v A -- Multiple cursors edits
Ctrl+o, Ctrl+i -- Switch between multiple opened files. o=out, i=in current file
Ctrl+o -- Last jump
CTRL+^ -- SWITCH BUFFERS
:e&

## Windows command
---
Ctrl+w - s -- vertical split
		 v -- hor split
		 i,j,k,l -- move between wondows
		 o -- Make my current one as Only one windows, ***
		 c -- close this window

## Insert Mode Completion 
--- 
Must be in insert Mode (i,a,c etc)
Ctrl+n (Also Ctrl+p) -- Auto completion of words
Ctrl+x, Ctrl+f -- Pops a list of filename
Ctrl+x s -- Spell check/suggestions
Ctrl+x Ctrl+v -- Command Mode suggestion/completion, on VIM commands.
Ctrl+x Ctrl+l -- Auto complete sentences

Command Mode shortcuts
Ctrl+r = -- evaluate exxpression (Mathematical)
Ctrl+r " -- Paste currently copied text

## Mathematics
---
Ctrl_A -- increase number under cursor
Ctrl_X -- decrease number under cursor
qaYpCtrl_Aq -- Copy pastes an increasing number below
100@a -- creates a list of increasing numbers


cIt -- 
cI{ -- change Inside Curly braces
cAt -- change Around the current tag.
cI" -- Change Inside Quotation marks.
cI< -- change inside the < tag
vI{ -- Visualize inside Paranthesis
dAt -- delete the Tag
vip -- select a paragraph
vip /sort -- selects a paragraph and sorts the lines. for eg sort a table

ctF -- Change (remove) till next F occurs from cursor
cf' -- Change (delete) everything from cursor to next singlequote '
c/oth -- Search oth and delete everything till that oth from current cursor
c6j -- Change (delete) 6 lines down.

f, F, t, T, /, ? -- Searches occurence. 
f -- takes to occurence
F -- Same as above, inverse order
t -- Before the occurence of the char
T -- Same as above, inverse order
; -- to move forward on char searches
, -- to move backward

d} -- delete entire method/paragraph, blanc line deliniated
% -- matches bracket.
`` Takes to last jump/char search -> ignore ``

cw, v/s ciw -- w is a option, i is a text object. ciw command can be repeated with .

- :r file1.txt
- :r! command
- :$r file
- :0r file
- :/pattern/r file
- `:normal d5j` -- Run this command in norm mode, delete 5 lines below, and reach next line.
- `:norm` -- runs normal mode command on command line, useful to perform repetable task/macro
- `:norm I<a href=" [^ A ><++> </a>` -- Adds href tag at selected text.
- `:map <leader>p 0yi":mupdf <C-R>" & disown <CR><CR>` -- Map Leader p command to - copy the text insode quote in current line, and execute mupdf command with that text on background with disown command so that vim should go on running, and hit carriage return

- nnoremap Q !!:$SHELL <cr> -- Map Ctrl_Q to run current line as shell command
- nnoremap <space>L yy:@" <cr> -- Run current line as Vim comand/normal mode

> -- indent
>j -- indent current line and one line below
x -- delete current char
r -- replace current char
R -- delete current word
tabe -- OPen new tab
E -- Open Explorer/directory view
b crazy.md -- Open file
:w !sudo tee %
10| -- Jump to 10th letter
vspl -- vertical split
nohl -- no highlight
set cursorline        # highlight current line
set cursorcolumn # highlight current column
:g/Hello/s/this/that -- Search fir hello, and replace this to that only on those lines.
:colorcheme 
c -- delete/change (selected text) command
ciw -- change inside word.
:%&g -- repeat the last command globally(on all lines)
g; -- Goto Last edited line
g, -- Reverse of g;
g# -- Search and move to last appearance of thw rord under cursor
g* -- Search and move to next appearance of thw rord under cursor
H -- Goto the top of the file
gwap -- Linting line width to 80 columns.
gqaw -- does same as before, cursor is moved to the begining of paraghaph.
1gqG -- Do same as above, for all paragraph. (You have to be at last paragraph)
:set spell
nmap <space>sp mm[s1z-`m -- ignore next --> `

J -- to Join next line with current with space.
gJ -- Join w/o space
gu -- Lower Case
gU -- Upper case
~ -- Toggle Case
= -- indent
> -- shift right
< -- shift left
w vs W -- 

"5p -- pastes line deleted in 5th stack/register. While last/recent one is saved on register 1.

## regex

- /s -> space
- /r -> return/enter/newline

## Search and Replace together

- /xxxx/d -- delete line containing xxx
- /yyy/+d -- delete line next to the one containing yyy 
- /pattern1/,/pattern2/d -- delete line between these 2 patterns, inclusing the line containing pattern
- /pattern1/+,/pattern2/-d

- %s/pattern//gn -- Count number of occurences of pattern. n in the end means -- no-op

- `.` -> redo last operation, in normal mode
- `.` -> Also stands for current line in comand mode
- `$` -> Last line in file
- `%` -> 1,$ -> Entire file
- ``t` -> position of mark t.

- /{pattern}[/] -- then next line where pattern matehces.
- ?{pattern}[/] -- then previous line where pattern matehces.
- :help range

- /l -- search l
- :%s//left/gc -- replace last searched item (l here) with left, g=globally, c=confirm before replacing each time.

## Tabs
---
gt
gT
:tabc
:tabe
:tabo

Terminal Buffer
---
:set shell=/usr/bin/zsh
:bo 15sp +te

vim args
---
`##` -> all args
- :args = **/*.py -- Add all py files in arg -> ignore **/**.

:vim /TODO/ ## -- Serch TODO in all files in my args list
:cn and :cp -- to navigate between args. Next and Previous
@: -- repeat that move (above)
:cdo s/TODO/DONE/g -- Change for every occurence of this and every line.

ctags
---
Ctrl_]
Ctrl_t

Jumps & Changes
---
:jumps
Ctrl_o
Ctrl_i
g:
g,
:changes


Marks 
-----
'

/pattern1/ mark a -- Marks a at the given pattern
'am/pattern2/-1 -- Move the marked line just before the pattern2

'[ and '] -- delimit/marks the region which was last edited
'< and >' -- delimit/marks the visual selected region
:help marks

Registers 
---------
Data is code
- " register
- 0 register -- 0-9 registers.
- ""p -- paste deleted text.
- `03wcWmy good^[j` -> from 0(begining of line) jump to 3words(3w) and change a Word(3W) to "my good", then escape(`^[`) and come to next line(j).
`""ay$` -> copy current line to register a
- @a -> replay the command/macro which was stored in register a
- @@ -> reoplay last run macro
- 6@@ -> replay the macro 6 times


`COM`  -> Count Operation Motion.

- :r! file
- :.!figlet -- run figlet with pushing this line as standard input as current line, and pastes the output on the cursor
- !!{cmd} -- normal mode
- noremap Q !!sh<CR> -- Thus runs the command in current line on she.l, with Q as a new command

### Random ,and Complex command
`+2,/^vim \*.scm/s/^#\(\d\+\)/\="#"  .strftime("%Y %b %d %X", submatch(1))/`
`-16,-14cp.`

## Macros -- Record and replay (Can use any character in place of q below)
- q<name of macro> for eg
- qq -> record a macro and save it to q
- q -> stop recording Macros
- @q -> replay macro q
- 4@q -> Replay the macro 4 times

## Argument List
- :arg `find lib spec -name '*.rb'` -> put output array of shell command on the args list.
- :args -- gonna print out all the parameters/arguments
- :argdo :%s/Math/MyMath/g | w -> Runs the Macro/substitution on arr the args(files)

---
# Plugins
#### - Ctrl+p -- To do fuzzy search and open files from a huge number of files

#### - Surrounding 
ds" -- Delete surrounding double quotes
cs'" -- change Surrounding single quote to double
ysiw" -- add surrounding double-quote around ineer word
cst<h2> -- can change surrounding tag lets say <p> to <h2>

#### - ReplacewithRegister
yiw -- copy the word
griw -- Will replace the inner word with buffer word (go replace)

TitleCase
gti' -- Toggle case inside '

#### - goSort
gsip -- sort inner paragraph

#### - System-Copy
cpip -- copies to system clipboard

#### - Inner Indent
cmii -- comment inner indent

#### - Entire
dae -- delete entire thing
cmae -- comment entire thing

## Someone's cool vimrc file -- 

" 2) force minimun window width
set winwidth=110

" 5) better searches
set hlsearch
set incsearch
set ignorecase
set smartcase

nnoremap <CR> :nohlsearch<cr>
:e! -- to undo all changes done to file
:se nu rnu -- Set relative hybrid line numbers

:se colorColumn
:se colorLine

==
===
:sh -- to enter terminal
Ctrl_d -- go back to vim
==
===

Change Airline themes
:AirlineTheme 

Pasting into the system clipboard, from remote vim

\e\e]52;c;
(b64_data)
\x07\e\\`
-- ignore this `

Ω≈ç√∫˜µ≤≥÷åß∂ƒ©˙∆∆˚¬
… 
æœ∑´®†¥¨ˆøπ“‘
«
¡™£¢∞§¶•ªº–≠

set listchars=tab:»\ ,extends:›,precedes:‹,nbsp:·,trail:·

set listchars=tab:→\ ,eol:↲,nbsp:␣,trail:•,extends:⟩,precedes:⟨

set listchars=eol:§,tab:¤›,extends:»,precedes:«,nbsp:‡
set listchars=tab:»·, eol:↲, nbsp:␣, trail:•, extends:→, precedes:←

if has('gui_running')
  set list listchars=tab:▶‒,nbsp:∙,trail:∙,extends:▶,precedes:◀
  let &showbreak = '↳'
else
  set list listchars=tab:>-,nbsp:.,trail:.,extends:>,precedes:<
  let &showbreak = '^'
endif

# in zsh
bindkey -V

# in bash
set -o vi

:set laststatus=2
Ctrl_g -- show sttaus in bottom

## Fugitive tips
---
:diff
:diffget -- do
:diffput -- dp
:diffupdate -- du

Using mouse
:set mouse=n
:set ttymouse=xterm2

:resize +5 -- Increase by 5
:vertical resize 50 -- Make it 50%


-> Diagraph -> http://www.alecjacobson.com/weblog/?p=443


set path+=**					" Searches current directory recursively.
set wildmenu	

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => Colors
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
hi LineNr ctermfg=242
hi CursorLineNr ctermfg=15
hi VertSplit ctermfg=8 ctermbg=0
hi Statement ctermfg=3

:%!sort -k2,2r -> you could sort the entire file in reverse by the second column.

:'<,'>!awk '/vim/ {print $3}' -> You could print only the third column of some selected text where the line matches the pattern /vim/ 

:1,10!column -t -> You could arrange keywords from lines 1 to 10 in nicely formatted columns
:1,5!column -t -> Change line number 1-5 into columnar style formatting.


Folding
---
Folding
fdc
fen
fdm
zf
za
zi
zm, zM, zr, zR, zo, zc

path
---
gf -> goto file
:set path+=lib/
<c-w><c-f> -> open with a split
gd -> goto Declaration
