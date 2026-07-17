# Day 28: Git Cherry Pick

The Nautilus application development team has been working on a project repository /opt/ecommerce.git. This repo is cloned at /usr/src/kodekloudrepos/ecommerce on storage server in Stratos DC. They recently shared the following requirements with the DevOps team:



There are two branches in this repository, master and feature. One of the developers is working on the feature branch and their work is still in progress, however they want to merge one of the commits from the feature branch to the master branch, the message for the commit that needs to be merged into master is Update info.txt. Accomplish this task for them, also remember to push your changes eventually.


- Infrastucture details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## Note:
- <img width="623" height="355" alt="image" src="https://github.com/user-attachments/assets/29c1a755-5706-439d-a169-64546dab2c14" />
- <img width="657" height="412" alt="image" src="https://github.com/user-attachments/assets/fe021ab1-96a3-41f8-9a21-653805b0b0cc" />



## Step 1 — Connect to Storage Server
```
ssh natasha@ststor01
```
<img width="645" height="144" alt="image" src="https://github.com/user-attachments/assets/095d48c5-38e8-41ae-a275-c2adc7831ea2" />

- switch as root
```
sudo su
```
## Step 2 — cd to change the directory and Check which branch you are currently on
```
cd /usr/src/kodekloudrepos/ecommerce
```
<img width="513" height="41" alt="image" src="https://github.com/user-attachments/assets/38da0a89-849c-434d-bea6-4fbd0b74952d" />

```
git branch
```
<img width="527" height="143" alt="image" src="https://github.com/user-attachments/assets/471b2a02-9625-463e-873e-7b9a2624a02c" />


## Step 3 — List all commits

```
git log feature
```
<img width="735" height="452" alt="image" src="https://github.com/user-attachments/assets/1968365c-c13d-486c-bd76-7554825a5eb3" />

## Step 4 — Switch to the master branch
```
git checkout master
```
<img width="549" height="119" alt="image" src="https://github.com/user-attachments/assets/480f08ce-f5fb-4485-9d29-fd4bfcf5d9a4" />

## Step 5 — Cherry-pick that specific commit into master
- add the commit number which we want to cherry pick
```
git cherry-pick 29e1113
```
<img width="589" height="82" alt="image" src="https://github.com/user-attachments/assets/3de80d6b-10a1-4314-b922-953024811afa" />

## Step 6 — Push the changes to the remote repository
```
git push origin master
```
<img width="698" height="179" alt="image" src="https://github.com/user-attachments/assets/8ff5c047-8d3e-4ee6-8a98-1ebb8ccee755" />

## step 7 - validate
```
git status
```
<img width="721" height="237" alt="image" src="https://github.com/user-attachments/assets/ff5f64d3-cd5c-44d5-9e3b-8a40bb0ded1b" />
