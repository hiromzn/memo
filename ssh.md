### basic operation

- prepare

```
#
# create SSH key without passphrase
#
$ ssh-keygen -t rsa
<RETURN>
<RETURN>
<RETURN>

#
# copy SSH key to remote host
#
$ ssh-copy-id user@hostname

$ ssh user@hostname # << check !
```

- delete password (passphrase) for SSH key

```
ssh-keygen -p
ssh-keygen -p [-f keyfile]
ssh-keygen -p [-P old_passphrase] [-N new_passphrase] [-f keyfile]
```

- ssh ping from client

```
$ cat ~/.ssh/config
ServerAliveInterval 60
ServerAliveCountMax 3
```

- ssh key algorithm

```
$ cat ~/.ssh/config
KexAlgorithms +diffie-hellman-group1-sha1
Ciphers +aes128-cbc,3des-cbc,aes192-cbc,aes256-cbc
```

```
# specify algotithm on command line
$ ssh -c aes128-cbc user_name@hostname
```

```
# check client algotithm
$ ssh -Q cipher
```

- config file
```
$ cat ~/.ssh/config

ServerAliveInterval 60
ServerAliveCountMax 3
KexAlgorithms +diffie-hellman-group1-sha1
Ciphers +aes128-cbc,3des-cbc,aes192-cbc,aes256-cbc

#
# Jump server settins
#   client ==> jump ==> work_server
#
Host jump gateway
    Hostname jump_hostname_or_IP
    User username_of_jump
    ForwardAgent yes
    AddKeysToAgent yes
    KexAlgorithms diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1
    Ciphers +aes128-cbc,3des-cbc,aes256-cbc,aes192-cbc
    ServerAliveInterval 60

Host work_server
    User hmizuno
    HostName work_server_name_or_IP
    ProxyCommand /usr/bin/ssh jump -W %h:%p
    # ProxyCommand ssh jump -i .ssh/id_rsa_jump -W %h:%p
    ForwardAgent yes
    AddKeysToAgent yes
```

## jump / proxy
```
# simple jump (local -> jump -> target)
ssh -J rs44@jump hmizuno@target

# double jump (local -> jump -> ap4 -> target)
ssh -J rs44@jump,hmizuno@ap4 -i ~/.ssh/id_rsa.ap4 -p 2233 hmizuno@target
```

- sample of config
```
KexAlgorithms diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1
Ciphers +aes128-cbc,3des-cbc,aes256-cbc,aes192-cbc
ServerAliveInterval 60
ServerAliveCountMax 3
ForwardAgent yes
AddKeysToAgent yes

Host jump jumpserver
    Hostname xx.xx.xx.xx
    User rs44
    SendEnv LANG=C
    ForwardAgent yes

Host ap4
        User hmizuno
        HostName xx.xx.xx.xx
        ProxyJump jump

Host mydevelop
        User hmizuno
        HostName xx.xx.xx.xx
        Port 2233
        IdentityFile /c/msys64/home/hmizuno/.ssh/id_rsa.ap4
        ProxyJump ap4
```

## remote forward
### How to use
- local PC
  - setup .ssh/config of local PC
  - execute ssh on local PC
    ```
    $ ssh mydevserver
    ```
- remote : develop server
  - setup proxy config
    - for yum
      - /etc/yum.conf : add entry "proxy=socks5h://localhost:3127"
    - for npm (nodejs)
      - execute "npm -g config set proxy socks5h://localhost:3127"
  - you can use yum and npm with proxy (port forwarding).

### .ssh/config of local PC
```
Ciphers +aes128-cbc,3des-cbc,aes256-cbc,aes192-cbc
ServerAliveInterval 60
ServerAliveCountMax 10
ForwardAgent yes
AddKeysToAgent yes

Host jump
    Hostname jumpserver1
    User user_of_jump
    HostkeyAlgorithms +ssh-rsa,ssh-dss
    KexAlgorithms diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1

Host jump2
    User hmizuno
    HostName jumpserver2
    ProxyJump jump

Host mydevserver
    User hmizuno
    HostName mydevcontainer
    Port 2233
    ProxyJump jump2
    StrictHostKeyChecking no
    UserKnownHostsFile /dev/null
    CheckHostIP yes
    ForwardX11 yes
    ForwardX11Trusted yes
    Compression yes
    GatewayPorts yes
    RemoteForward 6001
    RemoteForward 3127
```

