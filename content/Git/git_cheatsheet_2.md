These are my personal notes on git. Each section starts with a problem or goal, and is further broken down into specific scenarios. This way, the notes are structured _“Just in Time”_ (like asking a friend for help). Instead of _“Just in Case”_ (like reading the manual start-to-finish before driving the car).

**_How to use these notes_**

You can get started by reading through the **Table of Contents** or checkout some of the **rapid fire tips**. Alternatively, **bookmark** these notes and return when you have a specific goal or problem to solve.

## Table of Contents

- Rapid fire tips
- “How do I safely rebase (never fear rebase again)”
- “I messed up. How do I fix, revert or undo this?”
- “How do I use git effectively for me (and my team)”
- Worktrees `🚧 Work in Progress`

# Rapid Fire Tips

- `git pull` translates to `git fetch` + `git merge` (or `git rebase` given -r)
- `git pull -r origin main` to rebase the latest change from main
- `git fetch` is still useful on its own. Run before checking out a teammates remote branch.
- `git fetch` > `git reset --hard origin/main` to “force pull”
- Whoops. `git reset HEAD@{1}` (you can undo a bad reset with… reset)
- `git log -g` instead of `git reflog`. Personal preference.
- `git checkout -b temporary-branch` before commit / push if you’re worried about breaking something
- `git reset --soft` puts your commits back to the staged state. Now you can commit multiple previous commits as one well-written commit. _See examples_
- `git push` will automatically point to `git push ORIGIN FEATURE-BRANCH` given this config `git config --global push.autoSetupRemote true`
- `git push --force-with-lease` force pushes but prevents you from overwriting others work
- You should try Conventional commits. They make your commits easy to read and can automate tags/releases/versioning
- Oh wow. What coincidence. Totally unexpected. I wrote a CLI that helps you write conventional commits — [Better Commits Github](https://github.com/everduin94/better-commits)

# How do I safely rebase (never fear rebase again)

A few assumptions

- You’re working on a feature branch
- `origin` is the name of your remote
- `main` is the name of your default branch

Is it safe to rebase (**strict**) — **_Is your branch only local? (not public)_**

- Yes — Safe to rebase ✅
- No — Not safe to rebase ❌

Is it safe to rebase (**simple**) — **_Are you the only person working on your branch?_**

- Yes — Safe to rebase ✅
- No — Not safe to rebase ❌

If you work on a team where developers checkout their own feature branch, and _only they work on it_. Go for the simple version. If developers commonly _work on each other branches_ or share long lived branches. Go for the strict version.

How can you guarantee it’s safe to rebase 100% of the time?

`git checkout -b new-branch-name`

Because now your branch only exists locally... until you push of course.

**Rebase Checklist**

> 💡 Checkout the example below if you’re feeling overwhelmed by all of the steps

1. `git branch` > `git log` verify feature branch and existing commits
2. `git add` > `git commit` > `git push origin feature-branch` if you have changes, commit
3. `git pull -r origin main` to pull
4. **If you have conflicts**, fix > `git add` > `git rebase --continue`
5. `git log` verify your work
6. `git push --force` update your feature branch

**Rebase Checklist Explanation & Tips**

## 1

Make sure you’re on a feature/developer branch. Doing this directly on a main/release branch is a big no-no.

If you like to be thorough (I’m a fan) `git log` so you know what the log looked like before your rebase.

Concerned about making a mistake? `git checkout -b temporary-branch`. Now any mistakes will be isolated to this private temporary-branch. Skip this step if you're feeling confident (Don't worry, I believe in you).

## 2

If you have changes then stage, commit and push. `git add`, `git commit`, `git push`. Very important. If you don't make your own commit before rebase you may accidentally merge the work of another developer into your work and restarting from a mistake becomes more difficult.

**Multiple Commits & Rebasing (Situational)**

Given multiple commits (and possible merge conflicts). Squashing your commits first in this scenario saves you a lot of time because merge conflicts are compared against _each commit_.

## 3

`git pull -r origin main`. Pull -r means `git fetch` + `git rebase`, this guarantees you pull the latest commit.

## 4

If you have merge conflicts you’ll likely use your editor to resolve them. After resolving, remember these commands. `git add` will stage your resolutions, `git rebase --continue` will move to the next step. If you make a mistake, you can restart via `git rebase --abort`

## 5

Double check your work. At this point your branch should resemble main + your commits replayed on top.

## 6

`git push --force`. Don't fear the force flag. In fact, you need to use force here.

**Tip #1 —** Alternatively, you can use `force-with-lease` instead of `force` as a safer version of force. This prevents you from accidentally overwriting someone else’s work. If you followed the steps, that shouldn’t be an issue, but it doesn’t hurt to use.

**Tip #2 —** `git config — global push.autoSetupRemote true` this allows you to type `git push` instead of `git push origin name-of-branch-you-have-checked-out`

## Example

git log # Double check existing commits  
  
# Commit changes (if any)  
git status   
git add .  
git commit -m "My sweet new feature"  
git push origin my-feature-branch  
# End changes  
  
git pull -r origin main  
  
# Fix Conflicts (if any)  
git add .  
git rebase --continue  
# End Conflicts  
  
git log # Verify we did everything correctly  
git push origin my-feature-branch --force

# I messed up. How do I fix, revert or undo this?

git reset, clean, restore, cherry-pick

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

**Git Clean Guide (Removing unwanted files)**

Git Clean Dry Run

- To dry run a git clean, add the `-n` flag

I want to remove untracked files

- `git clean -f`

I want to remove untracked folders/directories

- `git clean -fd`

I added something to my _.gitignore_ that I previously committed, now I want to remove it.

- `` git rm --cached `git ls-files -i -c --exclude-from=.gitignore` ``

I want to remove changes in my working directory

- `git restore FILE_NAME`

I want to remove ignored files

- `git clean -fX`

Notice the capital X, you probably want capital. Lowercase x will remove files like `.environment`.

**I was working off of another developers branch, they merged to main. Now I have conflicts with their merge commit when I try to merge to main.**

There is very likely a more efficient method to do this, but as of today, here’s how I handle it.

git log # Copy each commit hash you've made  
# abcd1235   
# efgh6789  
# lmno1234 --> Commit you started from  
  
git checkout main  
git pull -r origin main # Now you should have their changes  
git checkout -b your-new-feature-branch  
  
# Apply from oldest to most recent  
git cherry-pick efgh6789  
git cherry-pick abcd1235  
  
# Push  
git push origin your-new-feature-branch

💡 You can simplify this process by having 1 commit. If you have multiple commits, see previous section “**I want to combine them into 1 commit”**

# How do I use git effectively for me (and my team)

Branch names, conventional commits, tools

**Branch naming conventions**

Any variation of this can help define intention. It’s also useful for automating actions, tags, versions and releases

- `username/type/TICKET-description`
- `username/type/description`
- `type/description`

Branch Examples

Examples using issue, ticket, or neither

- `everduin94/fix/44-infer-underscore`
- `everduin94/feat/FLT-10-public-notebooks`
- `everduin94/feat/custom-scopes`

**Commit message conventions**

You can find additional examples and explanations here: [Conventional Commits Summary](https://www.conventionalcommits.org/en/v1.0.0/)

Below is a conventional commit from `better-commits`

feat(tools): #3 infer ticket/issue from branch  
  
If commit_type.infer_value_from_branch is enabled,  
will attempt to infer ticket/issue from branch name.  
Updated documentation with details.  
  
Closes: #3

My workflow — **New branch checklist**

1. checkout default `git checkout main`
2. update main `git pull -r origin main`
3. install dependencies (example) `npm ci`

[Better Commits](https://github.com/everduin94/better-commits) is a CLI to achieve all of the above. It’s well documented, flexible, easy to install and I use it every day. It can help you (and your team) create a consistent branch / commit workflow and minimize mistakes across users without enforcing PR lint rules or pre-commit hooks.

[Check it out on Github](https://github.com/everduin94/better-commits) — better-commits uses… better-commits. In fact, you can review my exact config, branches, commits and releases to better understand how it helps me work.