# The Missing Semester of Your CS Eduction

[The Missing Semester of Your CS Education (mit.edu)](https://missing.csail.mit.edu/)

## Course overview + the shell

[Quoting (Bash Reference Manual) (gnu.org)](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)

- three quoting mechanisms: `\` `'` `"`
  - Escape Character `\`:
    - Used to escape the special meaning of individual characters.
    - It allows a mix of escaping and non-escaping within a quoted string.
    - Commonly used for escaping special characters or creating custom escape sequences.
      [ANSI-C Quoting (Bash Reference Manual) (gnu.org)](https://www.gnu.org/software/bash/manual/html_node/ANSI_002dC-Quoting.html)
  - Single Quotes `'`':
    - Treats everything enclosed as a literal string.
    - No variable expansion or command substitution occurs.
    - Ideal for preserving text exactly as it is, without any interpretation.
  - Double Quotes `"`:
    - Allow variable expansion and command substitution.
    - Interpret and expand variables and commands within the quotes.
    - Useful when you want to include variable values or execute commands within the string.
      [Locale Translation (Bash Reference Manual) (gnu.org)](https://www.gnu.org/software/bash/manual/html_node/Locale-Translation.html)

[Shebang (Unix) - Wikipedia](https://en.wikipedia.org/wiki/Shebang_(Unix))

- a **shebang** is the character sequence consisting of the characters [number sign](https://en.wikipedia.org/wiki/Number_sign) and [exclamation mark](https://en.wikipedia.org/wiki/Exclamation_mark) (#!) at the beginning of a [script](https://en.wikipedia.org/wiki/Script_(computing)).
- `#! interpreter [optional-arg]`

## Shell Tools and Scripting

[Special Characters (tldp.org)](https://tldp.org/LDP/abs/html/special-chars.html)

### shell script

Parameters:

- `$0` - 脚本名
- `$1` 到 `$9` - 脚本的参数。 `$1` 是第一个参数，依此类推。
- `$@` - 所有参数
- `$#` - 参数个数
- `$?` - 前一个命令的返回值
- `$$` - 当前脚本的进程识别码
- `!!` - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 `sudo !!`再尝试一次。
- `$_` - 上一条命令的最后一个参数。如果你正在使用的是交互式 shell，你可以通过按下 `Esc` 之后键入 . 来获取这个值。

shell的 *通配*（*globbing*）:

- 通配符 - 当你想要利用通配符进行匹配时，你可以分别使用 `?` 和 `*` 来匹配一个或任意个字符。例如，对于文件 `foo`, `foo1`, `foo2`, `foo10` 和 `bar`, `rm foo?`这条命令会删除 `foo1` 和 `foo2` ，而 `rm foo*` 则会删除除了 `bar`之外的所有文件。
- 花括号 `{}` - 当你有一系列的指令，其中包含一段公共子串时，可以用花括号来自动展开这些命令。这在批量移动或转换文件时非常方便。

[github.com/koalaman/shellcheck](https://github.com/koalaman/shellcheck)

### shell Tools

Find Files: `find` `fd` `lcoate`

Find Code: ==`grep`==  `rg` `ack` `ag`

Find Shell command: `history` `fzf`

[fzf Wiki (github.com)](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings#ctrl-r)

File direction: `fasd` `autojump`

[fasd](https://github.com/clvv/fasd)  [autojump](https://github.com/wting/autojump)

## Vim(Editor)

### Model

- **Normal**: for moving around a file and making edits
- **Insert**: for inserting text
- **Replace**: for replacing text
- **Visual** (plain, line, or block): for selecting blocks of text
- **Command-line**: for running a command

.vimrc

[amix/vimrc: The ultimate Vim configuration (vimrc) (github.com)](https://github.com/amix/vimrc)

```bash
git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim_runtime
sh ~/.vim_runtime/install_awesome_vimrc.sh
```

Practice

[Interactive Vim tutorial - sandbox (openvim.com)](https://www.openvim.com/sandbox.html)

## Data Wrangling

Regular expression

[regex101: build, test, and debug regex](https://regex101.com/)

[RegexOne - Learn Regular Expressions - Lesson 1: An Introduction, and the ABCs](https://regexone.com/)

awk xargs

gnuplot ffmpeg

## Command-line Environment

### Job Control

`man signal` [signal(7) - Linux manual page (man7.org)](https://www.man7.org/linux/man-pages/man7/signal.7.html)

Killing a process: `ctrl-C`  kill -TERM `<PID>`

Pausing and backgrounding processes: `jobs` , `bg` and `fg` `nohup` `pgrep`

### Terminal Multiplexers

[tmux(1) - Linux manual page (man7.org)](https://www.man7.org/linux/man-pages/man1/tmux.1.html)

- Sessions  \- a session is an independent workspace with one or more windows
  - `tmux` starts a new session.
  - `tmux new -s NAME` starts it with that name.
  - `tmux ls` lists the current sessions
  - Within `tmux` typing `<C-b> d` detaches the current session
  - `tmux a` attaches the last session. You can use `-t` flag to specify which
- Windows  \- Equivalent to tabs in editors or browsers, they are visually separate parts of the same session
  - `<C-b> c` Creates a new window. To close it you can just terminate the shells doing `<C-d>`
  - `<C-b> N` Go to the *N* th window. Note they are numbered
  - `<C-b> p` Goes to the previous window
  - `<C-b> n` Goes to the next window
  - `<C-b> ,` Rename the current window
  - `<C-b> w` List current windows
- Panes  \- Like vim splits, panes let you have multiple shells in the same visual display.
  - `<C-b> "` Split the current pane horizontally
  - `<C-b> %` Split the current pane vertically
  - `<C-b> <direction>` Move to the pane in the specified *direction*. Direction here means arrow keys.
  - `<C-b> z` Toggle zoom for the current pane
  - `<C-b> [` Start scrollback. You can then press `<space>` to start a selection and `<enter>` to copy that selection.
  - `<C-b> <space>` Cycle through pane arrangements.

https://tmuxcheatsheet.com/

[A Quick and Easy Guide to tmux (hamvocke.com)](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)

[终端神器tmux：多任务管理大师_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1ML411h7tF/?spm_id_from=333.337.search-card.all.click&vd_source=768baf220865b9cce87e8cb50c20fecc)

[Linux Command Line Adventure: Terminal Multiplexers](http://linuxcommand.org/lc3_adv_termmux.php)

### Aliases

```bash
alias alias_name="command_to_alias arg1 arg2"
```

### Dotfiles(配置文件)

Examples:

- `bash` - `~/.bashrc`, `~/.bash_profile`
- `git` - `~/.gitconfig`
- `vim` - `~/.vimrc` 和 `~/.vim` 目录
- `ssh` - `~/.ssh/config`
- `tmux` - `~/.tmux.conf`

[Dotfiles Repository search results (github.com)](https://github.com/search?o=desc&q=dotfiles&s=stars&type=Repositories)

Benefits:

- **Easy installation**
- **Portability**:

  - using `if` to  apply machine specific customizations

  ```bash
  if [[ "$(uname)" == "Linux" ]]; then {do_something}; fi

  # Check before using shell-specific features
  if [[ "$SHELL" == "zsh" ]]; then {do_something}; fi

  # You can also make it machine-specific
  if [[ "$(hostname)" == "myServer" ]]; then {do_something}; fi
  ```

  - `include`

    ```bash
    [include]
        path = ~/.gitconfig_local
    ```
  - share some configurations in different programs: like `.alias` of bash and zsh

    ```bash
    # Test if ~/.aliases exists and source it
    if [ -f ~/.aliases ]; then
        source ~/.aliases
    fi
    ```
- **Synchronization**
- **Change tracking**

### Remote Machines

To `ssh` a server

```bash
# the server can be URL or IP
ssh  usr@server
```

executing commands

```bash
ssh usr@server commands
```

SSH keys [ssh-keygen(1) - Linux manual page (man7.org)](https://www.man7.org/linux/man-pages/man1/ssh-keygen.1.html)

- Key generation:

  ```bash
  ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519
  ```
- Key based authentication: ssh `will look into`.ssh/authorized_keys

  ```bash
  ssh-copy-id -i .ssh/id_ed25519 usr@rsercer
  ```

Copying files over SSH

- `ssh+tee`:  `cat localfile | ssh remote_server tee serverfile`
- `scp`:  `scp path/to/local_file remote_host:path/to/remote_file`
- `rsync`:  a similar syntax to `scp`

Port Forwarding: [Article of Port forwarding in StackOverflow](https://unix.stackexchange.com/questions/115897/whats-ssh-port-forwarding-and-whats-the-difference-between-ssh-local-and-remot)

- Lock Port Forwarding
- Remote Port Forwarding

SSH configuration:

- local: `~/.ssh/config`
- server: `/etc/ssh/sshd_config`

Miscellaneous

- `Mosh`: Improved ssh
- `sshfs`: mount a folder on a remote server locally

## Q & A

Difference between `source script.sh` and `./script.sh`

- For `source` the commands are executed in your current bash session and thus any changes made to the current environment, like changing directories or defining functions will persist in the current session once the `source` command finishes executing.
- When running the script standalone like `./script.sh`, your current bash session starts a new instance of bash that will run the commands in `script.sh`.

[Filesystem Hierarchy Standard - Wikipedia](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard) :

- `/bin` - 基本命令二进制文件
- `/sbin` - 基本的系统二进制文件，通常是root运行的
- `/dev` - 设备文件，通常是硬件设备接口文件
- `/etc` - 主机特定的系统配置文件
- `/home` - 系统用户的主目录
- `/lib` - 系统软件通用库
- `/opt` - 可选的应用软件
- `/sys` - 包含系统的信息和配置([第一堂课](https://missing-semester-cn.github.io/2020/course-shell/)介绍的)
- `/tmp` - 临时文件( `/var/tmp` ) 通常重启时删除
- `/usr/` \- 只读的用户数据
  - `/usr/bin` - 非必须的命令二进制文件
  - `/usr/sbin` - 非必须的系统二进制文件，通常是由root运行的
  - `/usr/local/bin` - 用户编译程序的二进制文件
- `/var` -变量文件 像日志或缓存

More tricks about VIm

- 插件 - 花时间去探索插件。有很多不错的插件修复了vim的缺陷或者增加了能够与现有vim工作流结合的新功能。关于这部分内容，资源是[VimAwesome](https://vimawesome.com/) 和其他程序员的dotfiles。
- 标记 - 在vim里你可以使用 `m<X>` 为字母 `X` 做标记，之后你可以通过 `'<X>` 回到标记位置。这可以让你快速定位到文件内或文件间的特定位置。
- 导航 - `Ctrl+O` 和 `Ctrl+I` 命令可以使你在最近访问位置前后移动。
- 撤销树 - vim 有不错的更改跟踪机制，不同于其他的编辑器，vim存储变更树，因此即使你撤销后做了一些修改，你仍然可以通过撤销树的导航回到初始状态。一些插件比如 [gundo.vim](https://github.com/sjl/gundo.vim) 和 [undotree](https://github.com/mbbill/undotree) 通过图形化来展示撤销树。
- 时间撤销 - `:earlier` 和 `:later` 命令使得你可以用时间而非某一时刻的更改来定位文件。
- [持续撤销](https://vim.fandom.com/wiki/Using_undo_branches#Persistent_undo) - 是一个默认未被开启的vim的内置功能，它在vim启动之间保存撤销历史，需要配置在 `.vimrc` 目录下的 `undofile` 和 `undodir`，vim会保存每个文件的修改历史。
- 热键（Leader Key） - 热键是一个用于用户自定义配置命令的特殊按键。这种模式通常是按下后释放这个按键（通常是空格键）并与其他的按键组合去实现一个特殊的命令。插件也会用这些按键增加它们的功能，例如，插件UndoTree使用 `<Leader> U` 去打开撤销树。
- 高级文本对象 - 文本对象比如搜索也可以用vim命令构成。例如，`d/<pattern>` 会删除下一处匹配 pattern 的字符串，`cgn` 可以用于更改上次搜索的关键字。

Two Factor Authentication 2FA(双因子验证)

Universal 2nd Factor（U2F）


## Readings

**tmux:**

* [A Quick and Easy Guide to tmux (hamvocke.com)](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)
* [Making tmux Pretty and Usable - A Guide to Customizing your tmux.conf (hamvocke.com)](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/)
* [(3) r/unixporn - the home for *NIX customization! (reddit.com)](https://www.reddit.com/r/unixporn/)
