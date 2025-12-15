# SETUP  
##### Configuring user information used across all local repositories  
 - `git config --global user.name “[firstname lastname]” ` 
	set a name that is identifiable for credit when review version history  
 - `git config --global user.email “[valid-email]”  ` 
	set an email address that will be associated with each history marker  
- `git config --global color.ui auto  ` 
	set automatic command line coloring for Git for easy reviewing

# SETUP & INIT  
##### Configuring user information, initializing and cloning repositories  
 - `git init  `
	initialize an existing directory as a Git repository  
 - `git clone [url]  `
	retrieve an entire repository from a hosted location via URL

# STAGE & SNAPSHOT  
##### Working with snapshots and the Git staging area  
- `git status  `
	show modified files in working directory, staged for your next commit  
 - `git add [file]  `
	add a file as it looks now to your next commit (stage)  
- `git reset [file]  `
	unstage a file while retaining the changes in working directory  
- `git diff  `
	diff of what is changed but not staged  
 - `git diff --staged  `
	diff of what is staged but not yet committed  
 - `git commit -m “[descriptive message]”  `
	commit your staged content as a new commit snapshot

### How to commit changes in the current branch
`git status`  
`git add .`
`git commit -m "My sweet new feature"`
`git push`
### How to commit changes in another branch
`git status`  
`git add .`
`git commit -m "My sweet new feature"`
`git push origin my-feature-branch`

# BRANCH & MERGE  
##### Isolating work in branches, changing context, and integrating changes  
 - `git branch  `
	list your branches. a * will appear next to the currently active branch  
 - `git branch [branch-name]  `
	create a new branch at the current commit  
 - `git checkout  `
	switch to another branch and check it out into your working directory  
 - `git merge [branch]  `
	merge the specified branch’s history into the current one  
 - `git log  `
	show all commits in the current branch’s history

### How to create and push a new branch
`git checkout -b new_branch`
`git push -u origin new_branch`

# INSPECT & COMPARE  
##### Examining logs, diffs and object information  
- `git log  `
	show the commit history for the currently active branch  
 - `git log branchB..branchA  `
	show the commits on branchA that are not on branchB  
 - `git log --follow [file]  `
	show the commits that changed file, even across renames  
 - `git diff branchB...branchA  `
	show the diff of what is in branchA that is not in branchB  
 - `git show [SHA]  `
	show any object in Git in human-readable format  

# IGNORING PATTERNS  
##### Preventing unintentional staging or commiting of files  
 - `git config --global core.excludesfile [file]  `
	system wide ignore pattern for all local repositories

# SHARE & UPDATE  
##### Retrieving updates from another repository and updating local repos  
 - `git remote add [alias] [url]  `
	add a git URL as an alias  
 - `git fetch [alias]  `
	fetch down all the branches from that Git remote  
 - `git merge [alias]/[branch]  `
	merge a remote branch into your current branch to bring it up to date  
 - `git push [alias] [branch]  `
	Transmit local branch commits to the remote repository branch  
 - `git pull  `
	fetch and merge any commits from the tracking remote branch


# REWRITE HISTORY  
##### Rewriting branches, updating commits and clearing history  
 - `git rebase [branch]  `
	apply any commits of current branch ahead of specified one  
 - `git reset --hard [commit]  `
	clear staging area, rewrite working tree from specified commit

---
# I messed up. How do I fix, revert or undo this? (git reset, restore)

**I want to undo my commit, but I don’t want to lose those changes**

- `git reset --soft HEAD~1` resets 1 commit back to staged

**I want to remove a file from the staged state**

- `git restore --staged PATH_TO_FILE`

**I committed a file to my remote feature branch, now I want to remove it**

git reset --soft HEAD~1  
git restore --staged PATH_TO_FILE_TO_REMOVE  
git commit  
git push --force-with-lease # To your feature branch

**I want to undo my commit, and I want to remove those changes**

- `git reset --hard HEAD~1` resets 1 commit completely
- To remove the commit from the remote, follow with `git push --force`. Do this with caution and follow the same rebase rules above when doing so.

**Help! I used** `**reset --hard**` **and I meant to use** `**reset --soft**`

- `git reset HEAD@{1}` to undo your latest git operation

How does this work? Run `git log -g` or `git reflog`. Both commands show a reference log which include entries such as reset. For example, if the very last command you ran was `git reset --hard origin/main` you may see: _Reflog: HEAD@{1} Reflog message: reset: moving to origin/main_.

Reflog is your go to any time you make mistake but don’t have a specific commit to fall back to.

**I want to remove all changes and “force pull” from the remote**

- `git fetch` > `git reset --hard origin/your-branch-name`

**I have multiple commits on my remote feature branch, I want to combine them into 1 commit**

- First, follow the rebase rules. i.e. make sure you can safely force push without overwriting someone elses work.
- `git log` count the number of commits you want to squash (using 4 for the example)
- `git reset --soft HEAD~4`
- `git commit -m "your new message"`
- `git push --force`
