# Day #25 task: Git Merge Branches

The Nautilus application development team has been working on a project repository /opt/blog.git. This repo is cloned at /usr/src/kodekloudrepos on storage server in Stratos DC. They recently shared the following requirements with DevOps team:



Create a new branch devops in /usr/src/kodekloudrepos/blog repo from master and copy the /tmp/index.html file (present on storage server itself) into the repo. Further, add/commit this file in the new branch and merge back that branch into master branch. Finally, push the changes to the origin for both of the branches.



Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## Step-1: SSH into the storage server.
```
ssh natasha@ststor01
```
- swith to root user
```
sudo su
```
- cd to the given directory and validate whether you are in the master branch.
```
cd /usr/src/kodekloudrepos/blog
```
```
git branch
```
- Mark directory as safe (only needed once)
```
git config --global --add safe.directory /usr/src/kodekloudrepos/blog
```

## step-2: Create the new branch.
- check the branch status
```
git branch
```

- create the branch and switch
```
git checkout -b devops
```

## step-3: Validate the current branch.
```
git branch
```

## step-4: Copy the file from the /tmp folder and add and commit your changes to the newly created branch.
```
cp /tmp/index.html .
```
- verify the things, check the brach status do add and commit , confirm with git status
```
ls -larth
```
```
git status
```
```
git add .
```
```
git commit -t "added index.html file"
```
```
git status
```

## Note
You can do push and merge easily with the below steps.

1.Commit your current changes to the **A** branch.
2.Push your changes to the GitHub.
3.Switch branch from **A** to the ** master** branch in your local computer.
4.Then merge the **master** branch from branch** A**.
5.Finally push your merge into the GitHub.

## step-5: pushing and merging the things to origin
- push devoper branch to git hub
```
git push origin developer
```
- chevkout to master branch
```
git checkout master
```
- merge the changes
```
git merge developer
```
- push back to origin
```
git push origin main
```


## step-6: validation
<img width="475" height="239" alt="image" src="https://github.com/user-attachments/assets/044d7bed-de42-4d9d-8cc1-deb47392448e" />


