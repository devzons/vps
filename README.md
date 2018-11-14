# vps

## Change pass
```
# passwd root
# adduser dev
# gpasswd -a dev sudo
```
## Set Up SSH keys

check for existing SSH key

```
$ ls -l ~/.ssh/id_*.pub

```
generate a new 4096 bit SSH
```
$ ssh-keygen -t rsa -b 4096 -C "example@example.com"
```
verify
```
$ ls ~/.ssh/id_*
```

```
ssh root@168.235.85.22 C:\Users\Web\.ssh\my_server
```

## Improving the connection flow to the VPS

Create config file
```
Host my_vps_root
	HostName 168.235.85.22
	User root
	IdentityFile C:\Users\Web\.ssh\my_server
```
save file

```
ssh my_vps_root
```

## Keep active the connection to the server

```
ServerAliveInterval 120
ServerAliveCountMax 3

Host my_vps_root
	HostName 168.235.85.22
	User root
	IdentityFile C:\Users\Web\.ssh\my_server
```

## Pointing an existing domain to the VPS server

create A record
create CNMAE record

## Commands

```
ls -la

ll

cd -

touch

nano my_file.txt

cat my_file.txt // see the content from command

less my_file.txt // long content // Q to exit

mv my_file.txt / // to root 

mv my_file.txt ~/my_other_file.txt

cp

rm

rm test_*
```

Directory

```
mkdir

cp -r dir_1 dir_2

mv

rm -r test

clear

cd

cd / 
```

ctrl + c syste

## How to restart the server when required

```
reboot
```







```



