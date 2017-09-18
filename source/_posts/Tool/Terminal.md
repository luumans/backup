title:  Terminal配置
date: 2017-08-02 14:11:20
description: 
categories:
- Tool
tags:
- Terminal
toc: true
author:
comments:
original:
permalink: 
---
> vim ~/.bash_profile

a ：编辑模式
esc ：命令模式
:wq ：保存并退出

<!-- more -->


```
# for color
export CLICOLOR=1
# \h:\W \u\$
export PS1='\[\033[01;33m\]\u@\h\[\033[01;31m\] \W\$\[\033[00m\] '
# grep
alias grep='grep --color=always'
```

```
find_git_branch () {
  local dir=. head
  until [ "$dir" -ef / ]; do
    if [ -f "$dir/.git/HEAD" ]; then
      head=$(< "$dir/.git/HEAD")
      if [[ $head = ref:\ refs/heads/* ]]; then
        git_branch="<${head#*/*/}>"
      elif [[ $head != '' ]]; then
        git_branch="<(detached)>"
      else
        git_branch="<(unknow)>"
      fi  
      return
    fi  
    dir="../$dir"
  done
  git_branch=''
}
# for color
export CLICOLOR=1
# \h:\W \u\$
PROMPT_COMMAND="find_git_branch; $PROMPT_COMMAND"
export PS1='\[\033[01;33m\]\u@\[\033[01;31m\]\W\[\033[00;31m\] $git_branch\[\033[01;31m\]\$\[\033[00m\] '
```




