# Day #27 task: Git Revert Some Changes
The Nautilus application development team was working on a git repository /usr/src/kodekloudrepos/beta present on Storage server in Stratos DC. However, they reported an issue with the recent commits being pushed to this repo. They have asked the DevOps team to revert repo HEAD to last commit. Below are more details about the task:


1. In /usr/src/kodekloudrepos/beta git repository, revert the latest commit ( HEAD ) to the previous commit (JFYI the previous commit hash should be with initial commit message ).


2. Use revert beta message (please use all small letters for commit message) for the new revert commit.

- Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

  
## Step 1 — Connect to Storage Server
```
ssh natasha@ststor01
```
<img width="710" height="205" alt="image" src="https://github.com/user-attachments/assets/3b880176-2d7a-423f-81d2-e025da6fdd2d" />

## Step 2 — Go to the repository directory and make it as safe and check the git log
```
cd /usr/src/kodekloudrepos/beta
```
<img width="567" height="115" alt="image" src="https://github.com/user-attachments/assets/47a80786-d9c3-4263-beba-aebf52f398c3" />

```
sudo git config --global --add safe.directory /usr/src/kodekloudrepos/beta
```
<img width="896" height="78" alt="image" src="https://github.com/user-attachments/assets/5b03970e-9627-447c-aec1-57446534eeaa" />

```
sudo git log
```
<img width="687" height="276" alt="image" src="https://github.com/user-attachments/assets/9b5f65f8-4456-4c30-b3f6-efc19c038250" />

## Step 3 — Revert the latest commit (HEAD)
```
git revert HEAD
```
<img width="568" height="115" alt="image" src="https://github.com/user-attachments/assets/00faf058-1e7b-4bf9-81df-11cd432d8ffa" />

## Step 4 — Commit message and check logs
- The git commit --amend command is a convenient way to modify the most recent commit in your current branch.
```
sudo git commit --amend -m "revert beta"
```
<img width="673" height="131" alt="image" src="https://github.com/user-attachments/assets/5f21caea-be17-489f-a802-b4b22e0c56eb" />

```
sudo git log
```
<img width="777" height="384" alt="image" src="https://github.com/user-attachments/assets/198d63d4-b6bb-4e26-9358-8036f09663a6" />

