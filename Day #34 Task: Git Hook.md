# Day #34 task: Git Hook
  The Nautilus application development team was working on a git repository /opt/cluster.git which is cloned under /usr/src/kodekloudrepos directory present on Storage server in Stratos DC. The team want to setup a hook on this repository, please find below more details:



- Merge the feature branch into the master branch, but before pushing your changes complete below point.

- Create a post-update hook in this git repository so that whenever any changes are pushed to the master branch, it creates a release tag with name release-2023-06-15, where 2023-06-15 is supposed to be the current date. For example if today is 20th June, 2023 then the release tag must be release-2023-06-20. Make sure you test the hook at least once and create a release tag for today's release.

- Finally remember to push your changes.
Note: Perform this task using the natasha user, and ensure the repository or existing directory permissions are not altered.

- infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details
- note
   What is a Git Hook?
A Git hook is a script that Git automatically runs when a specific Git event occurs, such as a commit, a push, or a merge.

In our problem statement, we will create a server-side Git hook in the central demo.git repository. This hook will automatically create a release tag on the master branch whenever master is updated by a push from any developer.


## Step 1 — Connect to Storage Server
```
ssh natasha@ststor01
```
<img width="789" height="139" alt="image" src="https://github.com/user-attachments/assets/2f80a1b3-5ebf-4321-8ec1-8df33e82eda7" />

## Step-2: create a file in the hooks folder in the central repository.
" Hooks should be under bare repository instead of local repo "
- A working repository is the developer's local clone.
- A bare repository is typically the central remote repository (for example, /opt/project.git) that receives git push requests from developers.
- Bare RepositoryA bare repository contains only the Git administrative data and version history, completely omitting a working tree. It is typically used as a central server repository (like on GitHub or a self-hosted server).
  
```
cd /opt/cluster.git/hooks
```
<img width="547" height="287" alt="image" src="https://github.com/user-attachments/assets/902bc174-1f70-467b-bd84-a850a05f79bd" />

## step3 : You need to create the post-update hook/script in this path.
- /opt/cluster.git/hooks/
```
vi post-update
```
```
#!/bin/bash

# Loop through all refs that were updated
for ref in "$@"
do
    # We only care about updates to the master branch
    if [ "$ref" = "refs/heads/master" ]; then

        # Get today's date in YYYY-MM-DD format
        TODAY=$(date +%F)

        # Define release tag name
        TAG_NAME="release-$TODAY"

        # Get the latest commit hash on master
        COMMIT=$(git rev-parse refs/heads/master)

        # Create the tag (ignore error if tag already exists)
        git tag "$TAG_NAME" "$COMMIT" 2>/dev/null
    fi
done
```
- give them the executable permissions to those files

<img width="625" height="115" alt="image" src="https://github.com/user-attachments/assets/192fd977-e51b-4c70-a355-6fe7521bfcdd" />

## Note
1. Shebang.
   "#!/bin/bash"
   - This tells the system to run this script with Bash.
   - Every hook needs a shebang so the shell knows how to execute it.
3. Loop through updated refs.
   "for ref in "$@"
    do"
   - "$@" contains all branch references that were updated by the push.
   - The loop lets us check each updated branch individually.
   - ref is just a variable representing the current branch in the loop.
4. Only care about master.
   "if [ "$ref" = "refs/heads/master" ]; then"
   - We only want to create a release tag when master is updated.
   - refs/heads/master is the full Git reference name for the master branch.
   - Any other branch (a.k.a: feature.) is ignored.
5.  Get today’s date.
   "TODAY=$(date +%F)"
   - date +%F outputs today’s date in YYYY-MM-DD format.
   - Example: 2026-07-27
   - This lets us name the release tag dynamically based on the current date.
5.  Define the tag name.
  " TAG_NAME="release-$TODAY" "
   - Concatenates release- with today’s date.
   - Example: release-2026-07-27
   - This will be the name of the Git tag.
6.  Get the latest commit on master.
  "COMMIT=$(git rev-parse refs/heads/master)"
   - git rev-parse <ref> returns the commit hash that a ref points to.
   - Here, it gets the latest commit on master in the bare repo.
   - This ensures the tag points to the correct commit.
7.  Create the tag.
  "git tag "$TAG_NAME" "$COMMIT" 2>/dev/null"
  - Creates a lightweight tag named release-YYYY-MM-DD pointing to the latest master commit.
  - 2>/dev/null ignores errors, e.g., if the tag already exists (so the push doesn’t fail).
8.  End of loop
    "fi
     done"
    - Closes the if and for blocks.
    - Before we move on to the next step, you also need to make the script in the executable mode.

## Step-4: Merge the feature branch to your local master and validate whether the release is tagged.
- navigate to working directory
```
  cd /usr/src/kodekloudrepos/cluster/
```
- check the branch and checkout to master
```
git branch
```
```
gir checkout master
```
- merge the feature branch with master and push to origin
```
git merge feature
```
```
git push origin master
```
<img width="594" height="244" alt="image" src="https://github.com/user-attachments/assets/016b1809-184d-477f-a94c-b39d020cd241" />

- see the status of tags

<img width="542" height="284" alt="image" src="https://github.com/user-attachments/assets/bcb23839-2d50-4481-b1a4-6c27da71733f" />


