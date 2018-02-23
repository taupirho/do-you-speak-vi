A few useful commands to help you become fluent in the Unix text editor vi. Note this 
assumes know the basics already. Make sure you are in **command** mode of vi  
NOT in **insert** mode. If you don't know what that last sentence means, you don't know
the basics yet - get learning first!!

<h2>Useful vi commands</h2>

Crucial to the goal of becoming fluent in vi is the concept of marks. Marks are
simply a way of marking different sections or records of your buffer in vi so that you 
can refer to them in subsequent commands. Usually these commands will involve cutting 
and/or pasting. To mark a line of text use the **m** command followed by a letter in the range
**a** to **z** , for example  **ma** will mark the current line with the letter a. Note
that you won't see the characters 'ma' when you type this command (**if you do it means 
you are in INSERT mode!**), it’s just an internal label maintained by vi. 

**NB In the following examples, expressions in brackets may be typed instead of the
corresponding expression before the brackets. For example d(y)'a means you can
type d'a or y'a. The difference between deleting and yanking lines is that 
deleted lines are renmoved from the buffer whereas yanked lines are left in place.**


<h3>Cut and Paste between a marked line and the current line.</h3>

Goto the line you want to start cutting from. Type **ma** then move the cursor to
the line you want the cut to end at. Then for example you can ... 

**'a** - goto the start of the line marked with a

**\`a** - goto the cursor position marked with a

**d(y)'a** - deletes(yanks) all lines between mark a and the current line

**P( p)**  - put back deleted or yanked lines before(after) current line

**"xd(y)'a** - delete(yank) lines into a buffer named x, buffer x is
overwritten. Use capital X to append lines to buffer x keeping what's already 
in buffer x. This variant is useful when you want to cut and paste between
two or more opened files.


<h3>Cut and Paste between two marked lines.</h3>

Goto the start line to be marked. Type **ma**. Goto the end line to be marked. Type **mb**. Then
for example:-

**:'a,'b d(y)** - deletes(yanks) lines between the two lines marked with a and b

**:'a,'b mo(co) 5** - moves(copies) lines between marks to after line 5

**:'a,'b mo(co) 0(\$)** - moves(copies) lines between marks to top(bottom) of the file

**:'a,'b mo(co) .** - moves(copies) lines between marks to after current line

Of course you can do all of the above commands on numbered lines also, for example

**:13,20d** - deletes lines 13 through 20 inclusive

**:3,5 mo(co) 7** -  move(copy) lines 3 to 5 after line 7

**:.,+Nd** - delete current plus next N lines 

<h3>Finding/substituting text</h3>

**:s/abc/xzy/** - substitute first occurrence of abc with xyz on current line only

**:s/abc/xzy/g** - substitute all occurrences of abc with xyz on current line only

**:100,150s/abc/xyz/g** - substitute all abc's with xyz's between lines 100 and 150 only

**:'a,'bs/abc/xyz/g** - substitute all abc's with xyz's between marked lines

**:s/abc/\\U&/g** - capitalise all abc's on current line

**:5,/ABC/s/XYZ/TOM** - substitute the string XYZ with TOM between line 5 and the next line that contains the text ABC 

**:%s/^...X/...Y/g** - substitute all X's in 4th character position on all lines with a 'Z'

**g/abc/s/123/456/g** - on all lines containing abc substitute 123 with 456

**/(?)abc** - find next(previous) occurrence of abc starting from current cursor position

**n(N)** - find next(previous) occurrence of a search string

**:%s/abc/xyz/g** - substitute all abc's with xyz's in whole file


<h3>The remembered text in substitutions</h3>

The remembered text in substitution patterns is defined by the & character or
**\\n** where **n** is between 1 and 9.The & represents the last regular
expression found and the \\n variant is defined by any regular expression
contained within escaped brackets reading from left to right. For example in the
search expression **/\^X\\(.T\\)xyz\\(xxx\\)** where we are trying to find a line
beginning with X followed by any single character followed by a T followed by
xyz followed by 3 x's, the \\1 corresponds to .T and \\2 corresponds to xxx.
Some examples should clarify.

**:s/X/&123/** - change X to X123 in current line

**:s/.\$/X&/** - put an X before last character in current line

**:s/\\(.\*\\) HARRY \\(.\*\\)/\\2 \\1 HARRY/** - change a line containing DICK HARRY TOM to TOM DICK HARRY


<h3>Miscellaneous Commands</h3>

**:\#** - display current line number

**:map s 3dw** - map pressing s in command mode to be equivalent of deleting 3 words

**:map! s 3dw** - map pressing s in insert mode to be equivalent of deleting 3 words

**\^v** - allows entering of control characters such as escape

**NJ** - join N lines after current line to the end of current line

**!! O/S command** - put result of **operating system command** into current buffer at current
cursor position

**N\|** - goto character N of current line

**\@x** - runs the editor commands contained in the named buffer x

**"dp** - undo dth last delete (up to a max of nine) of 1 or more lines

**:set ic** - ignore case when searching text

**:set nows** - don't wrap around file when conducting searches

**:set number** - display line numbers

**:set** - display all options set

**:set all** - display all settable options

**cw** - change word

**c3l** - change next 3 characters only

**:map \#ndd** - map function key n to delete current line

**%** - find matching bracket

**i** - start input mode at current cursor position

**O** - open a new line above current one and start input mode

**Ndd** - delete N lines atrting at current line

**Ndw** - delete next N words

<h3>Moving about in the file  (in command mode)</h3>

**Nj(k)** - move cursor down(up) N lines

**Nh(l)** - move cursor left(right) N characters

**:G** - goto end of file

**:1** - goto start of file

**\$** - goto end of current line

**0** - goto start of current line

**:130** - goto line 130

**W** - move to start of next word

**\^F(^B)** - scroll forward(back) one screen

**\^D(^U)** - scroll forward(back) a half screen


<h3>The numbered buffers</h3>

vi automatically saves deleted (whole lines of) text into into nine numbered
buffers (1-9) which can be used to retrieve accidentally deleted text. The most
recent delete is in buffer number 1. To retrieve this use the command **"1p(P)** to
put back the line after(before) the current line. The dot command cycles through
each of the buffers in turn therefore a quick way to see all the contents of all
nine buffers is to use **"1p........**


<h3>Initialisation commands</h3>

We can put vi initialisation commands in a file called .exrc in our home directory. When vi 
is invoked it reads this file and the commands contained in it are executed and are in
force for the whole of the edit. A typical .exrc file might contain commands
such as

**:set ic** - ignore case

**:set nows** - no wrapscan

**:map rm 5dd** - map rm to delete 5 lines

**:set nomagic** - characters such as \*,? and . lose their special meaning

**:set number** - turn line numbering on
