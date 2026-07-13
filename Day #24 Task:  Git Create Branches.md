# Day #23 task:  Git Create Branches
Nautilus developers are actively working on one of the project repositories, /usr/src/kodekloudrepos/blog. Recently, they decided to implement some new features in the application, and they want to maintain those new changes in a separate branch. Below are the requirements that have been shared with the DevOps team:



On Storage server in Stratos DC create a new branch xfusioncorp_blog from master branch in /usr/src/kodekloudrepos/blog git repo.


Please do not try to make any changes in the code.




## Step 1 — Connect to Storage Server
First, connect to the target server using a regular user account( in our case its natasha)
```
ssh natasha@ststor01
```

## Step 2 — Mark directory as safe (only needed once)
```
git config --global --add safe.directory /usr/src/kodekloudrepos/blog
```

### git config
- Used to view or change Git configuration settings.
### --global
- Saves the setting in your global Git configuration (~/.gitconfig).
- It applies to the current user for all Git repositories.
### --add
- Adds a new value to a configuration key.
- This is useful because safe.directory can contain multiple repository paths.
### safe.directory
- A Git security setting introduced to prevent Git from operating on repositories owned by another user unless you explicitly trust them.
- Without this setting, you might see an error like:

## Step 3 —Change to the repo
Make sure natasha owns the repo (recommended):
```
sudo chown -R natasha:natasha /usr/src/kodekloudrepos/blog
```
- Why: this prevents permission problems and avoids needing sudo for Git.
- Change to the repo and ensure you’re on master.

```
cd /usr/src/kodekloudrepos/media
```
```
sudo git checkout master
```
## Step 4 — Create the new branch and switch to it
```
sudo git checkout -b xfusioncorp_blog
```

## Step 5 — Verify
```
git branch
```



