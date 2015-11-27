# i3 minimal configuration incorporating ideas from wmii

## Introduction

For a long time I have used [wmii](https://en.wikipedia.org/wiki/Wmii)
but recently I have started working with more than 2 monitors so wmii
is no longer the best option. It also seem to have died as a project,
so it was time to move on. I tried [i3](http://i3wm.org) a long time
ago and it sucked compared with wmii. But now is a great tilling
window manager. It's multiscreen aware, something wmii is not.

There were however a few things that wmii has that i3 in it's vanilla
version has not, namely:

 + **dynamic tagging**: I hate having to memorize workspace numbers to
   know where a certain window is. Note that by default i3 tags all
   workspaces with numbers.

 + **dmenu runs at bottom and not at the top**: I hate having to look
   to select an item from dmenu for launching that application.

Fortunately [i3](http://i3wm.org/docs/userguide.html) is configurable
enough to set the wmii behaviors in place.

## How it works

Among other customizations the dynamic tagging of windows and moving
between the workspaces is achieved purely in BASH. No other language
is required. It uses [jq](https://stedolan.github.io/jq/) (the `sed`
for JSON) to parce the workspace list (implemented as an array of
Javascript objects). The dynamic tagging and movement relies on two
simple shell scripts: `i3-dtags-moveto.sh`, `i3-dtags.sh` that pipe
the output from `i3-msg` to `jq` and `dmenu`. Here is the relevant
part of the i3 configuration.

```shell
## Rename workspace.
bindsym Mod4+r exec i3-input -F 'rename workspace to "%s"' -P 'New name: '

## Show all tags and allow user to select one though dmenu.
bindsym $mod+t exec $HOME/.i3/i3-dtags.sh -fn '$dfont'
## Show tags, and move the current window to the one selected using dmenu.
bindsym $mod+Shift+t exec $HOME/.i3/i3-dtags-moveto.sh -fn '$dfont'
```

`$dfont` is the font specification to be used by dmenu. There's also a
binding to rename workspaces.

The full configuration is in `config`. Note that I use the excelent
[terminus](http://terminus-font.sourceforge.net/) font instead of the
more flashing and eye straining *modern* fonts. It's extremely
readable and easy going on the eyes.

## Installation

 1. Get the terminus font. On debian install the
    [xfonts-terminus](https://packages.debian.org/search?keywords=xfonts-terminus)
    package.
 2. Install jq. On debian install the
    [jq](https://packages.debian.org/search?keywords=jq) package.
 3. Clone the repository to your i3 home directory `~/.i3`:
    
        git clone https://github.com/perusio/i3-config-dynamic-tagging.git ~/.i3
 4. Reload i3 in place: the default key binding is: `$mod+Shift+r`,
    where `$mod` is the modifier key. By default is `Mod1` (Alt) but
    it can be set to the super (windows) key `Mod4`. In that case
    adjust the rename command key binding. Otherwise it will conflict
    with the resize command.
 5. Done.

## License: 2-clause BSD

This program is free software, you can redistribute it and/or
modify it under the terms of the new-style BSD license.

You should have received a copy of the BSD license along with this
program. If not, see http://www.debian.org/misc/bsd.license.
