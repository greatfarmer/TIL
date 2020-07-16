# missing-semester
- https://missing.csail.mit.edu/

## Lecture 1: Course Overview + The Shell
- `Ctrl + L`
  - same as `clear`

## Lecture 2: Shell Tools and Scripting
- [shellcheck](https://www.shellcheck.net/)
  - ShellCheck is an open source static analysis tool that automatically finds bugs in your shell scripts.

- man 관련
  - [tldr](https://tldr.sh/)
    - The TLDR pages are a community effort to simplify the beloved man pages with practical examples.

- grep 관련
  - [ripgrep(rg)](https://github.com/BurntSushi/ripgrep)
    - ripgrep recursively searches directories for a regex pattern
  - [ack](https://linux.die.net/man/1/ack)
    - Ack is designed as a replacement for 99% of the uses of grep.
  - [ag](https://github.com/ggreer/the_silver_searcher)
    - A code-searching tool similar to ack, but faster.

- tree 관련
  - tree
    - recursive directory listing program that produces a depth-indented listing of files.
  - [broot](https://dystroy.org/broot/)
    - A new way to see and navigate directory trees
  - [nnn](https://github.com/jarun/nnn)
    - nnn is a full-featured terminal file manager. 

- find 관련
  - locate
    - locate is a Unix utility which serves to find files on filesystems.

- ls -R
- `Ctrl + R`
  - reverse-i-search

## Lecture 3: Editors (vim)
- `i`
  - insert mode

- `Ctrl + V`
  - visual mode

- `:help`
  - `:help :w`

- `:sp`
  - `:Ctrl + w`

- `w`
  - 단어의 첫
- `b`
  - 뒤로
- `e`
  - 단어의 끝
- `0`
  - 
- `$`

## Lecture 4:

## Lecture 5: Command-line Environment

### Job control
- SIGINT `Ctrl + C`
 - man signal
 - sigint.py
 - can't exit using `Cntl + C`, can exit using `Cntl + \`
 ```python
 #!/usr/bin/env python
import signal, time
def handler(signum, time):
    print("\nI got a SIGINT, but I am not stopping")

signal.signal(signal.SIGINT, handler)
i = 0
while True:
    time.sleep(.1)
    print("\r{}".format(i), end="")
    i += 1
 ```

  - SIGKILL
  - SIGQUIT `Ctrl + \`
  - SIGSTOP `Ctrl + Z`
  - 시그널 목록 확인
    - `kill -l`

  - nohup sleep 2000 &
  - jobs
  - bg %1
  - kill %3
  - kill -STOP %1
  - kill -HUP %1

### Terminal Multiplexers
- tmux

### Dotfiles

### Remote Machines