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
