# Day #30 task: Git hard reset

The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/blog present on Storage server in Stratos DC. This was just a test repository and one of the developers just pushed a couple of changes for testing, but now they want to clean this repository along with the commit history/work tree, so they want to point back the HEAD and the branch itself to a commit with message add data.txt file. Find below more details:



1. In /usr/src/kodekloudrepos/blog git repository, reset the git commit history so that there are only two commits in the commit history i.e initial commit and add data.txt file.


2. Also make sure to push your changes.






- infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details
- Note
   1. The git reset command is used to undo changes by moving your current branch (HEAD) to a specific previous commit. It alters your commit history, staging area, and local working directory depending on the flag you use
 
      - 1. Unstaging a Staged File
           If you ran git add but do not want to include that file in your next commit
           " git reset <file-name> "

        2. Undo the Last Commit (Keep Your Work)
           To safely undo your last commit without losing any of your actual code changes
           " git reset --soft HEAD~1"
           
        3. Undo the Last Commit (Unstage Your Work)
           To undo the last commit and completely unstage your changes, while preserving your code files
           " git reset HEAD~1 "

        4. Permanently Destroy the Last Commit and All Changes
           To delete your last commit and permanently wipe out any code modifications associated with it
           " git reset --hard HEAD~1 "

## Step 1 — Connect to Storage Server
```
ssh natasha@ststor01
```
<img width="585" height="185" alt="image" src="https://github.com/user-attachments/assets/2bed593a-8476-4a94-be49-74ce35a9aec6" />

## Step 2 — Go to the repository mentioned in task statement and secure that path
```
cd /usr/src/kodekloudrepos/blog
```
```
git config --global --add safe.directory /usr/src/kodekloudrepos/blog
```
<img width="767" height="256" alt="image" src="https://github.com/user-attachments/assets/f6fb5bab-183c-4820-be08-a827fcd8a639" />

## Step 3 — see the logs and commit hash for the message “add data.txt file”
```
git log --oneline
```
<img width="426" height="263" alt="image" src="https://github.com/user-attachments/assets/592375c3-2be8-4d24-9a43-29763cf48cd7" />

## Step 4 — Reset the branch so that the HEAD points to that commit
```
git reset --hard 89aab14
```
<img width="701" height="180" alt="image" src="https://github.com/user-attachments/assets/5a294d5e-9495-42a2-9407-54d803befe22" />

## Step 5 - push the changes to origin
```
git push origin master --force
```
- Because history changed, a force push is required:
```
git status
```
<img width="680" height="144" alt="image" src="https://github.com/user-attachments/assets/949004c3-c2c1-4f67-be5d-4fe3ef9b9fe1" />

           

           
