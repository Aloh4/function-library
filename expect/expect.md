

**Optional prompts in expect**

```
expect { 
    "prompt2" { 
        send "pass2"
        expect "prompt3"
        send "pass3"
    }
    "prompt3" {
        send "pass3"
    }
}

expect "prompt1"
send "pass1"
expect { 
    "prompt2" { 
        send "pass2"
        exp_continue
    }
    "prompt3" {
        send "pass3"
    }
}
```

**If-else expect**
```
if {[string first "Error: 1" $match]!=-1} {
    # scenario 1
} elseif {[string first "Error:" $match]!=-1} {
    # scenario 2
} else {
    # scenario 3
}
```

**Saving logs**
```
log_file -a file-name
log_file -noappend  # causes overwrite

or...

log_file output.log; # Start logging
#some actions here
log_file; # stopping logging here.
```
**Nested for**
```
#!/usr/bin/expect -f
# Set up various other variables here ($user, $password)
set user "myuser"
set password "mypass!"
# Get the list of hosts, one per line #####
set f [open "host.txt"]
set hosts [split [read $f] "\n"]
close $f

# Get the commands to run, one per line
set f [open "commands.txt"]
set commands [split [read $f] "\n"]
close $f

# Get the hostnames to compare, one per line
set f [open "commands.txt"]
set hostnames [split [read $f] "\n"]
close $f

# Iterate over the hosts
foreach host $hosts {
    spawn ssh $user@$host
    expect "password:"
    send "$password\r"

    # Iterate over the commands
    foreach cmd $commands {
        expect ">"
        send "$cmd\r"
    }

    # Tidy up
    expect "#"
    send "q\r"
    send "quit\r"
    expect eof
    close
}
```
**Expect inside shell script (Here Document)**
```
#!/bin/bash 

  echo "[DEBUG] INIT BASH"

  cd /home
  touch somefile

/usr/bin/expect <(cat << EOF
spawn scp -r -P remoteServerPort somefile remoteServerIP:/home
expect "Password:"
send "MyPassWord\r"
interact
EOF
)

  echo "[DEBUG] END BASH"
  
--- or --- 
 
 #!/bin/bash 

  echo "[DEBUG] INIT BASH"

  cd /home
  touch somefile

/usr/bin/expect << EOF
spawn scp -r -P remoteServerPort somefile remoteServerIP:/home
expect "Password:"
send "MyPassWord\r"
interact

EOF

``` 
**Arguments**
```
#!/usr/bin/expect -f
set username [lindex $argv 0];
set password [lindex $argv 1];

spawn ssh $username@any-ip
expect "password:" {send "$password\r" }
expect "*>" {send "display ve" }
```
