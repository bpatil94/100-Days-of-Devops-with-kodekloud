# Day #31 task: Git Stash
The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/ecommerce present on Storage server in Stratos DC. One of the developers stashed some in-progress changes in this repository, but now they want to restore some of the stashed changes. Find below more details to accomplish this task:

Look for the stashed changes under /usr/src/kodekloudrepos/ecommerce git repository, and restore the stash with stash@{1} identifier. Further, commit and push your changes to the origin.

- infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details
- some usefull gir stash command
  1. git stash: Is a Git command used to temporarily shelf (save) uncommitted changes so you can work on something else, then come back and reapply them later.
  2. git stash list: Displays all your saved stashes with their corresponding index identifiers (e.g., stash@{0}).
  3. git stash pop: Reapplies the most recently stashed changes and deletes them from the stash list.
  4. git stash apply: Reapplies the changes but keeps them safely stored in your stash list.
  5. git stash drop stash@{index}: Deletes a specific stash from your list.
  6. git stash clear: Permanently deletes all stashes in your local repository.
 

## Step 1 — Connect to Storage Server
```
ssh natasha@ststor01
```
<img width="715" height="150" alt="image" src="https://github.com/user-attachments/assets/e5db5a23-e64e-4e55-aab1-f3043f0fd0b1" />

## Step 2 — Navigate to the repository and make it as safe
```
cd /usr/src/kodekloudrepos/ecommerce
```
```
git config --global --add safe.directory /usr/src/kodekloudrepos/ecommerce
```
<img width="837" height="290" alt="image" src="https://github.com/user-attachments/assets/b1caaf74-2feb-4fa2-a790-50262dc7cb3b" />

## Step 3 — List all stashed changes to verify stash@{1} exists
```
sudo git stash list
```
 <img width="649" height="186" alt="image" src="https://github.com/user-attachments/assets/9d3c2831-f010-44a7-866f-6592f18ee1ba" />

## Step 4 — Restore the stash with identifier stash@{1}
```
sudo git stash apply stash@{1}
```
<img width="491" height="175" alt="image" src="https://github.com/user-attachments/assets/9941b22c-3224-45d5-9649-c2699b4b0472" />

## Step 5 — Add all changes to staging
```
sudo git add welcom.txt
```
<img width="660" height="170" alt="image" src="https://github.com/user-attachments/assets/3ea571c2-b5a3-4bb8-8c95-c54f387c6c9f" />

## Step 6 — Commit the changes
```
sudo git commit -m " committed stashed changes "
```
<img width="659" height="260" alt="image" src="https://github.com/user-attachments/assets/a7c5f4da-46c3-4eaf-8249-a0adb0d9ca7d" />

## Step 7 — Push changes to origin
```
sudo git push origin master
```
<img width="477" height="187" alt="image" src="https://github.com/user-attachments/assets/8f0d1b03-cb80-4c1a-b361-978dde9091b8" />

## Step 8 — Validation
```
git status
```
<img width="486" height="139" alt="image" src="https://github.com/user-attachments/assets/6dfda03b-e50f-4c29-8fb8-63f4fc887ede" />
