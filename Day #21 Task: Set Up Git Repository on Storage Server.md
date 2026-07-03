# Day 21 task: Set Up Git Repository on Storage Server

The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the Storage server in the Stratos DC:

1.Utilize yum to install the git package on the Storage Server.

2.Create a bare repository named /opt/official.git (ensure exact name usage).

## note: 
The core difference between a standard Git repository and a bare repository is the presence of a working directory and checked-out files.A standard repository is designed for active development, while a bare repository is meant purely for storage, sharing, and serving as a central hub.
Standard (Non-Bare) RepositoryA standard repository contains both the internal Git database and your actual project files.
#### Working Directory: The sandbox folder on your computer where your files live. You can directly see, open, and edit your source code here.
Checked-Out Files: The specific version or branch of files currently extracted from the Git history into your working directory.
The .git Folder: A hidden subdirectory inside your project root containing the full version history, commits, configurations, and the staging area.

news/
├── .git/
├── README.md
├── app.py
└── src/

#### Bare Repository (--bare)A bare repository is a Git repository that does not have a working directory.
Structure: It consists only of the administrative and history files that you would normally find hidden inside a .git folder.
No Checked-Out Files: Because there is no working directory, you cannot see your source code files directly, edit them, or run commands like git add or git status normally.
Purpose: It is used on central servers (like GitHub, GitLab, or your own private server) to safely receive git push operations without risk of overriding a developer's active working files.

A bare repository is a Git repository that contains only the version control data and does not have a working directory (checked-out files).

What's inside a bare repository?
A bare repository contains the contents that would normally be in the .git directory of a regular repository:
Commit history
Branches
Tags
Git objects
Configuration files

news.git/
├── HEAD
├── objects/
├── refs/
└── config

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
-xargs → Builds and executes command lines from standard input.
--n 1 → Pass 1 argument per command execution






