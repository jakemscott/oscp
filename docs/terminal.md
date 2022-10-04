# Terminal

A series of useful commands and actions to take full command of any terminal.

## Table of Contents

- [TMUX](#TMUX)
- [Jobs](#Jobs)
- [Files](#Files)
- [History](#History)

---------------------------------------------------------------------------

# TMUX

Everything needs (CTRL+b) before selecting anything below

##mSpliting the window

- % -> (left/right)
- " -> (top/bottom)

## Choosing panes

- o -> Shift panes
- ; -> Go back to last pane
- q -> Display pane numbers
- x -> Kill the pane

## Changing Windows

- 0-9 -> Choose window
- p -> Change to the previous window

---------------------------------------------------------------------------

# Jobs

Working with background jobs and bringing them to the fore when required:

- CTRL + Z -> background and suspend
- bg -> resume background
- & -> (at end of command = send to background)
- fg % [job no.] -> bring job number to the front
- jobs -> list all of the running background jobs in session

---------------------------------------------------------------------------

# Files

Quick ways to edit and interact with files only using the termianl (not comprehensive)

- less <filename> (allows for scrolling)
- sort <text file> | uniq > <new file> (sorts strings by newline and removes duplicates)
- tail -n +<number> <file> > <newfile> (removes beginning of file)

---------------------------------------------------------------------------

# History

Reuse an old command: `history | grep <parameter>`, `!!<choice>`

