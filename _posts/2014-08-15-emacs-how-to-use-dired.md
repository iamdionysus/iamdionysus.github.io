---
layout: post
title: To use emacs better, how can I do this and that with Dired?
---

C-x d (ido-dired) opens Dired. With this, I can [operate files inside emacs](https://www.gnu.org/software/emacs/manual/html_node/emacs/Operating-on-Files.html). Here are some useful tips to remember.

### Open Dired with only files which I want to see
When I open Dired, from the prompt, I can filter files that I want to see inside the directory. For example, ```Dired: c:/gabriel/*.md``` opens only markdown files within the directory.

### Delete files
* Delete multiple files
  * Select/Mark files which I want to delete by d where my cursor is.
  * Hit D to delete them all.
* Delete one file
  * Just hit D on the file which I want to delete

### Move files
* Move multiple files
  * Select/Mark files which I want to move by m where my cursor is.
  * Hit R to run dired-do-rename for those files.
  * From the prompt, type the directory where I want to move.
  
