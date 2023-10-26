## What is Git

Git is a Distributed Version Control Systems(DVCSs) 

Key features of Git

- Snapshots, not differences
  Git thinks its data is more like **a stream of snapshots**: (the biggest difference with other VCS)
  ![NeatReader-1698237047645](https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202310252037712.png)



- Local Operation

- Integrity
  checksummed before stored, with SHA-1 hash

- Generally only adds data

- Three states:

  - modified: changed since it is staged but has not been committed
  - staged: has been modified and was added to the staging area
  - committed: a particular version of a file is in the Git directory


  <img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202310252045064.png" alt="NeatReader-1698237938875" style="zoom:33%;" />



## Git Basic

Requirement:

- configure and initialize a repo
- begin and stop tracking files
- stage and commit changes



### Get a Git repo

- init a git repo in local dir

```bash
## init a git repo in local dir
git init

git add <file>
# git add licence
git commit -m 'annotation'
```



- clone an exiting git repo

```bash
## clone an exiting git repo to target dir
git clone <url> <dir>
```

#### Recording changes to the repo

- Two states in the working directory: Traced & Untraced

  - Traced File: files in VCS(git knows them)
  - Untraced File: files not in the VCS(git doesn't know them)

  <img src="https://picgo-wbyz.oss-cn-nanjing.aliyuncs.com/202310261113054.png" alt="Traced-file" style="zoom:33%;" />

```bash
git status # check the status of your files
## output:
# On branch master
# Your branch is up-to-date with 'origin/master'.
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
```

```bash
git add <file/dir> # transfer untraced file into traced file / stage the traced file
```

#### short status

```bash
## for short output, using:
git status -s
## or 
git status --short
```

#### the command's output:

```bash
## two columns: left -> staging area, right -> working tree
# M -> modified. A -> be added to staging area, ?? -> untraced
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

#### Viewing staged or not

```bash
## to see what you have changed but not yet staged
git diff 
## to see what you've staged will go into next commit
git diff --staged
## to see what you've staged so far
git diff --cached

## using externel viewing program to look at diffs
git difftool --tool
```



#### Committing 

```bash
## commit all files in stage area with some messages to remind yourself
git commit -m "Messages of your commit"

## auto stage all traced files and commit
git commit -a
```



#### Ignoring Files

Setting up a `.gitignore` file

- to avoid commit some tmp file and so on



Rules for setting up `gitigonre` file:

- `# string` are ignored, using for annotation
- **Glob patterns** works, is applied recursively in the entire working tree
- `/` to start patterns, avoiding recursivity 
- `!` to negate a pattern

> Glob patterns: 
>
> 1. **`\*`**：匹配零个或多个字符。例如，`*.txt` 会匹配所有以 `.txt` 结尾的文件。
> 2. **`?`**：匹配任何单个字符。例如，`file?.txt` 可以匹配 `file1.txt`、`fileA.txt` 等。
> 3. **`[...]`**：匹配方括号中的任何一个字符。例如，`file[123].txt` 可以匹配 `file1.txt`、`file2.txt` 和 `file3.txt`。
> 4. **`[!...]` 或 `[^...]`**：匹配方括号中**不包含**的任何字符。例如，`file[!123].txt` 会匹配任何不是 `file1.txt`、`file2.txt` 或 `file3.txt` 的 `fileX.txt` 文件。
> 5. **`{a,b,...}`**：匹配大括号中的任何一个模式。例如，`file{A,B}.txt` 会匹配 `fileA.txt` 和 `fileB.txt`。

 Example:

```bash
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```



#### Removing Files

```bash
## remove files from stage area & working tree, file must be modified
git rm <file/dir>

## remove dir recurively
git rm -r <dir>
## remove files from stage area & working tree, whatever file modified or not
git rm -f <file>
## remove files from stage area
git rm --cached <file>/<file-glob patterns>
```



#### Moving Files

```bash
## move files, also used to rename files
git mv <file_from> <file_to>
```



### Viewing Commit History

```bash
## list the commits in reverse chronological order
git log

## show diff of each commit
git log -p
git log --patch
## limit the number of log displayed
git log -2 # 2 can also be other number
## show abbreviated stats of each commit
git log --stat
## change the log format
git log --pretty=oneline # oneline / short / full...(optinoal value)
git log --pretty=format:... # '...' is the machine parsing you need
## other limited options to filter
git log --since=2.weeks	# time limited
git log --author=Misaki	# author limited
git log --grep=key_word	# key word limited
## multi match
git log --since=... --author=... 				# match one of options
git log --since=... --author=... --all-match	# match all options
```

```bash
## only find the commit that changes the occurences of string
git log -S string
## only show the commit that changes the target_file
git los -- path/to/file
```

### Undoing Things

```bash
## amend previous commit(e.g. to add forgotten file)
git commit --amend
```

#### Unstaging a Staged File

```bash
## unstage the file
git reset HEAD <file>

## in git v2.23.0, reset was 'repalced' by 'restore'
git restore --staged <file>
```

#### Unmodified a modified file

```bash
## unmodified a modified file
git checkout -- <file> # Dangerous!!!, better not use it

## restore!
git restore <file>
```





## Readings

pro-git

glob patterns