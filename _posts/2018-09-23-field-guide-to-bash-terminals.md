---
layout: post
title: Field Guide to Bash Terminals
categories: [bash, linux]
---

You're setting up your bash profile. You go to the [Bash man page](https://linux.die.net/man/1/bash){:target="_blank"}. Damn... you just
wanted to know where to stick your config and aliases and this page is written in legalese!

This is an overview of the different kinds of Bash terminal and how they find their config, written in less detail but
more to the point.

Terminology used:

- `--login`, the option that makes Bash a login shell
- `--interactive`, the option for a terminal that a user can interact with
- `-c`, the option for running a single command, non-interactively
- `~`, your home directory
- __file chain__, a list of file locations where Bash will look for config files

### The 2 File Chains

These file chains are referred to later on and act as a shorthand for particular behaviours of Bash. 'File chain' isn't an
official term, I made it up for this post.

Some general notes:
- files in `/etc` are shared across all users on the system. 
- files in `~` are run for you only
- missing files won't cause an error, they will just be skipped

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

`/etc/bash.bashrc1` was added so that the `bashrc` file chain would have its own
location for system-wide configuration, like `/etc/profile` provides. Debian and
Ubuntu have adopted this feature.

### The 5 Different Kinds of Terminal

#### `--login --interactive`  

_e.g. SSH-ing somewhere, or logging into your physical computer_

Runs the `profile` file chain, as this will be the first terminal to run for a user on a system.
When terminals create other terminals, they pass on their environment variables to their
children, which is how the `profile` file chain is shared among all
terminals on a system.

Although it is interactive, the property of being a `login` terminal overrides being an `interactive` terminal, so a 
`login` terminal won't run the `bashrc` file chain by default.

It's useful to make the assumption that login terminals are always interactive - they usually are,
and it allows us to:
- always call the `bashrc` file chain, using a command in a file in the `profile` file chain
- store config that applies to _all_ terminals in the `bashrc` file chain (which many distros
now do), knowing that it will be run regardless of whether a terminal is `login` or not

There's an edge case that contradicts this rule that I'll cover later.
___
#### `--interactive`  

_e.g. `bash`, or opening a GUI terminal program_

Runs the `bashrc` file chain. Doesn't run `profile` file chain as it expects to inherit
it from a parent login terminal. `bashrc` file chain is run once per terminal because:
- There is no guarantee `--login` terminal ran it
- It could contain code that needs to be run once per terminal creation
___
#### `-c` locally

_e.g. `bash -c 'echo hello world'`_

Run Bash with a single command rather than interactively. Expects to inherit both the
`profile` and `bashrc` file chains from a parent terminal. It also has a special environment
variable, `$BASH_ENV`, which can be set to a file path. Bash will run all commands in that file, before 
running the command set with `-c`
___
#### `-c` remotely

_e.g. `ssh user@host 'echo hello world'`_

Although it doesn't actually use the `-c` syntax, `ssh user@host <command>` achieves a similar effect. It won't run the
remote `profile` file chain, unlike SSH-ing normally does and it _doesn't_ use the `$BASH_ENV` variable,
but it will still run the remote system’s `bashrc` file chain before running the command.
___
#### `--login`, non-interactively??

_e.g. `ssh user@host < input-file.txt`_

99.99% of the time you can assume that all login terminals are also interactive. 0.01% of the time you 
"need" to pipe a command in a local file to be run on a remote system. In this edge case, the remote
`profile` file chain will be run, then the contents of `input-file.txt`, and then the SSH
connection will terminate. This edge case won't even have any meaningful side-effects most of the time.
<br><br>
### Key Points

- `--login` should be configured to also run the bashrc file chain as well as profile, as then config common to
all terminals can be stored there, rather than have the two file chains be run in a mutually exclusive way
(many distros are already doing this but it's worth checking)
- If it's just for you it should go in `~`, if it's for everyone on the system it should go in `/etc`
- Changes to `profile` file chain will only be applied automatically after your next login.
- Terminals can acquire config by running a file chain or inheriting it from a parent terminal process.