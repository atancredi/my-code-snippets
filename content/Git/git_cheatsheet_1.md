	 # ssh-agent
https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

# Pull requests
When thinking about branches, remember that the _base branch_ is **where** changes should be applied, the _head branch_ contains **what** you would like to be applied.

## Remove deleted remote branches
```
git fetch --prune
```

# Revert Commit
# <hr>

# The line ending problem
how to avoid problems in the git diff, configuring git to properly handle line endings.

basic concept: NEVER USE AUTOCRLF. It brings to a dead end in a lot of cases and it's widely considered broken.
Instead use the .gitattributes file:
- when committed to a repository it overrides the core.autocrlf setting for all contributors: this ensures consistent behavior regardless of the individual git settings and env.
- it must be created in the root of the repository
- it looks like a table with two columns: in the first is the file name, in the second is the line ending configuration that git should use for the specified file.

Example
```
# Set the default behavior, in case people don't have core.autocrlf set.
* text=auto

# Explicitly declare text files you want to always be normalized and converted
# to native line endings on checkout.
*.c text
*.h text

# Declare files that will always have CRLF line endings on checkout.
*.sln text eol=crlf

# Denote all files that are truly binary and should not be modified.
*.png binary
*.jpg binary
```

- text=auto Git will handle the files in whatever way it thinks is best. This is a good default option.

- text eol=crlf Git will always convert line endings to CRLF on checkout. You should use this for files that must keep CRLF endings, even on OSX or Linux.

- text eol=lf Git will always convert line endings to LF on checkout. You should use this for files that must keep LF endings, even on Windows.

- binary Git will understand that the files specified are not text, and it should not try to change them. The binary setting is also an alias for -text -diff.

## Refreshing a repository after changing line endings

When you set the core.autocrlf option or commit a .gitattributes file, you may find that Git reports changes to files that you have not modified. Git has changed line endings to match your new configuration.

To ensure that all the line endings in your repository match your new configuration, backup your files with Git, delete all files in your repository (except the .git directory), then restore the files all at once.

1. Save your current files in Git, so that none of your work is lost.
$ git add . -u
$ git commit -m "Saving files before refreshing line endings"

2. Add all your changed files back and normalize the line endings.
$ git add --renormalize .

3. Show the rewritten, normalized files.
$ git status

4. Commit the changes to your repository.
$ git commit -m "Normalize all the line endings"


<hr>

# <hr>

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




<hr>
# Various
## Delete git commit history
Deleting the `.git` folder may cause problems in your git repository. If you want to delete all your commit history but keep the code in its current state, it is very safe to do it as in the following:
1.  Checkout
    `git checkout --orphan latest_branch`
2.  Add all the files
    `git add -A`
3.  Commit the changes
    `git commit -am "commit message"`
4.  Delete the branch
    `git branch -D main`
5.  Rename the current branch to main
    `git branch -m main`
6.  Finally, force update your repository
    `git push -f origin main`
PS: this will not keep your old commit history around

## Remove and add file to trigger .gitignore changes
[Source](https://stackoverflow.com/posts/25436481/timeline)
The files/folder in your version control will not just delete themselves just because you added them to the `.gitignore`. They are already in the repository and you have to remove them. You can just do that with this:
**Remember to commit everything you've changed before you do this!**
```
git rm -rf --cached .
git add .
```
This removes all files from the repository and adds them back (this time respecting the rules in your `.gitignore`).

## Change line termination in repository
```
git config core.autocrlf false

# Don’t forget the dot at the end
git rm --cached -r . 

git reset --hard
```
Make sure to commit any changes before running this!

## Exclude from changes a conf file already versioned on the remote
```
git update-index --skip-worktree .env
```
This is useful because it leaves the local file unchanged and the remote file stays on the repo but it's not updated anymore.