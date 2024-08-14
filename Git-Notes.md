# Git Notes

Git Official Website: https://git-scm.com/

Git Pro Book: https://git-scm.com/book/en/v2

Git Pro Book on Github: https://github.com/progit/progit2

------

## Table of Contents

1. [Git](#Git)
2. [The Underlying Logic of Git](#The-Underlying-Logic-of-Git)
3. [Git Configuration](#Git-Configuration)
4. [Git Help](#Git-Help)
5. [The Basic Concept of Git](#The-Basic-Concept-of-Git)
6. [The Process of Generating a Snapshot](#The-Process-of-Generating-a-Snapshot)
7. [Git Basic Snapshot Commands](#Git-Basic-Snapshot-Commands)
8. [Tagging](#Tagging)
9. [Branch](#Branch)
10. [Integrate Branches](#Integrate-Branches)
11. [Working with Remote](#Working-with-Remote)
12. [Revision Selection](#Revision-Selection)
13. [Stashing and Cleaning](#Stashing-and-Cleaning)
14. [Rewriting History](#Rewriting-History)
15. [Reset Demystified](#Reset-Demystified)
16. [Github](#Github)
17. [Git Underlying Tools](#Git-Underlying-Tools)
18. [Installing Git](#Installing-Git)
19. [Ignoring Files](#Ignoring-Files)
20. [Viewing the Commit History](#Viewing-the-Commit-History)

------

# Git

Version control is a system that records changes to a file or set of files over
time so that you can recall specific versions later.

There are three types of Version control systems: Local Version Control System
(VCS), Centralized Version Control System (CVCS), Distributed Version Control
 system (DVCS).

Git is a distributed version control system. Every server and client each fully
mirror the repository, including its full history. Thus, if any server dies, it
can be restored from any of the client repositories or clones. Every clone is
 really a full bacKup of all the data.

------

## The Underlying Logic of Git

Git stores everything in its database not by file name but by the SHA-1 hash
value of files' contents or directory structure. A 40-character string composed
 of hexadecimal characters (0–9 and a–f) looks something like below:

24b9da6552252987aa493b52f8696cd6d3b00373

In Git, only the hash value of any commit, which represents a specific version
of your data, can be called a "Snapshot". In order to maintain the continuity
of data, the hash value of the previous snapshot is always stored in its
subsequent one, except the first. When you do actions in Git, nearly all of
them only add data to the Git database. It is hard to get the system to do
anything that is not undoable or to make it erase data in any way.

------

## Git Configuration

There are three levels of Git configuration files:

- The system config file, `/etc/gitconfig`, apllied to every user on the system.

- The global config file, `~/.gitconfig` or `~/.config/git/config`, specific to
  the user.

- The repository configure file, `.git/config`, in the Git directory of whatever
  repository you're currently working, specific to that single repository.

Each level overrides values in the previous level.

On defferent systems, the configuration files may located in different places.
 You can view all of your settings and where they are coming from using:

    $ git config --list --show-origin

**Git Configuring Commands**

git config [--level] name value

```
--local, --system, --global	/* Three levels of configurations.

-l, --list		/* List all configuration settings.
--add name value	/* Add a new configuration option.
--get name		/* Get the specified configuration value.
--default <value>	/* With --get, use default value when missing entry.

--unset name		/* Remove a variable.
--remove-section name	/* Remove a section.

-e | --edit		/* Edit the configuration file.

/* Set global configurations:

user.name "Your name"		/* Your identity.
user.email mail@example.com	/* Your email.

user.signingkey			/* GPG signing key.

/* Editor like : vim, emacs, or on a windows system: "'C:/Program
Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
core.editor
core.pager		/* default is less.
core.excludesfile	/* default is .gitignore file.

color.ui		/* true, false, default is auto.

core.autocrlf		/* Linux <LF>, Windows <CRLF>, MacOS <CR>
              true	/* before commiting, coverting line endings.
              input	/* No line endings converting.
              false	/* No line endings converting.

core.safecrlf		/* When end-of-line conversion is active,
              true	/* Check out line endings, if not ok, reject commit.
              false	/* Do nothing.
              warn	/* If not ok, just warn.

core.whitespace
                blank-at-eol	/* Warning spaces at the end of lines.
                blank-at-eof	/* Warning spaces at the end of files.
		space-before-tab	/* spaces before tabs at beginning.
		/* Default turn on blank-at-eol,blank-at-eof,space-beofre-tab.

core.quotePath		/* Whether quoting "unusual" characters in pathnames.
               true	/* Yes.
               false	/* No. Display Chinese characters correctly.

/* Set git command alias
alias.[short-command-name] "a-long-command-and-options"

init.defaultBranch main		/* Setting main as the default branch name.

pull.rebase		/* The default behavior of git pull command.
            true	/* Rebase when pulling.
            false	/* Fast-forward if possible, otherwise a merge commit.

commit.template	/path/filename	/* Customizing the commit template.
```

**Note**:

Since Git might read the same configuration variable value from more than one
file, it’s possible that you have an unexpected value for one of these values
and you don’t know why. In cases like that, you can query Git as to the origin
for that value, and it will tell you which configuration file had the final say
in setting that value:

```
$ git config --show-origin rerere.autoUpdate
file:/home/johndoe/.gitconfig	false
```

A configuring example, the first thing you should do after installing Git.

```
$ git config --global user.name 'yourname'
$ git config --global user.email your@email.com
$ git config --global core.editor your-favourite-editor

$ git config --global core.quotePath false
$ git config --global pull.rebase false

$ git config --global alias.logg 'log --oneline --decorate --graph --all'
```

------

## Git Help

If you ever need help while using Git, there are three equivalent ways to get
the comprehensive manual page (manpage) help for any of the Git commands:

**Git Help Commands**

```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

------

## The Basic Concept of Git

**Snapshot**

Git Stores data as changes to a base version of each file;
Git Stores data as snapshots of the project over time;
Git thinks about its data more like a stream of snapshots.

```
              Recording over time as a stream of snapshots
//====================================================================>

    Snapshot    98ca9      34ac2      f30ab      .....      |  Data
    Version     Version 1  Version 2  Version 3  Version n  |
    File        files      files      files      files      |
```

**Branch**

A Git Branch is part of all of snapshots. At least one will be required, the
default branch is `master` or `main`, and can be customized.

**Repository**

A Git Repository is all of data of one project. There are two types of
repositories:

- Remote repositories, not only can be on a git repository hosting server like
  github, Azure-DevOps, etc, but can be on your local machine as well. The
  default remote repository name is `origin`, and can be customized.

- Local repositories, a directory `.git`, is normally located in your project
  directory, when you initialize your project directory or clone from a remote
  repository with Git tools. It can be customized as well.

------

## The Process of Generating a Snapshot

You will see four main status of files and three corresponding storage areas.

### Four Status of Files

- **Untracked** means that Git detects new files in your working directory, and
  is ready to stage.

- **Modified** means that files of its previous version have been changed, and
  is ready to stage.

- **Staged** means that you have marked modified files in its current version,
  and is ready to commit to your local database. Staged files can be modified
  and staged again. `Cached` has the same meaning.

- **Committed** means that the data of that version is stored in your local
  database as a snapshot.

### Three Storage Areas

- **The working tree** is a single checkout of one version of the project.
  These files are pulled out of the compressed database in the Git directory
  and placed on disk for you to use or modify.

- **The staging area** is a file, generally contained in your Git directory,
  that stores information about what will go into your next commit. Its
  technical name in Git parlance is the “index”, but the phrase “staging area”
  works just as well.

- **The Git directory** is where Git stores the metadata and object database
  for your project. This is the most important part of Git, and it is what is
  copied when you clone a repository from another computer.

**The relationship between file status, storage areas and snapshots**

```
            Working Directory              Stageing Area     .git directory

  |----------------------------------|--------------------|----------------|

       Untracked          Modified           Staged            Snapshots
       or Committed

  |-Add new files------------------->|                    |      8932c   ^
  |                  |-Stage files-->|                    |      52038   |
  |-Edit files------>|               |                    |      79f22   |
  |                  |-stage Fixes-->|                    |      ff692   |
  |                  |               |-Commit files------>|      8b3c2   |
                                                   A snapshot--> 32f74   |
  |<---------------------------------Checkout the project-|
  |-`.gitignore` files to avoid to track unwanted files-->|  Branch Master
```

------

## Git Basic Snapshot Commands

**1. Create or Clone a Repository**

```
init	/* Create an empty git repository or reinitialize an existing one.
     --bare		/* Create a bare repository as a remote repo.

/* Clone a remote repository with same or desired directory name.
clone <remote-repo> [dir]
```

**2. Basic Operations of Four Status and Three Areas**

```
add			/* Add file contents to the index (staging areas).

commit			/* Record changes to the repository.
       -v, --verbose	/* Show unified diff in commit message template.
       -m <msg>, --message=<msg>	/* Use <msg> as the commit message.
       -a, --all	/* Stage all files and commit.

mv file_from file_to	/* Move or rename a file, a directory, or a symlink.

rm			/* Remove files form the working tree and the index.
   -r <directory>	/* Recursively remove a directory.
```

**3. Inspect File Status and Differentials**

```
status			/* Show the working tree status.
       -s, --short	/* Give the output in the short-format.

diff		/* Show changes between the last commit and the working tree.
     --staged	/* Show changes between the last commit and staged areas.
     --cached		/* It is a synonym of --staged.
```

**Note**: There are other Diff tools including graphical and external programs.
Run `git difftool --tool-help` to see what is available on your system.

**4. Reverse the Process**

```
checkout -- <file>...		/* Discard changes in working directory.
reset HEAD <file>...		/* Unstage unwated files.
commit --amend			/* Redo the top commit.

restore --staged <file>...	/* Unstage unwated files.
restore <file>...		/* Discard changes in working directory.

Note: Git version 2.23.0, git restore is an alternative to git reset.
```

**Note**: the backslash `\` in front of the *. This is necessary because Git
does its own filename expansion in addition to your shell’s filename expansion.

An example to illustrate the process of recording changes to local database.

```
$ mkdir your-project	/* Create your project directory.
$ cd your-project

$ git init		/* Create basic database structure .git direcotry.
Initialized empty Git repository in
/home/admin/Desktop/repository/your-project/.git/

$ touch README.md			/* Create a new file.
$ git add README.md			/* Stage the new file.
$ git commit -m 'First commit'	/* Commit with a message `First commit`.

$ git logg	/* View commit history. `logg` is a git alias.logg setting.
* cee25cb (HEAD -> master) First commit

$ echo 'This is a README file.' > README.md	/* Modify it.
$ git add README.md				/* Stage it.
$ echo '2021-11-10' >> README.md		/* Modify the file staged.
$ touch a b c					/* Create three files.

$ git add .	/* Stage README.md, `.` means all files in the current dir.
$ git status		/* Check out file status, and find 3 unwanted files.

$ git reset HEAD a b c				/* Unstage 3 files.
$ git commit -m 'Second commit'			/* Commit it.

$ git logg					/* View the log history.
* 14c6f6f (HEAD -> master) Second commit
* cee25cb First commit
```

**Note**: You should always check out file status as possible before your next
operation. And should figure out the meaning of file status report information.

An example to understand simplifed files status.

```
$ git status -s

__________  The left-hand column indicates the status of staging area.
| ________  The right-hand column indicates the status of the working tree.
||
 M README		/* Right M indicates modifed but unstaged files.
MM Rakefile
A  lib/git.rb		/* Left A indicates new and staged files.
M  lib/simplegit.rb	/* Left M indicates modified and staged files.
?? LICENSE.txt		/* ?? indicates new files that aren't tracked.

/* MM indicates the file was modifed and staged, and then modified again.
```

------

## Tagging

Typically, tagging specific points in a repository’s history to mark release
points (v1.0, v2.0 and so on).

Git supports two types of tags: lightweight and annotated.

A lightweight tag is very much like a branch that doesn’t change — it’s just a
pointer to a specific commit.

Annotated tags, however, are stored as full objects in the Git database.
They’re checksummed; contain the tagger name, email, and date; have a tagging
message; and can be signed and verified with GNU Privacy Guard (GPG).

By default, the git push command doesn’t transfer tags to remote servers. You
will have to explicitly push tags to a shared server after you have created
them.

git push <remote> --tags will push both lightweight and annotated tags. There
is currently no option to push only lightweight tags, but if you use git push
<remote> --follow-tags only annotated tags will be pushed to the remote.

**Git Tag Commands**

```
git

tag			/* Operating tags. without options, list tags.
    <tagname>		/* Make a lightweight tag, or with commit checksum.

    /* Make an unsigned, annotated tag, need a message.
    -a, --annotate <tagname>
    -m, --message <message>	/* Provide an annotated tag message.
    -s, --sign			/* Make a GPG-signed tag.

    -v, --verify <tagname>	/* Verify signed tags.

    -l, --list			/* List tags.

    /* Delete tags locally. This doesn't delete remote tags.
    -d, --delete

Note: When creating tags, wether lightweight or annotated tag, you can give the
commit checksum to tag a specific point by puting it after <tagname>.
```

------

## Branch

Git Branches are very lightweight. The default branch `master` will be
available, after completing the initial commit. A new branch based on any
commit from another branch can be created, deleted, or switch between them
easily as you want. Every branch and tag is just a mark with the specific
commit, and indicates the data it ownes.

There is a HEAD pointer that always points to the branch you are currently on,
and generally the top of it, which makes things easy to see where you are now,
while you switch between branches.

For example, making some changes to your production environment such as writing
a part of your document or developing new App features, you should always do
them on a new branch. After reviewing or testing it, you can easily integrate
from it into your production environment branch, such as the master or main
branch.

**Git Branch Commands**

```
git

/* With no arguments, list all local branches. Imply --list option.
branch
       --list		/* List all local branches.
       -v		/* List all local branches with more information.
       --all		/* List all local and remote branches, imply --list.

       /* List branches that are merged into the branch you're on.
       --merged
       --no-merged	/* List branches which haven't been merged in.

                   /* Specify the branch which has been or not merged into.
                   <branch>

       <branch-name>			/* Create a branch.

       /* Rename a branch locally, but this doesn't change the remote.
       -m, --move <old-branch> <new-branch>
       -d, --delete <branch-name>		/* Delete a branch.

checkout <branch-name>		/* Switch to the specific branch.
         -b <new-branch-name>	/* Create a branch and switch to it.

switch <branch-name>	/* Switch to the specific branch. Git version 2.23.
       -c, --create <new-branch-name>	/* Create a branch and switch to it.

Note: `-` means the previous branch. You can use `git checkout -` to switch
back to the previous branch.
```

An example to understand how to create, delete or switch between branches.

```
$ mkdir new-project
$ cd new-prject

$ git init			/* Initialize the new-project directory.
$ touch a b c d
$ git add a
$ git commit -m 'a'		/* Every file is a commit and so on.
...

$ git logg			/* We should get something like this.
* 6417fdf (HEAD -> master) d
* 45f3d77 c
* 289a3e2 b
* 16c9ce9 a

$ git branch b01 289a3e2	/* Create branch b01 bash on commit 289a3e2.
$ git branch b02 45f3d77	/* Create branch b02.
$ git logg			/* In commit history, there are 3 branches.
* 6417fdf (HEAD -> master) d	/* The HEAD points to the master branch.
* 45f3d77 (b02) c
* 289a3e2 (b01) b
* 16c9ce9 a

/* Switch to the branch b01 and checkout the data it owns.
$ git checkout b01
$ git logg
* 289a3e2 (HEAD -> b01) b
* 16c9ce9 a

$ touch e f g			/* Create three files.
...				/* Every file is a commit, and so on.

$ git checkout b02		/* Switch to the branch b02.
$ touch h i j			/* Do something like on the branch b01.
...

$ git logg			/* Finally we get something like this.
* 11ad5bf (HEAD -> b02) j
* bf6d5d8 i
* 28dc4dc h
| * c14f5cb (b01) g
| * aa50c41 f
| * e4a8af9 e
| | * 6417fdf (master) d
| |/
|/|
* | 45f3d77 c
|/
* 289a3e2 b
* 16c9ce9 a

$ git checkout master
$ git branch bug		/* Create a new branch `bug` bashed on master.
$ git branch --move bug snake	/* Rename the branch `bug` to `snake`.
$ git branch
  b01
  b02
* master			/* On the master branch currently.
  snake

$ git branch --delete snake	/* Delete the branch `snake`.
$ git branch
  b01
  b02
* master
```

------

## Integrate Branches

In Git, there are two main ways to integrate changes from one branch into
another: the merge and the rebase.

### Git merging

If a branch bases on the top of another, a fast-forward merge can be performed.
Otherwise a merge commit will be created. If there is a merge conflict, the
merging will be interrupted. You can abort the merging to return to the
previous status, or fix the conflict and then stage, commit to complete the
merging.

**Fast-forward merge**

```
   a---b---c---d---e  < top    a---b---c---d---e  < top
           ^                                   ^
           master                              master
```

**Git Merging Commands**

```
git

/* Merge the specific branch into the branch you're on.
merge <branch-name>
                    -m <msg>		/* Merge with a message.

      /* Conflicts interrupt the merge.
      --abort		/* Revert back to your stage before you ran the merge.
      /* After staging all resolved conflicts, complete the merging.
      --continue
```

**Note**: After completing the merging, you can drop the merge commit by using
`git reset --hard HEAD~` if you want.

An example to understand how to merge.

```
/* We use the new-project with three branches that has been created before.

         e---f---g <--- b01            e---f---g <--- b01
        /                             /         \
   a---b---c---d        master   a---b---c---d---k    master
            \                             \
             h---i---j  b02                h---i---j  b02

$ git checkout master
$ ls
a b c d

$ git merge b01			/* Merge the branch b01 into master.
$ git log --oneline
bd67bf2 (HEAD -> master) k	/* In the merge template, just enter a `k`.
c14f5cb (b01) g
aa50c41 f
e4a8af9 e
6417fdf d
45f3d77 c
289a3e2 b
16c9ce9 a
```

------

### Git Rebasing

Rebasing is just like reapply commits on top of another branch. All of
reapplying commit checksums will be changed, since the contents of those
snapshots have been changed, but the contents of those files are the same as
before rebasing.

**Git Rebasing Commands**

```
git

rebase <to-branch>		/* Reapply commits to <to-branch>.

       /* Reapply <break-branch> based on <base-on-branch> to <to-branch>.
       --onto <to-branch> <base-on-branch> <break-branch>

       /* After staging all resolved conflics, complete the rebasing.
       --continue

       /* If rebasing stopped by conflict, --abort to quit it.
       --abort
```

An example to understand how to do basic rebase.

```
/* We continue the new-project that has been created before.

      e---f---g <--- b01            e---f---g <--- b01
     /         \                   /         \
a---b---c---d---k    master   a---b---c---d---k---h'---i'---j'   b02  rebase
         \                                    ^ fast-forward >
          h---i---j  b02                      master                  merge

$ git checkout b02		/* Switch to the branch b02.
$ git rebase master		/* Rebase the branch b02 onto the master.
First, rewinding head to replay your work on top of it...
Applying: h
Applying: i
Applying: j

$ git logg
* 78641d4 (HEAD -> b02) j	/* Checksum has been changed.
* d841fcc i
* 7650b4f h
*   bd67bf2 (master) k
|\
| * c14f5cb (b01) g
| * aa50c41 f
| * e4a8af9 e
* | 6417fdf d
* | 45f3d77 c
|/
* 289a3e2 b
* 16c9ce9 a

$ git checkout master
$ git merge b02
Fast-forward
...

$ git logg
* 78641d4 (HEAD -> master, b02) j
* d841fcc i
* 7650b4f h
*   bd67bf2 k
|\
| * c14f5cb (b01) g
| * aa50c41 f
| * e4a8af9 e
* | 6417fdf d
* | 45f3d77 c
|/
* 289a3e2 b
* 16c9ce9 a
```

Two examples to understand how `rebase --onto` works.

```
git rebase --onto master next topic

                   f---g---h  topic             f'---g'---h'  topic
                  /                            /
             d---e  next                       | d---e  next
            /                                  |/
   a---b---c  master                   a---b---c  master

Note: This is only useful when topic doesn't depend on next.


git rebase --onto new-topic~5 new-topic~3 new-topic

   e---f---g---h---i---j new-topic     e---h'---i'---j' new-topic

/* Remove the part of new-topic which you don't want.
```

------

## Working with Remote

All of examples showed above only store data locally. Let's have a view of the
abstract concept of remote repoes, before introducing how to operate it.

1. Remote repoes' data are almost the same as the .git directory in working
   directory.

2. Remote repoes don't record how local and remote branches connected or
   tracked each other.

3. After cloning a remote repo, all of data and remote branches will be
   download or fetched, but only the local branch master or the branch with the
   initial commit will be created and track this remote branch. you can create
   any local branch based on any remote branch with same or different names.
   This procedure means the local branch has tracked the remote branche or made
   synchronization between them, as they marke the same commit checksum. Every
   branch can be done like this.

4. Otherwise, let's say you have a local repo with some commits and a new empty
   remote repo. After adding the remote repo locally, you should use `git push
   --set-upstream <remote-repo> <local-branch>:<remote-branch>` command to
   track remote branch and the data of this branch will be pushed to it at the
   same time. Generally, after tracking the branch master, and then `git push
   <remote-repo> --all` command will make all local branches track remote
   branches and their data will be synchronized. Alternatively, just using
   `push --set-upstream` to track every branch.

5. When collaborating with others, things seem to become tricky. Before pushing
   to remote, you must always fetch commits of others first. And then you can
   either merge those commits by using `merge <remote-repo>/branch`, or create
   a new branch based on the remote branch to rebase your own commits. Don't
   forget that you can always create multiple branches which base on one branch
   or the same commit. This means you have multiple backups of a branch and you
   can always do testing operations on them. After having been fully tested,
   you can merge tested branch into the important branch, and it will always be
   a fast-forward merge.

**Note**: Git remote repository third party hosted options,
https://git.wiki.kernel.org/index.php/GitHosting

```
  Local                                                           Remote

  Repo      git remote add <shortname> <url>                      Repo
  Branches  git push -u <shortname> <localbranch>:<remotebranch>  Branches
  master    git push <shortname> <branch>                         master
  b01       git fetch <shortname> <branch>                        b01
  b02       git branch <localbranch> <shortname>/<remotebranch>   b02
```

**Git Commands to interact with remote**

```
git

clone <url> [<desired-name>] 	/* Clone remote repoes with desired names.

remote		/* With no options, displays remote repo's shortnames.
       -v, --verbose		/* Show verbose information.

       /* Show the desired remote <repo-name> information.
       show <repo-name>
       add <shortname> <repo-url>	/* Add remote repos.
       rename <old> <new>		/* Rename remote repos' names.
       remove <repo-name>		/* Remove a remote repo.

fetch <remote>		/* Download objects and refs from remote repo.
               -t, --tags		/* Fetch all tags.
      --all				/* Fetch all remotes.


/* Push local data to remote and add upstream tracking reference.
push
     /* Set which of local branches tracking which of remote branches.
     -u, --set-upstream <remote> <local-branch>:<remote-branch>

     <remote> [local-branch][:][remote-branch]	/* Push branch to remote.

     --all			/* Push all branches to the remote.
              --tags		/* Push all tags to the remote.
              --follow-tags	/* Only push annotated tags.

              --delete <branch>	/* Delete unwanted remote branches.

              --delete <tagname>	/* Delete remote tags.

                       /* This option means ovewrite the remote forcely.
                                 --force
/* This command is equivalent to fetch and then merge <remote>.
pull <remote> <branch>
```

An example to understand how to work with remote repoes.

```
/* Continue the new-project that has been created before.

/* You must create a remote repo on a hosting server like github, Azure
Dev-Ops, etc.

/* Add the remote repo with it's shortname `origin`.
$ git remote add origin https://github.com/username/new-project

/* Set local master tracking remote master and push its data.
$ git push -u origin master:master
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.

/* Now you can push all branches to the remote, after tracking the branch with
the initial commit. Other branches will be tracked automatically with same
names.
$ git push origin --all
 * [new branch]      b01 -> b01
 * [new branch]      b02 -> b02

$ git branch --all		/* Show all of local and remote branches.
  b01
  b02
* master
  remotes/origin/b01
  remotes/origin/b02
  remotes/origin/master


/* Now we try a remote repo at local disk.

$ cd ..				/* Get out of the new-project directory.

$ mkdir public-new-project
$ cd public-new-project
$ git init --bare	/* Initialize this directory as a bare remote repo.

$ cd ../new-project		/* Go back to your new-project directory.

/* Add the remote repo which has been created in your local disk.
$  git remote add public-new-project /path/public-new-project

/* Tracking every branch separately.

$ git push -u public-new-project master:main
 * [new branch]      master -> main
Branch 'master' set up to track remote branch 'main' from 'public-new-project'.

$ git push -u public-new-project b01:bugfix
 * [new branch]      b01 -> bugfix
Branch 'b01' set up to track remote branch 'bugfix' from 'public-new-project'.

$ git push -u public-new-project b02:new-feature
 * [new branch]      b02 -> new-feature
Branch 'b02' set up to track remote branch 'new-feature' from
'public-new-project'.

$ git branch --all
  b01
  b02
* master
  remotes/origin/b01
  remotes/origin/b02
  remotes/origin/master
  remotes/public-new-project/bugfix
  remotes/public-new-project/main
  remotes/public-new-project/new-feature

/* Delete the remote branch bugfix of the public-new-project repo.
$ git push public-new-project --delete bugfix
 - [deleted]         bugfix


/* Now we delete the new-project directory, as there is a full backup on the
remote.

$ cd ..
$ rm -rf new-project

/* Clone the remote repo to the directory web-project. If not specifying the
directory, a directory with the same name of the remote repo will be created.
$ git clone https://github.com/username/new-project web-project

$ cd web-project
$ git branch --all	/* Only local branch master has been created.
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/b01
  remotes/origin/b02
  remotes/origin/master

/* Created the local branch b01 based on the remote branch origin/b01 and
switch to b01.

$ git checkout -b b01 origin/b01
Branch 'b01' set up to track remote branch 'b01' from 'origin'.
Switched to a new branch 'b01'
```

------

## Revision Selection

```
         o---p---q  next               Type of References:
        /          ^^2
       /   f---g---h  topic            Single Revisions: git show 1c361
      /   /         \                  Branches: git show topic
 a---b---c---d---e---i---j---k HEAD    Reflog Shortnames: git show HEAD@{5}
     ~6  ~5  ~4 ^^^  ^^  ^
     1c361                   master

/* HEAD^ means the first parent. But HEAD^^2 means parent>parent>parent in 2nd,
the branch which merge in.
Ancestry: git show HEAD^^2
/* HEAD~ == HEAD^; HEAD~2 == HEAD^^, but only in the branch which merge into.
          git show HEAD~5

/* Show what is in the next branch that hasn't yet been merged into the master.
Ranges: git log master..next

/* More than two branches can be specified. The three below is equivalent.
Multiple Points: git log master..next == ^master next == next --not master

/* Specifys all the commits that are reachable by either of two references, but
not by both of them.
Triple Dot: git log master...next

/* Shows which side of the range each commit is in by preceding a < or >.
            git log master...next --left-right
```

------

## Stashing and Cleaning

Stashing takes the dirty state of your working directory — that is, your
modified tracked files and staged changes — and saves it on a stack of
unfinished changes that you can reapply at any time (even on a different
branch).

```
git

stash [push]		/* To stash the dirsty state.
      list		/* To list all stashing.
      apply [@{number}]		/* To reapply a stashing in the list.
      drop [@{number}]		/* To drop a stashing.

      branch <new-branch>	/* To create a branch from a stash.

clean			/* Cleaning your working directory.
      -d		/* Remove untracked directories.
      -f		/* forcelly.

      /* Don’t actually remove anything, just show what would be done.
      -n, --dry-run
```

------

## Rewriting History

It's very convinient to use `git rebase -i HEAD~?` rewrite commit history. Just
edit the commit history list and follow the instruction.

```
/* Delete a file in history and rewrite commit history. Glob-pathname can be
used.
git filter-branch --tree-filter 'rm -f filename' HEAD

/* Replace Identity information in commit messages.
$ git filter-branch --commit-filter '
        if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ];
        then
                GIT_AUTHOR_NAME="Scott Chacon";
                GIT_AUTHOR_EMAIL="schacon@example.com";
                git commit-tree "$@";
        else
                git commit-tree "$@";
        fi' HEAD
```

**Note**: git filter-branch has many pitfalls, and is no longer the recommended
way to rewrite history. Instead, consider using git-filter-repo, which is a
Python script that does a better job for most applications where you would
normally turn to filter-branch. Its documentation and source code can be found
at https://github.com/newren/git-filter-repo.

------

## Reset Demystified

```
Reset Commands           Two-Dimensional Position of HEAD    Go Back to Status

reset [--mixed] HEAD~2          HEAD-------------------->    The working tree
                                                                             ^
reset --soft HEAD~1                    HEAD------------->    The staged area |
                                                                             |
reset --hard HEAD~                            HEAD------>    The snapshot    |
                                                                        ^    |
Go Back to Commit        78641  d841f  7650b  bd67b  aa50c   Snapshots  |    |
                         ~3     ~2     ~1     ~      HEAD               |    |
                                                                        |    |
                                                                        |    |
                         git checkout -- <file>... -- discard changes ---    |
                               git reset HEAD <file>... -- unstage changes ---

/* Extract unedited and edited status of desired files in specified snapshot
and put them in the working tree and staged area separately, but don't
overwrite the newest version of those files in the working tree.
git reset [commit-hash] -- file...

git reset --soft [commit-hash] -- file...
  \                                                                        /
   ------------------------------------------------------------------------
                                        ^
git reflog     /* Show all commands history with the reference hash.
```

------

## Github

You should always use the security features of Github, such as Two-factor
authentication, SSH and GPG keys, Security & analysis, branches protection and
Notifications.

Branch protection:
```
Any Repo Settings --> Branches --> Branch name pattern:
The simplest way is just to enter the branch name you want to protect.

Rules applied to everyone including administrators:
Don't allow force pushes and deletions.
```

### The GitHub Flow

Here’s how it generally works:

1. Fork the project.
2. Create a topic branch from master.
3. Make some commits to improve the project.
4. Push this branch to your GitHub project.
5. Open a Pull Request on GitHub.
6. Discuss, and optionally continue committing.
7. The project owner merges or closes the Pull Request.
8. Sync the updated master back to your fork.

You can clone the forked project to you computer and then push your changes of
topic. The Pull Request is like a platform for communication. After discussing,
the project owner can choose to merge or rebase your commit. This step he also
can do on his local repo and then push to update. This has the same result,
which the Pull Request will be completed. Finally you can sync the fork branch.

------

## Git Underlying Tools

Basically, Git stores its data as various types of objects. There are a lot of
this type of tools in Git. Sometimes we want to view the contents of the
previous commits. There are two tools which can do this kind of thing.

```
git

/* Provide content or type and size information for repository objects.
cat-file
         -t <object>	/* Show the object type identified by <object>.
         -s <object>	/* Show the object size identified by <object>.
         -p <object>	/* Print the contents of <object> based on its type.
         <type> <object>	/* Same as above.

ls-tree <object>	/* List the contents of a tree object.
```

An example to understand how to explore objects.

```
$ mkdir doc-project
$ cd doc-project
$ git init

$ echo 'hello world' > README.md
$ git add .
$ git commit -m "initial commit"

$ echo 'Hello World!' > README.md
$ git commit -a -m "format words"

$ git logg
* 9357c3c (HEAD -> master) format words
* c726af5 initial commit

$ git cat-file -t 9357c3c
commit

$ git cat-file commit 9357c3c
tree ad123a385f6c7d11ca711427e575d1530239caad
parent c726af540223cf2f4433ba2849a2a7c3d8c530a3
author JohnSnow <JohnSnow@outlook.com> 1637380522 -0500
committer JohnSnow <JohnSnow@outlook.com> 1637380522 -0500

format words

$ git ls-tree ad123
100644 blob 980a0d5f19a64b4b30a87d4206aade58726b60e3	README.md

$ git cat-file -t 980a0
blob

$ git cat-file blob 980a0
Hello World!

/* All of objects are stored under their SHA-1 names inside the Git directory:
$ find .git/objects/
.git/objects/
.git/objects/pack
.git/objects/info
.git/objects/9d
.git/objects/9d/736b0af62e2c337764475d7fb8afe227924fc4
.git/objects/bd
.git/objects/bd/15470645f1beb99b12a4bcf08b19b80753d6f9
.git/objects/c7
.git/objects/c7/26af540223cf2f4433ba2849a2a7c3d8c530a3
.git/objects/98
.git/objects/98/0a0d5f19a64b4b30a87d4206aade58726b60e3
.git/objects/ad
.git/objects/ad/123a385f6c7d11ca711427e575d1530239caad
.git/objects/93
.git/objects/93/57c3cc1a8bc67c56f6f7b1ab497cba1c15168f
```

------

## Installing Git

### Installing on Linux

If you’re on Fedora (or any closely-related RPM-based distribution, such as
RHEL or CentOS), you can use dnf:

    $ sudo dnf install git-all

If you’re on a Debian-based distribution, such as Ubuntu, try apt:

    $ sudo apt install git-all

### Installing on Windows

Download and install  https://git-scm.com/download/win.

### Installing from Source

If you do want to install Git from source, you need to have the following
libraries that Git depends on: autotools, curl, zlib, openssl, expat, and
libiconv. For example, if you’re on a system that has dnf (such as Fedora) or
apt-get (such as a Debian-based system), you can use one of these commands to
install the minimal dependencies for compiling and installing the Git binaries:

```
$ sudo dnf install dh-autoreconf curl-devel expat-devel gettext-devel \
  openssl-devel perl-devel zlib-devel

$ sudo apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev \
  gettext libz-dev libssl-dev
```

In order to be able to add the documentation in various formats (doc, html,
info), these additional dependencies are required:

```
$ sudo dnf install asciidoc xmlto docbook2X
$ sudo apt-get install asciidoc xmlto docbook2x
```
**Note** Users of RHEL and RHEL-derivatives like CentOS and Scientific Linux
will have to enable the EPEL repository to download the docbook2X package.

If you’re using a Debian-based distribution (Debian/Ubuntu/Ubuntu-derivatives),
you also need the install-info package:

    $ sudo apt-get install install-info

If you’re using a RPM-based distribution (Fedora/RHEL/RHEL-derivatives), you
also need the getopt package (which is already installed on a Debian-based
distro):

    $ sudo dnf install getopt

Additionally, if you’re using Fedora/RHEL/RHEL-derivatives, you need to do
this:

    $ sudo ln -s /usr/bin/db2x_docbook2texi /usr/bin/docbook2x-texi

due to binary name differences.

When you have all the necessary dependencies, you can go ahead and grab the
latest tagged release tarball from several places. You can get it via the
kernel.org site, at https://www.kernel.org/pub/software/scm/git, or the mirror
on the GitHub website, at https://github.com/git/git/releases. It’s generally a
little clearer what the latest version is on the GitHub page, but the
kernel.org page also has release signatures if you want to verify your
download.

Then, compile and install:

```
$ tar -zxf git-2.8.0.tar.gz
$ cd git-2.8.0
$ make configure
$ ./configure --prefix=/usr
$ make all doc info
$ sudo make install install-doc install-html install-info
```

After this is done, you can also get Git via Git itself for updates:

    $ git clone git://git.kernel.org/pub/scm/git/git.git

------

## Ignoring Files

Often, you’ll have a class of files that you don’t want Git to automatically
add or even show you as being untracked. These are generally automatically
generated files such as log files or files produced by your build system. In
such cases, you can create a file listing patterns to match them named
.gitignore. Here is an example .gitignore file:

```
$ cat .gitignore

*.[oa]
*~
```

The first line tells Git to ignore any files ending in “.o” or “.a” — object
and archive files that may be the product of building your code. The second
line tells Git to ignore all files whose names end with a tilde (~), which is
used by many text editors such as Emacs to mark temporary files. You may also
include a log, tmp, or pid directory; automatically generated documentation;
and so on. Setting up a .gitignore file for your new repository before you get
going is generally a good idea so you don’t accidentally commit files that you
really don’t want in your Git repository.

The rules for the patterns you can put in the .gitignore file are as follows:

- Blank lines or lines starting with # are ignored.

- Standard glob patterns work, and will be applied recursively throughout the
  entire working tree.

- You can start patterns with a forward slash (/) to avoid recursivity.

- You can end patterns with a forward slash (/) to specify a directory.

- You can negate a pattern by starting it with an exclamation point (!).

Glob patterns are like simplified regular expressions that shells use. An
asterisk (*) matches zero or more characters; [abc] matches any character
inside the brackets (in this case a, b, or c); a question mark (?) matches a
single character; and brackets enclosing characters separated by a hyphen
([0-9]) matches any character between them (in this case 0 through 9). You can
also use two asterisks to match nested directories; a/**/z would match a/z,
a/b/z, a/b/c/z, and so on.

**Tip**

GitHub maintains a fairly comprehensive list of good .gitignore file examples
for dozens of projects and languages at https://github.com/github/gitignore if
you want a starting point for your project.

**Note**

In the simple case, a repository might have a single .gitignore file in its
root directory, which applies recursively to the entire repository. However, it
is also possible to have additional .gitignore files in subdirectories. The
rules in these nested .gitignore files apply only to the files under the
directory where they are located. The Linux kernel source repository has 206
.gitignore files.

------

## Viewing the Commit History

```
git log
        -p, --patch	/* Shows the difference.
        -number		/* Shows the numbers of logs.
        --stat		/* Shows some abbreviated stats for each commit.

/* This option changes the log output to formats other than the default.
        --pretty
                =oneline	/* In one line mode.
                =short
                =full
                =fuller

                =format:"%h - %an, %ar : %s"
/* Shows your branch and merge history by using ASCII graph.
        --graph

        --since, --until
                 =2.weeks	/* In the last two weeks.
                 "2008-01-15"   /* Specific time format.
                 "2 years 1 day 3 minutes ago" /* also works.

/* limit the log output to commits that introduced a change to those files.
        -- path/to/file

/*  To prevent the display of merge commits.
        --no-merge

```

Table 1. Useful specifiers for git log --pretty=format

```
Specifier 	Description of Output

%H		Commit hash
%h		Abbreviated commit hash
%T		Tree hash
%t		Abbreviated tree hash
%P		Parent hashes
%p		Abbreviated parent hashes
%an		Author name
%ae		Author email
%ad		Author date (format respects the --date=option)
%ar		Author date, relative
%cn		Committer name
%ce		Committer email
%cd		Committer date
%cr		Committer date, relative
%s		Subject
```

Table 2. Common options to git log

```
Option		Description

-p		Show the patch introduced with each commit.
--stat		Show statistics for files modified in each commit.
--shortstat	Display only the changed/insertions/deletions line from the
--stat command.

--name-only		Show the list of files modified after the commit
information.

--name-status		Show the list of files affected with
added/modified/deleted information as well.

--abbrev-commit		Show only the first few characters of the SHA-1
checksum instead of all 40.

--relative-date		Display the date in a relative format (for example, “2
weeks ago”) instead of using the full date format.

--graph			Display an ASCII graph of the branch and merge history
beside the log output.

--pretty		Show commits in an alternate format. Option values
include oneline, short, full, fuller, and format (where you specify your own
format).

--oneline		Shorthand for --pretty=oneline --abbrev-commit used
together.
```

Table 3. Options to limit the output of git log

```
Option			Description

-<n>			Show only the last n commits

--since, --after
Limit the commits to those made after the specified date.

--until, --before
Limit the commits to those made before the specified date.

--author
Only show commits in which the author entry matches the specified string.

--committer
Only show commits in which the committer entry matches the specified string.

--grep
Only show commits with a commit message containing the string

-S
Only show commits adding or removing code matching the string
```

------
