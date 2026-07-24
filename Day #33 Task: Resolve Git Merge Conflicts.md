# Day #33 task: Resolve Git Merge Conflicts

Sarah and Max were working on writting some stories which they have pushed to the repository. Max has recently added some new changes and is trying to push them to the repository but he is facing some issues. Below you can find more details:


SSH into storage server using user max and password Max_pass123. Under /home/max you will find the story-blog repository. Try to push the changes to the origin repo and fix the issues. The story-index.txt must have titles for all 4 stories. Additionally, there is a typo in The Lion and the Mooose line where Mooose should be Mouse.


Click on the Gitea UI button on the top bar. You should be able to access the Gitea page. You can login to Gitea server from UI using username sarah and password Sarah_pass123 or username max and password Max_pass123.


Note: For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.

Infrastructure Details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

Gitea UI: https://3000-port-bqnbjxr4khcpbwzf.labs.kodekloud.com/

## Step 1 — Connect to Storage Server
```
ssh max@ststor01
```
<img width="597" height="117" alt="image" src="https://github.com/user-attachments/assets/114ea212-d99e-425e-9e04-d2cc83d9037b" />


## Step 2 — Navigate to the repository
```
cd /home/max/story-blog/
```
<img width="792" height="222" alt="image" src="https://github.com/user-attachments/assets/81b2abf3-a748-4480-a72f-487df001569c" />

## Step 3: Open the file with an editor
```
vi story-index.txt
```
<img width="644" height="96" alt="image" src="https://github.com/user-attachments/assets/c6719bc1-8b3e-4ad6-a0b8-e3fb9b629be8" />

## Step 4 — Pull with rebase from origin
```
git pull origin master --rebase
```

## Step 5 — Fix the conflict manually
- Remove lines with <<<<<<<, =======, >>>>>>>
- Keep all 4 story titles
- Fix typo: “Mooose” → “Mouse”
- Make sure you have 4 complete story titles

```
vi story-index.txt
```
<img width="524" height="163" alt="image" src="https://github.com/user-attachments/assets/2c1eae78-e527-478b-93be-f8aefb9ff374" />

## Step 6 — Add the resolved file and Continue rebase
```
git add .
```
```
git commit -m "adding file"
```
<img width="720" height="144" alt="image" src="https://github.com/user-attachments/assets/9a6f4c94-35e6-43da-b543-ebf2915aaf94" />

## Step 7—Push changes
```
git push origin master
```
<img width="718" height="213" alt="image" src="https://github.com/user-attachments/assets/508c6b10-5edb-4b53-9aa4-1905200fe118" />

## Step 8 — Check changes
verigy in Gitea UI page

<img width="1358" height="411" alt="image" src="https://github.com/user-attachments/assets/79d38783-fe5f-4bd4-afc1-ccb7480ed735" />

