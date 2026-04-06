# Correction using  `Git Reset`, `Revert`, `Restore`, `Amend`, and `Cherry-Pick`
This guide covers five highly practical Git topics:

- **reset**
- **revert**
- **restore**
- **amend**
- **cherry-pick**

These commands become essential once you already understand commits, branches, merges, rebases, and Pull Requests.

They help answer real-world questions such as:

- How do I undo something safely?
- How do I fix the last commit?
- How do I restore a file?
- How do I move one specific commit to another branch?
- When should I rewrite history, and when should I preserve it?

---

## Learning Goals

By the end of this section, you should be able to:

- explain the difference between **reset** and **revert**
- understand the purpose of **restore**
- amend the most recent commit safely
- move a specific commit with **cherry-pick**
- choose the correct tool depending on whether history should be rewritten or preserved
- avoid common mistakes when undoing or reusing work

---

## 1. Why These Commands Matter

In real Git usage, people constantly need to correct mistakes. For example:

- you committed too early
- you forgot to include one file in the last commit
- you want to undo a bad commit without deleting history
- you accidentally staged the wrong file
- you want to bring one bug fix from one branch into another
- you want to discard local edits and go back to the committed state

These are not unusual situations. They are part of normal Git work.

That is why `reset`, `revert`, `restore`, `amend`, and `cherry-pick` are core professional tools.

---

## 2. A High-Level Map

A useful mental model is:

- **reset** → move branch pointers and optionally unstage or discard changes
- **revert** → create a new commit that undoes an earlier commit
- **restore** → restore file contents or unstage changes
- **amend** → modify the most recent commit
- **cherry-pick** → apply one specific commit onto another branch

### Another Useful Distinction

Some commands primarily **rewrite local history**:

- `reset`
- `commit --amend`

Some commands primarily **preserve history while undoing effects**:

- `revert`

Some commands primarily **manipulate working tree or staging area state**:

- `restore`

Some commands **reuse existing work across branches**:

- `cherry-pick`

---

## 3. Working Tree, Staging Area, and Commit History

Before using undo commands well, keep this model in mind.

### Working Tree

The actual files in your project directory.

### Staging Area

The set of changes prepared for the next commit.

### Commit History

The already recorded snapshots in Git.

Many Git commands differ because they affect different layers.

For example:

- one command may only unstage files
- another may change the working tree
- another may move the branch pointer
- another may add a brand-new commit

That is why these commands can feel similar at first, even though they behave very differently.

---

## 4. `git reset`: Core Idea

`git reset` is one of the most powerful and potentially dangerous Git commands.

Its core purpose is to move **HEAD** and, depending on how you use it, also affect:

- the staging area
- the working tree

### Conceptual Meaning

At a high level, `reset` means:

> Move my current branch to a different commit, and possibly adjust staged or working changes to match.

Because of that, `reset` can be:

- very helpful
- very destructive if used carelessly

---

## 5. Common Forms of `git reset`

The three common modes are:

- `--soft`
- `--mixed`
- `--hard`

### `--soft`

Moves the branch pointer, but keeps changes staged.

### `--mixed`

Moves the branch pointer and unstages changes, but keeps file modifications in the working tree.

### `--hard`

Moves the branch pointer, unstages changes, and resets files in the working tree.

This is the most dangerous form because it can discard local work.

---

## 6. `git reset --soft`

Example:

```bash
git reset --soft HEAD~1
```

This means:

- move the current branch back by one commit
- keep the changes from that commit staged

### Use Case

You made a commit, but now want to:

- rewrite the message
- split the work differently
- recommit in a cleaner way

This is useful when the commit itself was premature, but the content is still correct.

---

## 7. `git reset --mixed`

This is the default if you do not specify a mode.

Example:

```bash
git reset HEAD~1
```

Or explicitly:

```bash
git reset --mixed HEAD~1
```

This means:

- move back one commit
- keep file changes in your working directory
- unstage them

### Use Case

You want to undo a commit, but then re-stage the files more selectively.

---

## 8. `git reset --hard`

Example:

```bash
git reset --hard HEAD~1
```

This means:

- move back one commit
- reset the staging area
- reset the working tree
- discard local changes that were part of that state transition

### Important Warning

`--hard` can permanently destroy local work that is not otherwise recoverable through reflog or other advanced recovery methods.

Use it only when you are sure.

---

## 9. When Is `reset` Appropriate?

`reset` is most appropriate when:

- you are cleaning up **local history**
- you have **not safely shared** the commits yet
- you want to rewrite or remove recent local commits
- you want to unstage files
- you want to discard local changes intentionally

### Practical Rule

`reset` is usually best for **local correction**.  
It is more dangerous when the commits were already pushed and other people may rely on them.

---

## 10. Why `reset` Can Be Dangerous on Shared Branches

If you use `reset` on commits that were already pushed to a shared branch:

- history changes
- commit IDs may disappear from the visible branch
- collaborators may still have the old history
- force-pushing may become necessary
- confusion and integration problems can follow

### Safer Rule

Use `reset` freely on your own local mistakes.  
Use it very carefully on anything already shared.

---

## 11. `git revert`: Core Idea

`git revert` is different from `reset`.

Instead of moving history backward, `revert` creates a **new commit** that undoes the effect of an earlier commit.

### Conceptual Meaning

At a high level:

> Keep the history, but add a new commit that reverses the change.

This makes `revert` much safer for shared history.

---

## 12. Example of `git revert`

```bash
git revert abc1234
```

This tells Git to:

- find commit `abc1234`
- compute the inverse of its effect
- create a new commit applying that inverse

### Result

The original commit remains in history.  
The new revert commit records that its effects were undone.

---

## 13. When Should You Prefer `revert`?

Prefer `revert` when:

- the bad commit was already pushed
- the branch is shared
- you want a clear audit trail
- you need to undo a change safely without rewriting history

### Typical Professional Rule

- **local mistake not yet shared** → often `reset`
- **shared bad commit** → often `revert`

---

## 14. `reset` vs `revert`

This distinction is fundamental.

### `reset`

- rewrites branch position
- can rewrite visible history
- best for local cleanup
- can be dangerous on shared branches

### `revert`

- does not remove the original commit
- adds a new undo commit
- safer for shared history
- leaves a clear record of what happened

### Mental Shortcut

- **reset** = “pretend the branch pointer moved back”
- **revert** = “add a new commit that cancels an old one”

---

## 15. `git restore`: Core Idea

`git restore` was introduced to make certain file-level operations clearer.

It is mainly used to:

- restore file content in the working tree
- unstage files from the index

It helps separate these actions from the older, overloaded `checkout` behavior.

---

## 16. Restore a File in the Working Tree

Example:

```bash
git restore app.py
```

This means:

- discard local modifications in `app.py`
- restore it from the current `HEAD` state

### Use Case

You edited a file locally and want to abandon those uncommitted edits.

---

## 17. Unstage a File with `restore`

Example:

```bash
git restore --staged app.py
```

This means:

- remove `app.py` from the staging area
- keep the file changes in the working tree

### Use Case

You accidentally staged a file and want to unstage it without losing the edit.

---

## 18. Restore from Another Source

You can also restore a file from a particular commit.

Example:

```bash
git restore --source=abc1234 README.md
```

This means:

- take `README.md` as it existed in commit `abc1234`
- restore it into the working tree

This is useful when you want one file from an earlier point without changing the whole branch history.

---

## 19. Why `restore` Is Useful

`restore` is useful because it lets you think in terms of **file state**, not only commit history.

It is especially helpful for:

- undoing accidental local edits
- unstaging files cleanly
- restoring a single file from a chosen commit

It is often safer and clearer than reaching for `reset` when your goal is only file-level correction.

---

## 20. `git commit --amend`: Core Idea

Amend changes the **most recent commit**.

It is commonly used when:

- the last commit message is wrong
- you forgot to include one file
- you want to slightly adjust the most recent commit

### Example

```bash
git commit --amend
```

Git opens the commit message editor and lets you replace the last commit with a new version.

---

## 21. Amend the Last Commit Message

Example:

```bash
git commit --amend -m "Fix login validation for empty input"
```

This replaces the latest commit message with a new one.

### Important Note

Even changing only the message rewrites the commit.  
That means the commit hash changes.

---

## 22. Amend to Add a Forgotten File

Suppose you made a commit but forgot one file.

Workflow:

```bash
git add missing_file.py
git commit --amend
```

Now Git rebuilds the latest commit to include that file.

This is a very common and useful correction pattern.

---

## 23. When Is Amend Safe?

Amend is safest when:

- the commit is still local
- you have not pushed it yet
- nobody else is depending on that exact commit hash

### Warning

If you amend a commit that was already pushed:

- the commit hash changes
- force-push may be needed
- collaborators can be confused if they already used the old commit

---

## 24. `git cherry-pick`: Core Idea

`git cherry-pick` applies the effect of one specific commit onto your current branch.

### Conceptual Meaning

At a high level:

> Take that commit over there, and replay its effect here.

This is very useful when you do **not** want to merge a whole branch, but only want one particular change.

---

## 25. Example of `cherry-pick`

Suppose commit `abc1234` on another branch contains an important bug fix.

You are on your current branch and run:

```bash
git cherry-pick abc1234
```

Git then attempts to apply that commit here as a new commit.

### Result

You get the effect of the original commit, but the new branch history remains separate.

---

## 26. When Is `cherry-pick` Useful?

Cherry-pick is useful when:

- one branch contains a bug fix you need elsewhere
- you want a single commit, not the whole branch history
- you are backporting a fix to a release branch
- you want to reuse a precise change selectively

### Common Professional Scenario

A bug is fixed on `main`, and the same fix is needed on a maintenance branch:

```bash
git switch release/1.2
git cherry-pick abc1234
```

---

## 27. Cherry-Pick Can Also Conflict

Cherry-pick is not magic.  
If the target branch differs too much, the commit may not apply cleanly.

Then Git may stop with conflicts, and you resolve them similarly to merge or rebase conflicts:

```bash
git status
git add .
git cherry-pick --continue
```

Or, if needed:

```bash
git cherry-pick --abort
```

---

## 28. Choosing the Right Tool

A professional Git user learns to choose based on intent.

### Use `reset` when:

- you want to rewrite recent local history
- you want to undo local commits before sharing
- you want to unstage or discard local state

### Use `revert` when:

- a bad commit is already shared
- you want a safe undo with history preserved

### Use `restore` when:

- you want to restore one file
- you want to unstage a file
- you want file-level correction without branch history surgery

### Use `amend` when:

- you only need to fix the last commit

### Use `cherry-pick` when:

- you want one specific commit on another branch

---

## 29. Common Mistakes

### 1. Using `reset --hard` too casually

This can destroy local work.

### 2. Using `reset` instead of `revert` on shared branches

This can make collaboration messy.

### 3. Forgetting that `amend` rewrites history

Even a message-only change creates a new commit hash.

### 4. Cherry-picking large sequences without thinking

This can create duplicated or confusing history if done carelessly.

### 5. Using history-rewriting commands after public sharing without coordination

This is one of the most common professional Git mistakes.

---

## 30. Practical Scenario 1: Undo the Last Local Commit, Keep the Changes

You committed too early, but want to keep working.

A good option is:

```bash
git reset --mixed HEAD~1
```

Now:

- the commit is removed from visible branch history
- the file changes remain in your working tree
- nothing is staged

You can now stage files more selectively and recommit properly.

---

## 31. Practical Scenario 2: Undo a Shared Commit Safely

A bad commit is already on `main` and must be undone.

A safer option is:

```bash
git revert abc1234
```

This creates a new commit that reverses the earlier one.

This is the professional-safe pattern for shared history.

---

## 32. Practical Scenario 3: Remove a File from Staging

You accidentally staged `config.local.json`, but you do not want it in the next commit.

Use:

```bash
git restore --staged config.local.json
```

Now:

- the file is unstaged
- your local edits remain

---

## 33. Practical Scenario 4: Fix the Last Commit

You committed, then realized the message is weak or one file is missing.

A common correction is:

```bash
git add forgotten_file.py
git commit --amend -m "Add validation and tests for empty email input"
```

This replaces the previous final commit with a better version.

---

## 34. Practical Scenario 5: Backport a Fix to Another Branch

Suppose `main` has a bug fix, but you also need it on `release/1.2`.

Workflow:

```bash
git switch release/1.2
git cherry-pick abc1234
```

This applies only that selected fix to the release branch, without merging unrelated work from `main`.

---

## 35. A Useful Safety Principle

Before running an undo-related command, ask yourself:

### Question 1

Do I want to **rewrite history**, or do I want to **preserve history**?

- rewrite history → maybe `reset` or `amend`
- preserve history → maybe `revert`

### Question 2

Am I changing a **file state**, or am I changing **commit history**?

- file state → maybe `restore`
- commit history → maybe `reset`, `revert`, `amend`, or `cherry-pick`

### Question 3

Has this already been shared with others?

- not shared → more freedom
- shared → prefer safer history-preserving choices

---

## 36. Quick Comparison Table

### `reset`

- moves branch pointer
- can affect staging and working tree
- good for local cleanup

### `revert`

- adds a new inverse commit
- good for undoing shared commits safely

### `restore`

- restores files or unstages them
- good for file-level corrections

### `amend`

- replaces the most recent commit
- good for fixing the latest commit

### `cherry-pick`

- applies one specific commit onto the current branch
- good for selective reuse across branches

---

## 37. Quick Reference Commands

### Undo last local commit, keep changes staged

```bash
git reset --soft HEAD~1
```

### Undo last local commit, keep changes unstaged

```bash
git reset --mixed HEAD~1
```

### Undo last local commit and discard changes

```bash
git reset --hard HEAD~1
```

### Safely undo a shared commit

```bash
git revert <commit>
```

### Discard local changes in one file

```bash
git restore path/to/file
```

### Unstage one file

```bash
git restore --staged path/to/file
```

### Fix the latest commit

```bash
git commit --amend
```

### Move one commit to another branch

```bash
git cherry-pick <commit>
```

---

## 38. Final Summary

These five commands solve different classes of Git problems.

### `reset`

Use it for local history cleanup and pointer movement.

### `revert`

Use it for safe undo in shared history.

### `restore`

Use it for file-level restoration or unstaging.

### `amend`

Use it to improve or correct the last commit.

### `cherry-pick`

Use it to apply one specific commit somewhere else.

The deeper lesson is this:

> Good Git usage is not just knowing commands. It is knowing which layer you intend to change: file state, staging state, branch history, or shared history.

---

## 39. Suggested Practice Exercises

1. make two commits in a test repository
2. use `git reset --soft HEAD~1` and observe the staged state
3. repeat with `git reset --mixed HEAD~1`
4. create a new commit and undo it with `git revert`
5. modify a file and discard changes using `git restore`
6. stage a file and unstage it using `git restore --staged`
7. make a commit with a bad message and fix it using `git commit --amend`
8. create two branches and use `git cherry-pick` to move one fix from one branch to another
9. compare histories using:

```bash
git log --oneline --graph --all
```

These exercises build intuition much faster than memorizing definitions.

---

# GitHub Notes

## Forking a Repository

- We should **fork** a repository of interest.
- Forking allows us to:
  - create our own copy of the repository
  - modify its content and code independently
  - experiment without affecting the original project

---

## Using Git Locally vs GitHub

- Git can be used **locally** on your system:
  - useful when working with **sensitive datasets**
  - no need to upload data online
- When using GitHub:
  - use a `.gitignore` file to exclude sensitive or unnecessary files
  - this helps keep the repository clean and secure

---

## Pull Command

The **pull** command means:

- bringing changes from a **remote repository** into your local branch
- or updating your branch with changes from another branch

```bash
git pull
```

---

# Git Supplement: Finding the Right Commit and Debugging Regressions

This section is a **supplement** to the main Git section. It focuses on practical points that are not explicitly covered inside the reset/revert/restore/amend/cherry-pick section itself:

1. how to find the commit ID (hash) you need
2. how to refer to commits without memorizing full hashes
3. how to identify the exact commit where code stopped working
4. how to use `git bisect` to find the first bad commit efficiently

Use this supplement as a hands-on companion after reading the main Git guide.

## 1. How to Get the Commit Number (Commit Hash)

Many Git commands need a target commit, for example:

- `git revert <commit>`
- `git reset --hard <commit>`
- `git cherry-pick <commit>`

In Git, the “commit number” is usually called the **commit hash** or **commit ID**.

### Fast Way to List Commits

```bash
git log --oneline
```

Example output:

```text
a1b2c3d add login validation
f6g7h8i fix navbar layout
91ab234 initial API integration
```

Here:

- `a1b2c3d`
- `f6g7h8i`
- `91ab234`

are short commit hashes.

You can usually use the short form as long as it is unique in your repository.

## 2. Which Log View Should You Use?

### Basic history

```bash
git log
```

Shows:

- full commit hash
- author
- date
- message

### Short history

```bash
git log --oneline
```

Best when you just want the commit IDs quickly.

### Visual branch history

```bash
git log --oneline --graph --all
```

Very useful when branches or merges are involved.

Example mental picture:

```text
* 8f4c2ab fix test on release branch
| * c7d9120 add login retry
| * 13bc991 refactor login flow
|/
* 91ab234 initial API integration
```

This helps you answer:

- Which commit is newest?
- Which branch introduced the change?
- Which commit should I revert or cherry-pick?

## 3. You Do Not Always Need the Full Commit Hash

Instead of typing a full hash, Git also lets you refer to recent commits relative to `HEAD`.

`HEAD` means: **the commit I am currently on**.

### Common Relative References

- `HEAD` → current commit
- `HEAD~1` → one commit before current
- `HEAD~2` → two commits before current
- `HEAD~3` → three commits before current

Examples:

```bash
git reset --soft HEAD~1
git reset --hard HEAD~2
git show HEAD~3
```

This is useful when the bad change is recent and you just want to move back a few commits.

## 4. When Code Worked Before but Is Broken Now

This is a classic debugging situation:

- an older commit worked
- the current commit is broken
- you want to know **exactly which commit introduced the problem**

There are two approaches.

### A. Manual checking

You move between commits and test the code yourself.

Example:

```bash
git checkout <commit>
```

Then run your program or tests.

If that commit is good, move forward.  
If it is bad, move backward.

This works, but it can be slow in repositories with many commits.

### B. `git bisect` (recommended)

This is the professional way when you know:

- one commit that is definitely **good**
- one commit that is definitely **bad**

Git then performs a binary search through history to find the first bad commit much faster.

## 5. `git bisect` Step by Step

### Step 1: Start bisect

```bash
git bisect start
```

### Step 2: Mark the current state as bad

```bash
git bisect bad
```

This usually means: “the version I am on right now is broken.”

### Step 3: Mark an older known-good commit

```bash
git bisect good <commit-id>
```

Now Git chooses a commit in the middle.

### Step 4: Test that commit

Run your program or your tests.

- If it works:

  ```bash
  git bisect good
  ```

- If it fails:

  ```bash
  git bisect bad
  ```

Git keeps narrowing the search until it prints something like:

```text
<commit-id> is the first bad commit
```

## 6. Real Example of `git bisect`

Suppose your history is:

```text
A - B - C - D - E - F
```

You know:

- `B` was still good
- `F` is bad

You run:

```bash
git bisect start
git bisect bad
git bisect good B
```

Git may test `D` first.

- If `D` is good, then the bug must be in `E` or `F`
- If `D` is bad, then the bug must be in `C` or `D`

So instead of checking every commit one by one, Git cuts the search space in half each time.

## 7. If You Already Have Automated Tests, Bisect Becomes Much Stronger

If your project has a command that returns success or failure, Git can automate the search.

Examples:

```bash
git bisect run pytest
git bisect run npm test
git bisect run python -m unittest
```

In this mode:

- test passes → Git treats commit as good
- test fails → Git treats commit as bad

This is one of the best tools for tracking down regressions.

## 8. How This Connects to Reset, Revert, Restore, Amend, and Cherry-Pick

After you find the problematic commit, you can decide what to do.

### If the bad commit was already shared

Use:

```bash
git revert <bad-commit>
```

### If the bad commit is only local and you want to rewrite history

Use one of:

```bash
git reset --soft <target>
git reset --mixed <target>
git reset --hard <target>
```

### If the problem is just one file

Use:

```bash
git restore path/to/file
```

### If the issue is only in the latest commit message or contents

Use:

```bash
git commit --amend
```

### If you want to bring a good fix from another branch

Use:

```bash
git cherry-pick <good-commit>
```

So the missing skill is often not the command itself, but **finding the correct commit first**.

## 9. Practical Workflow You Can Follow in Real Projects

When you suspect a regression:

1. Inspect history

   ```bash
   git log --oneline --graph --all
   ```

2. Identify:
   - one known good commit
   - one known bad commit

3. Use `git bisect`

   ```bash
   git bisect start
   git bisect bad
   git bisect good <known-good-commit>
   ```

4. Test each checked-out revision

5. Finish with:

   ```bash
   git bisect reset
   ```

6. Then choose the appropriate fix:
   - `revert`
   - `reset`
   - `restore`
   - `amend`
   - `cherry-pick`

## 10. Quick Command Summary

### Find commit IDs

```bash
git log --oneline
git log --oneline --graph --all
```

### Refer to recent commits

```bash
HEAD
HEAD~1
HEAD~2
HEAD~3
```

### Move to a specific commit temporarily

```bash
git checkout <commit-id>
```

### Find the first bad commit

```bash
git bisect start
git bisect bad
git bisect good <commit-id>
git bisect good
git bisect bad
git bisect reset
```

### Automated bisect

```bash
git bisect run pytest
```

## 11. Final Takeaway

The main section explains **what** `reset`, `revert`, `restore`, `amend`, and `cherry-pick` do.

This supplement adds the missing practical layer we discussed:

- how to **find the commit hash**
- how to **target recent commits using `HEAD~n`**
- how to **detect where the code started breaking**
- how to **use `git bisect` to find the first bad commit efficiently**

That combination is what makes these Git commands truly usable in real debugging work.

---

# Git Workflow: Connect a Local Folder to a GitHub Repository

## Goal

We will:

- create a repository on GitHub
- create a local folder
- initialize Git
- connect to GitHub
- push code

## Step 1: Create a Local Folder

```bash
mkdir my-project
cd my-project
```

## Step 2: Initialize Git

```bash
git init
```

## Step 3: Add Files

```bash
touch README.md
```

## Step 4: Stage Files

```bash
git add .
```

## Step 5: Commit

```bash
git commit -m "Initial commit"
```

## Step 6: Connect to GitHub

```bash
git remote add origin https://github.com/username/my-project.git
```

## Step 7: Check the Remote

```bash
git remote -v
```

The result should be in the form of:

```text
origin git@github.com:username/myproject.git (fetch)
origin git@github.com:username/myproject.git (push)
```

## Step 8: Push

```bash
git branch -M main
git push -u origin main
```

---

# Git Archaeology Example

This example explains a practical **Git archaeology** workflow using the `networkx` repository.

## Goal

We want to:

1. switch to a specific historical release branch or context
2. search for a specific text in the repository
3. find the file and line where it appears
4. identify the commit related to that line
5. inspect the commit
6. create a branch from that historical commit

This is useful when you want to understand:

- where a bug message came from
- which commit introduced or changed something
- how the code looked at that point in time

## Step 1: Create a Working Branch from `networkx-2.6.3`

We start by creating a new branch called `exercise` from `networkx-2.6.3`.

This gives us a safe branch to work on without changing the original branch.

```bash
git switch --create exercise networkx-2.6.3
```

## Step 2: Search for the Text in the Repository

Now we search for the phrase:

`Logic error in degree_correlation`

This tells us which file or files contain that text.

```bash
git grep "Logic error in degree_correlation"
git grep -n "Logic error in degree_correlation"
```

## Step 3: Annotate the File

After identifying the file, we run `git annotate` on it:

`networkx/algorithms/threshold.py`

This shows, line by line:

- the commit hash
- the author
- the date
- the content of the line

This helps us discover **which commit last changed the line** containing the text.

```bash
git annotate networkx/algorithms/threshold.py
```

## Step 4: Search Inside the Annotate Output

Inside the `git annotate` viewer, search for the phrase:

`Logic error`

You typically type:

```text
/Logic error
```

This is not a shell command. It is a search command used inside the pager (`less`) opened by Git.

## Step 5: Inspect the Relevant Commit

From the annotation output, we find the commit hash:

`90544b4fa`

Now we inspect that commit with `git show`.

This displays:

- commit metadata
- author and date
- commit message
- the diff (what changed)

```bash
git show 90544b4fa
```

## Step 6: Create a Branch from That Historical Commit

Now we create a new branch called `past-code` from commit `90544b4fa`.

This lets us explore the code exactly as it existed at that point in history.

```bash
git branch past-code 90544b4fa
```

## What This Example Demonstrates

This example solves the following investigation workflow:

- find a text in the repository
- locate the exact file and line
- identify the commit responsible for that line
- inspect the commit
- create a branch from that point in history

In short:

```bash
git grep -> find the text
git annotate -> find the responsible commit
git show -> inspect the commit
git branch -> create a branch from that commit
```

This is a classic Git archaeology workflow.

## Command Summary

```bash
git switch --create exercise networkx-2.6.3
git grep "Logic error in degree_correlation"
git grep -n "Logic error in degree_correlation"
git annotate networkx/algorithms/threshold.py
/Logic error
git show 90544b4fa
git branch past-code 90544b4fa
```
