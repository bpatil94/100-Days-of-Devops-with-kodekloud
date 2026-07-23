# Day #32 task: Git Rebase
The Nautilus application development team has been working on a project repository /opt/demo.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:



One of the developers is working on feature branch and their work is still in progress, however there are some changes which have been pushed into the master branch, the developer now wants to rebase the feature branch with the master branch without loosing any data from the feature branch, also they don't want to add any merge commit by simply merging the master branch into the feature branch. Accomplish this task as per requirements mentioned.


Also remember to push your changes once done.


infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## 1️⃣ Connected to the Storage Server
```
ssh natasha@ststor01
```
<img width="739" height="142" alt="image" src="https://github.com/user-attachments/assets/79594a83-ebb5-4221-971b-b5a117aacb6d" />

## 2️⃣ Navigated to the Repository
```
cd /usr/src/kodekloudrepos
```
<img width="712" height="65" alt="image" src="https://github.com/user-attachments/assets/ed7efaba-e830-436c-98df-670cd7d4241c" />

## 3️⃣ Handled Git Safe Directory & Permission Issues
Sometimes in shared environments, Git warns about “dubious ownership.” I resolved it by marking the directory as safe:
```
git config - global - add safe.directory /usr/src/kodekloudrepos/demo
```
<img width="740" height="21" alt="image" src="https://github.com/user-attachments/assets/3fc85097-11b3-4dbe-ac4f-499a34fda199" />

When permission issues arose, I elevated privileges with sudo to proceed safely.

## 4️⃣ Fetched the Latest Changes to master( make sure doing in master branch)
```
sudo git fetch origin
```
 <img width="723" height="307" alt="image" src="https://github.com/user-attachments/assets/b6d9b40c-ba93-4b3f-a6ca-bd608b98202c" />

## 5️⃣ Switched to the Feature Branch
```
sudo git checkout feature
```
<img width="730" height="32" alt="image" src="https://github.com/user-attachments/assets/d414e328-260f-4220-a7eb-47ce967198c7" />

## 6️⃣ Rebased Feature onto Master
```
sudo git rebase master
```
<img width="732" height="33" alt="image" src="https://github.com/user-attachments/assets/07604caf-e284-4b20-86a0-a816b506df74" />

- This moved all feature branch commits on top of the latest master commits — no merge commits, cleaner history!
## 7️⃣ Pushed Changes with Safety
```
sudo git push --force-with-lease origin feature
```
<img width="736" height="270" alt="image" src="https://github.com/user-attachments/assets/5116b186-f9e8-4a4b-be2b-ef868f34e2d1" />

 

- Used --force-with-lease instead of — force to prevent overwriting unexpected changes from others.


- Note:
  1. git rebase is great for maintaining a clean history — but requires careful use, especially on shared branches.
  2. Always fetch before rebasing to ensure you’re working with the latest state.
  3. — force-with-lease is safer than a blind — force push.
