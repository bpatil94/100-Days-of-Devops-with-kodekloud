# Day 21 task: Set Up Git Repository on Storage Server

The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:

1.Utilize yum to install the git package on the Storage Server.

2.Create a bare repository named /opt/official.git (ensure exact name usage).

## note: A bare repository is a Git repository that contains only the version control data and does not have a working directory (checked-out files).

What's inside a bare repository?
A bare repository contains the contents that would normally be in the .git directory of a regular repository:
Commit history
Branches
Tags
Git objects
Configuration files

## step1 : First, connect to the target server using a regular user account
( in above , its mentioned storage server)
```
ssh natasha@ststor01
```
check for the os distriibution
```
cat /etc/os-release
```

## step2 : Install git and create bare repository mentioned in problem statement
```
sudo yum install git -y
```
```
git --version
```
```
sudo mkdir -p /opt/official.git
```
```
sudo git init --bare /opt/official.git
```

## step3 : validation 
to verify wheather that bare repo is created or not, use file command ( if file is absent, install it)

to install file
```
sudo yum install file -y
```
```
file /opt/official.git
```
If the above command output is directory (file only identifies the path as a directory), then validation is correct

Other validations methods

```
ls -ld /opt/blog.git
```
```
sudo git --git-dir=/opt/blog.git rev-parse --is-bare-repository
```
The second command should output true.

## or
you can confirm it's a bare Git repository by listing its contents:
```
ls /opt/news.git 
```
If it's bare, you'll see something like: 
HEAD branches config description hooks info objects refs

or you can use this too view in one coloumn also
```
ls /opt/news.git | xargs
```
xargs → Builds and executes command lines from standard input.
-n 1 → Pass 1 argument per command execution






