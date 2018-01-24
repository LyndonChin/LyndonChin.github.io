---
title: vim
date: 2018-01-22 13:04:42
tags: vim
---

## Command-Line Mode
### Tip 27 Meet Vim's Command Line

* :[range]delete [x]
* :[range]yank [x]
* :[line]put [x]
* :[range]copy {address}
* :[range]move {address}
* :[range]join
* :[range]normal {commands}
* :[range]substitue/{pattern}/{string}/[flags]
* :[range]global/{pattern}/[cmd]
* :edit/:write
* :tabnew
* :split
* :prev/:next
* :bprev/:bnext
* :h ex-cmd-index

### Tip 28 Execute a Command on One or More Consecutive Lines
* :print
* :delete
* :join
* :substitute
* :normal
* :3 <=> 3G
* :3d <=> 3G dd
* :{start},{end}
* . = the current line
* $ = the end line
* % = all the lines
