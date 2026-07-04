# Day #22 task: Clone Git Repository on Storage Server

The DevOps team established a new Git repository last week, which remains unused at present. However, the Nautilus application development team now requires a copy of this repository on the Storage Server in the Stratos DC. Follow the provided details to clone the repository:

The repository to be cloned is located at /opt/games.git

Clone this Git repository to the /usr/src/kodekloudrepos directory. Perform this task using the natasha user, and ensure that no modifications are made to the repository or existing directories, such as changing permissions or making unauthorized alterations.

- Infrastructure details:
https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## Note
- Git Fork: Copies a remote repository to your own cloud account (e.g., GitHub).
- Git Clone: Downloads a remote repository onto your local computer.
- Git Pull: Fetches and merges the latest updates from a remote repository into your local files.

  <img width="585" height="385" alt="image" src="https://github.com/user-attachments/assets/bf4bc5a6-d329-4948-bb09-8b8e3fcf6220" />

## step1: ssh to storage server.
  in above case storage server user is natasha, ssh to that with password

```
ssh natasha@ststor01
```
## step2: see those specified .git and directories are there or not
```
cd /opt/games.git
```
```
cd /usr/src/kodekloudrepos
```
## step3: perform clone in /usr/src/kodekloudrepos
```
pwd
```
if it is mentioned path i.e /usr/src/kodekloudrepos
```
git clone /opt/games.git
```

## step4: validation
list out the files in /usr/src/kodekloudrepos
```
ls -lart
```
get into games
```
cd games/
```
check the git status
```
git status
```
## or 
list out the things in games, see .git is present or not
```
ls -lart
```
if .git is present, task is sucessfull




