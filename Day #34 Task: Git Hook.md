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
<img width="625" height="115" alt="image" src="https://github.com/user-attachments/assets/192fd977-e51b-4c70-a355-6fe7521bfcdd" />

## Step-4: Merge the feature branch to your local master and validate whether the release is tagged.



<img width="594" height="244" alt="image" src="https://github.com/user-attachments/assets/016b1809-184d-477f-a94c-b39d020cd241" />

<img width="542" height="284" alt="image" src="https://github.com/user-attachments/assets/bcb23839-2d50-4481-b1a4-6c27da71733f" />


