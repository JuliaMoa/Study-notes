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

git diff main chatbot
git branch
git branch -m old_name new_name
git branch -d chatbot
git branch -D chatbot




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

# Working with branches
Branch= an individual version of a repo. Git uses branches to systematically track multiple versions of files. 

By default, every repo has a branch called main. Generally, this is where we store our working application. In a seperate branch, we can develop and test a new feature. 
Branches allow multiple developers to work on a project simultaneosly. Git makes it easy to compare the state of a repo between branches, and to combine contents, pushing new features to a live system. Generally, each branch should have a specfic purpose.

Listing branches, * = current branch

    git branch 

Swtiching between branches

    git switch main

Creating a new branch

    git branch command speed-test

create a new branch and switch to it

    git switch -c speed-test

comparing branches

    git diff main summary-statistics

Navigate large git diff outputs with space to progress and q to exit.

Rename . Use descriptive, consistent naming conventions like including ticket numbers.

    git branch -m current_name new_name

Deleting a branch - wise to do after being finished with them. 

    git branch -d branch_name

If we haven't merged the branch into main the -d flag will produce an error. Still possible with -D flag instead. Beware - it is not easy to recover a deleted branch.

## Merging

Once the task of the branch has been completed, we need to incorporate these changes into the live system, typically hosted in the main branch. 

When merging two commits, the last commits form each branch are called parent commits. The branch we want to merge from is known as source. The branch we want to merge into is the destination.

First switch to the destination branch.

    git switch main

    git merge ai-assistant

If we are in a different branch we can still perform a merge, but need to include the source and destination branches in that order

    git merge source destination

Git merge output: first we can see the first seven characters from each branch's latest commit hashes at the top. These are the parent commits. Next, we see the type of commits, ex: Fast-forward: linear commit history, branched off main to create ai-assistand. 
The output also shows the number of lines added or deleted per file. The final line shows the phrase "create mode", indicating a new file has been created.

### Collaborations 

Merge conflicts - a scenario where Git is unable to resolve differences in contents of one or more files between branches. For example: if we edit the same file in two different branches, Git won't know which version should be kept.

Open the file from the terminal using the Nano command

    nano README.md

Git has added characters to highlight where the conflict occurs. Git shows us how the contents differ between branches. We remove the conflict syntac and make sure the file looks like it should, then save.

Then add and commmit. Then try to merge again. Remember: we should avoid editing the same file in multiple branches. Prevention is better than cure when it comes to git conflicts.

## Intro to remotes

Local repo - on our computer. 
Remote repo - stored in the cloud, for example on GitHub.
Local remotes - repo copies on our computer or network. It can be useful for collaboration where cloud storage is prohibited such as in enterprises with strict data privacy rules. 

Here we clone/copy a local repo

    git clone /home/george/repo

Cloning and naming a local project

    git clone /home/geroge/repo new_repo

While remotes can exist locally it is more common to store them in an an online hosting service. We can clone a remote repo on to our local computer. We do it with a URL instead of a local path: 

    git clone https://github.com/datacamp/project

When we clone a repo, Git remembers where the original was. It does it by storing a remote tag in the new repo's configuraiton. If we are in a repo, we can list the names of its remotes like this:

    git remote

If we want more information, such as the remotes ULRs, we can add the -v flag, which stands for 

    git remote -v

When cloning, Git will automatically name the remote as origin. We can add more remotes by specifying a name for Git to assign. 

    git remote add george https://github.com/george_datacamp/repo

It is useful to define the remote name as we can use it as a shortcut when merging rather than listing the URL or path.

Let's look at how to get content from a remote into our local repo. Say we've been working in our local repo, nut others have been working the remote. If several people are collaborating on a project then they will access the remote work locally, save files, and synchronize their changes between the remote and local repos. This means that the remote repo should be the project's source of truth, where the latest versions of ifiles that are not drafts can be located. To synchronize the files in a remote to a local repo we need to fetch branches from the remote. 

    git fetch origin

This fetches all branches from the remote into the local repo. If a branch only exists in the remote it will be created in the local repo as a result of this command. Note this doesn't merge the remote repo's contents into our local repo. 
If we want to fetch from a particular branch only, we specify it after the remote name. Here origin is the remote name, main is the branch name.

    git fetch origin main 

After fetching we need to synchronize content between the remote and local repos. To do this we perform a merge of the remote into our local repo. If we don't specify a local branch, the merge will be from the remote's main branch into the local branch that we are currently located in.

    git merge origin 

The output is the same as if we merged two local branches.

Local and remote synchornization is a common workflow. Git simplifies this process for us by allowing us to fetch and merge using a single command:

    git pull origin

This will fetch and merge from the remote's default (main) into the local repo's current branch. As with git fetch, we can specify which remote branch to pull from by including the branch name after the remote name.

    git pull origin dev

Note that if we have been working locally and not yet committed our changes, then Git won't allow us to pull from the remote! 

To recap, we use git pull to gather content from a remote and merge it into our local repo. The opposite of pulling is pushing, where we merge our local's repo content into a remote. 
As with pulling contents from a remote, we need to commit local changes before we can push to the remote. 

    git push prigin main

If we push before pull: if we don't pull from the remote at the start of the workflow, then our local repo won't be synchronized with the remote.

Recursive merge: Git opens the nano text-editor and asks us to add a message for the merge. We leave a message that we are pulling the latest report from the remote, then save and exit nano. We can avoid it, but it is not recommended.

When we have a local branch that doesn't exist yet remotly:

    git push origin hotfix


