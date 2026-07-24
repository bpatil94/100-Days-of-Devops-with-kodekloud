# Day #33 task: Resolve Git Merge Conflicts







Infrastructure Details: 

Gitea UI: 

## Step 1 — Connect to Storage Server
```
ssh max@ststor01
```

## Step 2 — Navigate to the repository
```
cd /home/max/story-blog/
```

## Step 3: Open the file with an editor
```
vi story-index.txt
```

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
## Step 6 — Add the resolved file and Continue rebase
```
git add story-index.txt
```
```
git rebase --continue
```

## Step 7—Push changes
```
git push origin master
```
## Step 8 — Check changes

