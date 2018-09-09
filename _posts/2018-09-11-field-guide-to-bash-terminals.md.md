---
layout: post
title: Field Guide to Bash Terminals
categories: [bash, linux]
---

You're setting up your bash profile. You go to the [Bash man page](https://linux.die.net/man/1/bash). Damn... you just
wanted to know where to stick your config and environment variables and this page is written in legalese.

This guide is an overview of the different kinds of Bash terminal and how they find their config, which should give you
an intuitive understanding of where your config needs to live.

Terminology used:

- `--login`, makes Bash a login shell, runs special config
- `--interactive`, bash option for terminal that a user can interact with, runs normal config
- `-c`, bash option for running a non-interactive command
- `~`, your home directory
- __file chain__, a list of file locations where Bash will look for config files

### The 2 File Chains

The files in `/etc` are shared across all users on the system. The files in `~` are run for
you only. These file chains won't error if the files don't exist.

##### `profile` file chain - run once per login

- run whatever is in the file `/etc/profile`
- run the first match from the following list:
  * `~/.bash_profile`
  * `~/.bash_login`
  * `~/.profile`
  
- when a terminal is closed, run both of the following files:
  * `/etc/bash.bash_logout`
  * `~/.bash_logout`

##### `bashrc` file chain - run once per terminal creation

- run `/etc/bash.bashrc` _(on most distributions of Linux)_
- run `~/.bashrc`

### The 5 Different Kinds of Terminal

####`--login --interactive`

_e.g. SSH-ing somewhere, or logging into your desktop_

Runs the `profile` file chain, as it is the first terminal to run for a user on a system.
When terminals create other terminals, they pass on their environment (e.g. environment
variables) to their children, which is how the `profile` file chain is shared among all
terminals on a system. In a practical but untrue sense, login terminals are always
interactive.

####`--interactive`

_e.g. `bash`, or opening a GUI terminal program_

Runs the `bashrc` file chain. Doesn't run `profile` file chain as it expects to inherit
it from a parent login terminal. `bashrc` file chain is run once per terminal because:
- There is no guarantee Login terminal ran it
- It could contain code that needs to be run once for every terminal

#### `-c` locally

_e.g. `bash -c 'echo hello world'`_

Run Bash with a single command rather than interactively. Expects to inherit both the
`profile` and `bashrc` file chains from a parent terminal. It also has a special environment
variable, `$BASH_ENV`. If this is set, Bash will treat it as a file of commands
it should run before the main command. Only to be used for config used for non-interactive,
but not interactive

#### `-c` remotely

Like running `-c` locally, but it will run the remote system’s `bashrc` file chain. TODO remote bashrc or file chain, TODO local or remote BASH_ENV or both

#### `--login`, non-interactively??

_e.g. `ssh user@host < input-file.txt`_

99.99% of the time you can assume that all login terminals are also interactive. 0.01% of the time you need to pipe
a command in a local file to be run on a remote system. This edge case arises as `-c` is not
present and bash defaults to being a login terminal.


### Key Points

--login should be configured to also run the bashrc file chain (but most distros already do this)
