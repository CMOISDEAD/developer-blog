---
author: CMOISDEAD
pubDatetime: 2023-06-21T21:08:00Z
title: How create zsh theme for your terminal
postSlug: how-create-zsh-theme
featured: true
draft: false
tags:
  - Terminal
  - Customize
ogImage: ""
description: Some recommendations & steps for creating your custom theme for your zsh.
---

Here Some recommendations & steps for creating your custom theme for your zsh.

## Table of contents

## Install & configure Oh My ZSH

## Start with your own theme

First you need to create a file called `doom.zsh-theme`, in this file we go to start creating the basic structure of our theme.

Inside of your theme file, add a `PROMPT` variable, this variable represent the left prompt of your theme, put something like:

```zsh
PROMPT="username: "
```

In the same way `RPPROMPT` variable represent, the right prompt of your theme.

### Add your username and actual directory to the prompt

To add your username or anything else you can do it creating a function who return the value you want to show.

```zsh
username() {
  echo $USER
}

PROMPT="${username}"
```

And to show the current directory you return this value.

```zsh
current_dir() {
  local dir="%3~"
  echo dir
}

PROMPT="${username} at ${current_dir} $ "
```

## Add sections

You can split your file in multiple files with the objective to make your theme more clean, create a dir called `sections` and inside create files with a function who return your section value.

Then in the top of your `doom.zsh-theme` source this files.

```zsh
DOOM_ROOT=`$HOME/path/to/theme/` # This variable will help you to make your code cleaner.

builtin source "$DOOM_ROOT/sections/username"
builtin source "$DOOM_ROOT/sections/dir"
```

To make this more dynamic, create an array with the name of all your sections, then source all this files in this way.

```zsh
sections=(
  # add here all your sections
  username
  dir
  # status # check the last command status
)

for section in "$sections[@]"; do
  # check if the section file exists
  if [[ -f "$DOOM_ROOT/sections/$section.zsh" ]]; then
    source "$DOOM_ROOT/sections/$section.zsh"
  else
    echo "Section '$section' was not loaded."
  fi
done
```

And to add all the sections you need to do something similar.

```zsh
generate_prompt() {
  prompt=""
  [[ -z $sections ]] && return
    for section in $sections; do
      prompt+="$($section)"
    done
  echo $prompt
}

PROMPT="$(generate_prompt)"
```

In this way you can have a lot of sections and only need to add the name of the section in one place.

## ZSH Hooks
