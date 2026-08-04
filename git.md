git init
git add .
git status
git diff report.md
git diff --staged report.md
git commit -m "Stage project updates"
git log -3
git log --since='' --until=''
git log -2 mental_health_survey.csv
git show c27fa586
git revert HEAD
git revert HEAD --no-edit
git revert HEAD -n
git checkout HEAD~1 -- report.md
git restore --staged report.md
git restore --staged



### The commit structure

1. Commit
- contains the metadata - author, log message, commit time

2. Tree
- tracks the names and locations of files and directories when that commit happend
- like a dictionary - mapping keys to files/directories

3. Blob (binary large object)
- may contain data of any kind
- a compressed snapshot of a file's contents

### Git hash

A 40-character string of number and letters. 

For each commit.

- pseudo-random number generator - hash function
- hashed allow data sharing between repos
    - Git only needs to compare hashed. If two files are the same, their hases are the same. 

### Git log

Viewing commit information using the git log command, displaying all commits made to the repo in reverse chronological order, starting with the most recent commit.
It will show the commit hash, author, date, and commit message. If the output doesn't fit in the terminal window, a colon will be at the end, indicating more commits. We can se more my pressing the space bar.
To exit the log, we can press q and return to the terminal.

Customizing the git log:

- we can restrict the number of commits displayed using -
    - git log -3

- to only look at the commit history of one file
    - git log report.md

- combining
    - git log -2 mental_health_survey.csv

- restrict by date - git log --since='YYYY-MM-DD'
    - git log --since=''
    - git log --since='' --until=''

#### Finding a particular commit

Suppose we have observed issues since a particular date. We need to see what changed since that day. We run git log and note the commit hash of interest.
Only need the first 8-10 characters of the hash. We can then use:

git show c27fa586

The output displays the log entry for that commit at the top, followed by a diff output showing changes between the file in that commit and the current version in the repo.

### Comparing versions

The git show contains a diff section

We can perform comparisons using the git diff command

#### Comparing to an unstaged file
Here we compare the last commited version with a modified version that is not in the staging area by running 

    git diff report.md

Version B is generally the newest version. The line starting with two @@ symbols tells us what changed between the two versions. 

Red text starting with a minus symbol representing a line in version A that is not in version B. 
Green text starting with a plus symbol: added in version B and is not in version A.

#### Comparing to a staged file
We just add --staged flag:

    git diff --staged report.md

Same output. 

#### Comparing multiple staged file
If we omit the file name we'll get a comparison of all staged files versus their last committed versions.
    
    git diff --staged

### Comparing versions between two commits

2 options

Finding the commit hashed with:

    git log

then compare them with 

    git diff 5fg67h h8j9k
    
Git will show what changed from the first commit to the second one. So we put the most recent last.

OR

The word HEAD, can be used to refer to the msot recent commit. To look at older commits we can reference them to HEAD by using a tilde, then a number. To compare the second most recent commit with the latest commit, we replace the first hash with HEAD~1 and the second hash with HEAD.

    git diff HEAD~1 HEAD
    
## Restoring and reverting files

### Reverting files

Suppose we've modified a file, added it to the staging area, and made a commit. However, we realize the last version has a type. We need to undo our most recent changes, restoring this file to the state before the last commit. 

    git revert

This reinstates previous versions AND makes a new commit with these older versions. It will restore all files updated in that commit. We need to provide a reference, through a commit hash or HEAD, to the changes that we want to undo
Undoing the most recent commit:

    git revert HEAD

Running this, a text editor will open where we are prompted to add a commit message. If we want to avoid opening the text editor:

    git revert --no-edit HEAD

Revert without commiting (bring files into the staging area):

    git revert -n HEAD

### Reverting a single file 

git revert works on commits, not individual files
To revert a single file:

    git checkout

Versitile command. 

    git checkout HEAD~1 -- report.md

Here we refer to the second most recent commit and narrow the command to a single file. We won't get an output but we can confirm the file has been modified to its previous state and is in the staging area by running git status. We then make a new commit to save the file's latest state.

    git status

    git commit -m "Checkout previous version of report.md"

What if we need to restore a file in the staging area before the update has been committed?

#### Unstaging a file

We need to remove this file from the staging area or, to put it another way, unstage it so its back in the repo.

    git restore --staged summary_statistics.csv

This will move the file back to the working directory, allowing us to edit it before restaging and commiting. 

To unstage all files:

    git restore --staged
