---
title: "Better Use Vim"
layout: post
date: 2020-02-20 16:00
img: vim.jpg
headerImage: True
tag: [Vim, Learning]
category: blog
author: ykx
description: These commands and shortcuts may help use Vim more efficiently.
---



## Commands

| command                        | function                                                     |
| ------------------------------ | ------------------------------------------------------------ |
| vim -O [filename1] [filename2] | open file1 & file2 simultaneously and split horizontally     |
| vim -o [filename1] [fliename2] | open file 1 & file 2 simultaneously and split vertically     |
| :s/old/new                     | replace old string with new string for the first case in the line |
| :s/old/new/g                   | replace old string with new string for every case in the line |
| :%s/old/new/g                  | replace old string with new string for every case in the file |



## Shortcuts

| key    | function |
| ------ | -------- |
| u      | undo     |
| ctrl+r | redo     |



## Multi-line Operations

#### Block Insert

1. Ctrl+v: select multiple lines 

2. I (capital i): insert mode

3. insert the words

4. Esc: after several seconds, the words are inserted to all selected lines

#### Block Delete

1. Ctrl+v: select contents in multiple lines (with j, i, k and l)

2. d: delete the selected contents