# Linux Priv Esc

[GTFO]('https://gtfobins.github.io/')
## Exploiting Script/Program Ownership

If a custom script is located in the box that is executed by another user, injecting something like: `import pty; pty.spawn("/bin/bash");` will allow you to immediately login as the new user without needing to setup a reverse shell.
