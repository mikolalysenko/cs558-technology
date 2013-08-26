CS558 Technology Lecture 3: git
===============================
In the last two lectures we covered JavaScript and node.js, and we also went over the basics of how to publish a module on npm.  The goal of this lecture is to discuss how to collaborate on modules more efficiently and even more importantly to introduce git, which is a powerful distributed version control system.

Introduction to git
===================
Git is a tool that lets you maintain multiple distributed versions of a collection of files (for example, the source code for a large program).  It was invented by Linus Torvalds, who is the creator of Linux and is used in the day-to-day maintence of the Linux kernel.  Learning to use git is much easier than learning a new programming language or environment, but it does require a bit of patience to get started.

## Installing git
Installing git is a bit different depending on what system you are using.  On Linux or Mac, the easiest way to do this is to install git using your package manager.  For example, on Debian/Ubuntu you could do:

```sh
sudo apt-get install git
```

On OS X, you can install it using [homebrew](http://brew.sh/) or port.  For example,

```sh
sudo brew install git
```

You can also download and install git from git-scm.org directly:

* [http://git-scm.com/downloads](http://git-scm.com/downloads)

On Windows, the easiest solution might be to use github's windows installer.  You can find this here:

* [http://windows.github.com/](http://windows.github.com/)

## Basic git usage
Once you have git set up on your system, you can use it to create a new repository

To create a new git repository in some directory, you just type:  `git init`:

```
> git init
Initialized empty Git repository in /Users/mikolalysenko/GitHub/git-demo/.git/
```

This will create a folder in the current directory called `.git` that contains all the data necessary to track the history of the repository.  To add files to the repository, you type `git add filename`.  For example, here is how you can add a README to your git reposotory:

```
> touch README.md
> git add README.md
```

Once you've marked the files you want to track in the current revision, you can commit your changes using git commit

```
> git commit -m "initial project version"
[master (root-commit) 8eaaebf] initial project version
 0 files changed
 create mode 100644 README.md
```

To check out the status of your repository, you can type in `git status`

```
> git status
# On branch master
nothing to commit (working directory clean)
```

This tells you what files have outstanding changes that you need to commit.  If you want to make sure that some files are explicitly not tracked by git, you can create a file called .gitignore that will explicitly prevent you from accidentally adding them to version control.  This can be useful for example to prevent git from accidentally indexing your node_modules/ folder.  Here is example .gitignore file:

```
# A comment line
node_modules/*
npm-debug.log
```

You can also move/rename files using `git mv` and remove files using `git rm`.  For example,

```
> touch test.txt

> git add test.txt

> git commit -m "test.txt"

[master fec59f0] test.txt
 0 files changed
 create mode 100644 test.txt

> git mv test.txt foo.txt

> git commit -m "renamed test.txt to foo.txt"

[master 1b8fa77] renamed test.txt to foo.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename test.txt => foo.txt (100%)

> git rm foo.txt

> git commit -m "removed foo.txt"

[master dfdcb07] removed foo.txt
 0 files changed
 delete mode 100644 foo.txt
```

If you accidentally add a file that you didn't mean to, you can unmark it using the reset command.  Here is a simple example,

```
> touch test.txt
> git add test.txt
> git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#	new file:   test.txt
#
> git reset HEAD test.txt
> git status
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	test.txt
nothing added to commit but untracked files present (use "git add" to track)
```

You can also add tags to your commits to keep track of things like version numbers for your project.  The way this works is you use the `tag` command:

```
> git tag -a 1.0 -m "version 1.0"
```

You can then quickly reference the tagged branch using its alias.  For example,

```
> git show 1.0
```

## Branching
The above details are more-or-less standard in any competent version control system.  However what makes git unique is its powerful and simple distributed branching model.  At any time 


Collaborating using github
==========================
GitHub 

## Setting up github

## Basic usage

## Other features

### Markdown

### Wikis

### Issue tracking

### Pull requests



References
==========
There are a large number of tutorials online that talk about how to use git in great detail.  One of the best of these is "Pro Git" which covers all the basics in great detail:

* [Pro Git](http://git-scm.com/book)

There are also a number of fun interactive tutorials to practice using git:

* [Learn git branching!](http://pcottle.github.io/learnGitBranching/)

The above reference is highly recommended and can be played as something like an interactive puzzle game.

