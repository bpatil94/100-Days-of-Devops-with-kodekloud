# Day #26 task: Git Manage Remotes

The xFusionCorp development team added updates to the project that is maintained under /opt/apps.git repo and cloned under /usr/src/kodekloudrepos/apps. Recently some changes were made on Git server that is hosted on Storage server in Stratos DC. The DevOps team added some new Git remotes, so we need to update remote on /usr/src/kodekloudrepos/apps repository as per details mentioned below:


a. In /usr/src/kodekloudrepos/apps repo add a new remote dev_apps and point it to /opt/xfusioncorp_apps.git repository.


b. There is a file /tmp/index.html on same server; copy this file to the repo and add/commit to master branch.


c. Finally push master branch to this new remote origin.

infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## Step-1: SSH into the storage server and create the new remote and point it to the mentioned path.
```
ssh natasha@ststor01
```
- switch to root user
```
sudo su
```
- cd to mentioned path
```
cd /usr/src/kodekloudrepos/apps
```
- check the git branches and the current pointing branch
```
git branch
```
- add mentioned remote and point to mentioned path
```
git remote add dev_apps /opt/xfusioncorp_apps.git
```
## Step-2: Copy the /tmp/index.html file to this path in master branch and push it to the new remote repo location.

```
cp /tmp/index.html .
```
- see status, add/commit changes to master branch, re-check the status
```
git status
```
```
git add .
```
```
git commit -m "added index.html file"
```
```
git status
```
- push the changes to dev_apps remote repo
```
git push dev_apps master
```


