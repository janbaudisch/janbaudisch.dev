+++
title = "How not to use the filesystem"
date = 2018-11-04
draft = false

[taxonomies]
tags = ["linux", "truestory"]
+++

Today, I probably did the most stupid thing I have ever done on a Linux machine.

But let's start from the beginning, how the story unfold:

## How not to use rustup's filesystem layout

`rustup` (by default) installs toolchains under `~/.rustup` and all the important binaries are available under `~/.cargo`.
Because I am having some trouble with getting Android Studio to reach my Rust toolchain, I tried to symlink all binaries in `~/.cargo/bin` to `/usr/bin`. I knew this was going to be bad practice, I did it anyway. Only to realize that this hack did not work and now I wanted to revert my changes. But how? Easy right: Just remove all files beginning with `cargo` or `rust` and any other file (read: symlink) from `~/.cargo/bin`. Now one can see how bad my decision was to just 'try it out' and symlink all those files.

## How not to remove files

For my 'fix' to work, I would obviously have to use `sudo`. Thus, extra caution is necessary. But now see what I did:

What I wanted to do:
```shell
sudo rm cargo*
```

What I did instead:
```shell
sudo rm cargo *
```

...

So, basically, I just deleted _**everything**_ in `/usr/bin`.

Seriously, that's how I just broke my system.

## What we learned

1. *NEVER* 'try this out' when you modify your **system level filesystem**
2. *ALWAYS* backup your system
3. *ALWAYS* read your command twice if it combines `rm` and `sudo`
4. *even better:* use something like `rm -i` or [`trash-cli`][trash-cli]

[trash-cli]: https://github.com/andreafrancia/trash-cli
