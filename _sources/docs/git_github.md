# Git and GitHub

This chapter introduces Git and GitHub, two powerful tools that allow developers to track the full history of a project and efficiently manage version control, including the ability to revert to previous versions.

---

## Table of Contents
- [What is Git?](#git)
- [What is GitHub](#github)

---

# What is Git?
<a name="git"></a>
**Git** is a version control system. It helps you:

-   track changes in your files
-   save project history
-   collaborate with others
-   restore older versions when needed
-   make new branches of your project and work there without affecting
    the main files of project.

In this section we learn about  **history and snapshots**, then later commands such as `log`, `branch`, `merge`, `rebase`, and `revert`.These are fundamental commands in daily usage of git. 
Git runs on your own computer. GitHub is an online platform that hosts Git repositories. In order to turn a floder into a Git repository, we do
as follows:

##  Configure Git; `git config`

Git uses configuration settings such as your name, email, editor, pager behavior, and aliases. The following commands uses to setup the configuration.

:::{note}

- **Set your identity**

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
These values appear in your commits.

-  **View all configuration**

``` bash
git config --list
```

-  **View one specific value**

``` bash
git config user.name
```

- **Set the default editor**

``` bash
git config --global core.editor "code --wait"
```

- **Enable color output**

``` bash
git config --global color.ui auto
```

- **Create aliases**

Git aliases let you define shorter names for longer commands.

``` bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.st status
git config --global alias.lg "log --oneline --graph --all"

```
Then you can use:

```bash
git co main
git br
git cm -m "Fix typo"
git st
git lg
```

- **Git Configuration Levels** 

Git supports three configuration levels:

-   `--system` → applies to the whole system
-   `--global` → applies to the current user
-   `--local` → applies only to the current repository

- **Priority**

If the same setting exists in multiple places, the order is:

``` text
local > global > system
```

This means local settings override global settings, and global settings override system settings.


- **Show Configuration Sources** 

To see both the configuration values and where they come from, use:

``` bash
git config --list --show-origin
```
Example output

``` text
file:/etc/gitconfig        core.editor=vim
file:/home/user/.gitconfig user.name=Ali
file:.git/config           core.repositoryformatversion=0
```
-  **Disabling Git Pagers**

Git often uses a pager such as `less` to display long output. For example, `git log` may open inside a scrollable interface.

You can disable the pager for a specific command:

``` bash
git config --global pager.log off
```

This makes `git log` print directly in the terminal.

- **Other examples**

``` bash
git config --global pager.diff off
git config --global pager.show off
git config --global pager.config off
git config --global pager.stash off
git config --global pager.help off
git config --global pager.blame off
git config --global pager.branch off
git config --global pager.annotate off
```

- **A Better Pager Setup**

Instead of disabling all pagers, many developers prefer a smarter pager configuration:

``` bash
git config --global core.pager "less -FRX"
```

This keeps the benefits of a pager while making it less intrusive.

:::

## Initialize a Repository; `git init`

To turn a folder into a Git repository, go to the folder path and type
the following command in the terminal:

``` bash
git init
```

If you want the default branch to be `main` immediately, use:

``` bash
git init -b main
```
The above command:

1.  creates a new Git repository
2.  creates the initial branch with the name `main`

This is cleaner than using `git init` first and renaming the branch later.


## Stage and Commit Changes; `git add`, `git commit` 

A common Git workflow looks like this:

``` bash
git add .
git commit -m "your message"
```

:::{note}

-   `git add .` stages the changes
-   `git commit -m "..."` records those staged changes into Git history
  
:::

 **Example**

``` bash
git add app.js
git commit -m "Fix login bug"
```

This means the changes in `app.js` are now stored as a commit with the message `Fix login bug`.


## View Commit History with `git log` 

To see commit history, use:

``` bash
git log
```

**What it shows**

For each commit, Git usually shows:

-   commit hash
-   author
-   date
-   commit message

There are several ways to show the history of a project as follows:

:::{note}
**One-line history**

``` bash
git log --oneline
```

**Graph view**

``` bash
git log --oneline --graph
```

**Last 5 commits**

``` bash
git log -n 5
```

**Show full patch with each commit**

``` bash
git log -p
```

**Show history for one file**

``` bash
git log file.txt
```
:::

## Compare Changes with `git diff`

To compare changes, use:

``` bash
git diff
```

:::{note}

There are **git diff** variations and options:

- **Basic meaning; unstaged changes**

This shows changes that are **not staged yet**.
``` bash
git diff
```

Example

``` diff
- console.log("Hello");
+ console.log("Hello World");
```


- **Staged changes**

``` bash
git diff --staged

or

git diff --cached
```
- **Compare with the latest commit**

``` bash
git diff HEAD
```

- **Compare two commits**

``` bash
git diff commit1 commit2
```

- **Compare one file**

``` bash
git diff file.txt
```

- **Show only file names**

``` bash
git diff --name-only
```
:::

## Branching; `git branch` and `git switch`

**Why Branches Matter**
A branch gives you a separate line of development. Instead of putting every change directly onto `main`, you can isolate work.

- Why Branches Are Important
Branches let you:

- keep main stable
- work on features or fixes separately
- experiment without damaging the main line
- create clean Pull Requests later
- integrate work step by step

Core Idea
A branch is not a second full copy of the project. It is a movable pointer into commit history.
Here is the most commons commands for branching and switiching

:::{note}
- **List available branches on your project**
```bash
git branch
```

- **Create a new branch**
```bash
git branch feature/login
```

- **Switch to another branch**
```bash
git switch main feature/login
```

- **Create and switch in one step**
```bash
git switch -c feature/login
```

**Why This Matters**

Branching should be understood **before** Fork and Pull Request workflows, because PRs are usually built from branches.

:::

A cleane everyday local workflow is as follows:

:::{admonition} ⭐ Summary

```text
main → create branch → make changes → commit → switch back when needed
```

Example:

```bash
git switch main
git pull
git switch -c fix/readme-typo
git add README.md
git commit -m "Fix typo in README"
```

**Key Takeaway**

Before learning collaboration on GitHub, a learner should already be comfortable with:

- creating a branch
- switching branches
- committing on a branch
- understanding that work stays isolated until integrated

When a Git repository is not connected to a remote (e.g., GitHub), the entire workflow happens locally on your machine. You create or modify files, stage changes using `git add`, and record them with `git commit`. You can organize work using branches (`git switch -c <branch>`), move between them (`git switch`), and integrate changes via merging (`git merge`). All history, experimentation, and version control remain isolated within your local repository, and no synchronization commands like `git push` or `git pull` are required since there is no external repository involved.


In the next chapter we learn how to create a remote repository and how to communicate with the local version on our machine. A clean workfolw often looks likes this:


```bash
git switch main  # Go to the main branch on your local machine
git pull  # Get all changes on the main branch of remote repo and update your main local
git switch -c fix/readme-typo  # make a new branch and switch on it and then make your edition on README.md
git add README.md # stage your changes on the fix/readme-typo branch
git commit -m "Fix typo in README" # save changes with history

```
:::

---
# What is GitHub
<a name="github"></a>

A local repository is enough for solo experimentation, but collaboration requires a shared remote repository.
GitHub provides that shared remote space.

**Why Remotes Matter**

They allow you to:

- back up your project online
- collaborate with teammates
- open Pull Requests
- fetch and push shared history

**Important Distinction**

- local Git = history on your machine
- remote GitHub repo = shared history online

**📌 Goal: Connecting an Existing Local Git Repository to GitHub**

:::{note}

You already have a local Git repository and want to:

1. Create a repository on GitHub
2. Link (connect) it to your local repo
3. Push your code to GitHub

:::

**Create a GitHub Account**

To use GitHub effectively, the first step is to create a user account.

**Steps**

1.  Go to GitHub.
2.  Click **Sign up**.
3.  Enter:
    -   your email address
    -   a password
    -   a username
4.  Verify your email address.
5.  Complete the sign-up process.

## Connect Git to GitHub

After creating your account, you can create repositories, upload code, and connect Git on your computer to GitHub.
This is the standard pipeline for creating a repository and working on it on a daily basis.


::: {note}
- **🧱 Step 1 — Create a Repository on GitHub**

1. Go to GitHub
2. Click **New repository**
3. Choose a name (e.g., `my-project`)
4. **Do NOT** initialize with README, `.gitignore`, or license
5. Click **Create repository**


- **🔗 Step 2 — Add Remote to Your Local Repository**

In your terminal (inside your project folder):

```bash
git remote add origin git@github.com:USERNAME/REPOSITORY.git
```

Example:

```bash
git remote add origin git@github.com:john/my-project.git
```

✔ This connects your local repo → GitHub repo


- **🔍 Step 3 — Verify Remote Connection**

```bash
git remote -v
```

Expected output:

```bash
origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
origin  git@github.com:USERNAME/REPOSITORY.git (push)
```

- **🚀 Step 4 — Push Your Code to GitHub**

If your main branch is `main`:

```bash
git push -u origin main
```

If it's `master`:

```bash
git push -u origin master
```

✔ `-u` sets upstream so future pushes are simpler:

```bash
git push
```
:::

:::{warning}
**⚠️ Common Issues**

- ❌ **Permission denied (publickey)**
  → SSH key not set up
- ❌ **Repository not found**
  → Wrong URL or repo name
- ❌ **Branch mismatch**
  → Use correct branch (`main` vs `master`)

:::

:::{summary}

Your workflow now becomes:

```bash
git add .
git commit -m "your message"
git push
```

And to get updates:

```bash
git pull
```
:::

##  HTTPS vs SSH for GitHub 

There are two common ways to connect Git to GitHub:

- **HTTPS**

``` bash
git clone https://github.com/user/repo.git
```
-   simpler to start with
-   commonly available by default
-   often uses a token for authentication

- **SSH**

``` bash
git clone git@github.com:user/repo.git
```
-   very common on Linux and macOS
-   excellent for repeated use
-   avoids typing credentials repeatedly
-   useful beyond Git, for server administration as well

SSH is widely considered worth learning.

---
**What Is SSH?**

**SSH** stands for **Secure Shell**. It is a **secure network protocol** used to connect to remote systems over the internet.

:::{note}
**Important clarification**

SSH is **not something separate from the internet**. It is a secure way of communicating **over** the internet.

**Why SSH matters**

Without encryption, data may be exposed. With SSH, communication is encrypted and authenticated.
:::

**Generate SSH Keys**

To use SSH with GitHub, you normally create an SSH key pair on your own
computer using the following commands:


``` bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Then your operating system creates two files:

-   **private key**
-   **public key**

Usually they are stored in:

``` bash
~/.ssh/
```

For example:

-   `~/.ssh/id_ed25519` → private key
-   `~/.ssh/id_ed25519.pub` → public key


**Which Part Stays Private and Which Part Goes to GitHub?**

**Private key**

-   stays on your computer
-   must never be shared
-   is used to prove your identity

**Public key**

-   can be shared
-   is copied to GitHub
-   allows GitHub to recognize your computer

You do **not** copy or upload the private key to GitHub.


**How GitHub Verifies That You Have the Correct Private Key**

A common question is:

- If GitHub only has my public key, how does it know I have the right private key?

The answer is cryptographic authentication.

**Simplified process**

1.  GitHub already has your **public key**
2.  During login/authentication, GitHub sends a challenge
3.  Your computer uses the **private key** to sign that challenge
4.  GitHub checks the signature using the **public key**
5.  If the signature is valid, authentication succeeds

:::{warning}
**Key point**

The **private key never leaves your computer**.
You do not paste it anywhere.
Only the cryptographic proof is sent.
:::

**Add the Public Key to GitHub** 

To display your public key so you can copy it:

``` bash
cat ~/.ssh/id_ed25519.pub
```

Copy the output and add it to your GitHub account under SSH keys.

Only the `.pub` file should be copied to GitHub.

**Test the SSH Connection**

After adding the public key to GitHub, test the connection with:

``` bash
ssh -T git@github.com
```

**Meaning of `-T`**

`-T` means:

-   do not open an interactive shell
-   only test authentication

**Successful output**

``` text
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

This means:

-   SSH is configured correctly
-   GitHub recognizes your key
-   Git operations over SSH should work

---

**Common SSH Test Errors**

---
- **First-time host verification**

You may see:

``` text
Are you sure you want to continue connecting (yes/no)?
```

Type:

``` bash
yes
```

This stores GitHub\'s host fingerprint on your machine.

---

---
- **Permission denied**

You may see:

``` text
Permission denied (publickey).
```

This usually means one of the following:

-   the public key was not added to GitHub
-   the wrong key is being used
-   the key was not loaded by the SSH agent
-   the file path is wrong

---


---

- **Check Whether Your SSH Key Exists**

To see files in your SSH directory:

``` bash
ls ~/.ssh
```

You should usually see files like:

``` text
id_ed25519
id_ed25519.pub
```

---


---
- **Check Whether the Key Is Loaded**

To list keys currently loaded in the SSH agent:

``` bash
ssh-add -l
```

If your key is not loaded, add it:

``` bash
ssh-add ~/.ssh/id_ed25519
```
---

---
- **Do You Need to Run `ssh -T git@github.com` Every Time?**

No.

You usually run:

``` bash
ssh -T git@github.com
```

only for:

-   initial setup
-   troubleshooting
-   verifying that authentication works

:::{note}
**In normal daily use**

You do **not** run the SSH test every time.

Instead, you just use Git normally:

``` bash
git clone git@github.com:user/repo.git
git pull
git push
```

SSH authentication happens automatically in the background.
:::

::: {summary}
**Workflow Summary**

A typical workflow after setup looks like this:

1.  create or enter your project folder
2.  initialize Git
3.  make changes
4.  stage changes
5.  commit
6.  push to GitHub using SSH

**Example**

``` bash
git init -b main
git add .
git commit -m "Initial commit"
git remote add origin git@github.com:username/repository.git
git push -u origin main
```
:::

::: {note}
**Recommended Mental Model**

You can think of the SSH process like this:

-   your computer keeps the **private key**
-   GitHub stores the **public key**
-   when needed, your computer proves ownership of the private key
-   GitHub verifies the proof with the public key

This is why SSH is secure and convenient.
:::


:::{warning}
-   never share the private key
-   only upload the public key
-   protect the private key with a passphrase if possible

:::

**Why learning SSH is valuable**

SSH is useful not only for GitHub, but also for:

-   remote server access
-   cloud systems
-   DevOps workflows
-   secure file transfer


:::{note}

These are all the commands you have learned so far. Try to explain what each of them does.

``` bash
# Initialize a repository
git init
git init -b main

# Stage and commit
git add .
git commit -m "message"

# View history
git log
git log --oneline
git log --oneline --graph
git log -n 5
git log -p

# View differences
git diff
git diff --staged
git diff --cached
git diff HEAD
git diff commit1 commit2
git diff file.txt
git diff --name-only

# Git configuration
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --list
git config --list --show-origin

# Disable pagers for selected commands
git config --global pager.log off
git config --global pager.diff off
git config --global pager.show off
git config --global pager.config off
git config --global pager.stash off
git config --global pager.help off
git config --global pager.blame off
git config --global pager.branch off
git config --global pager.annotate off

# Better pager setup
git config --global core.pager "less -FRX"

# Generate SSH keys
ssh-keygen -t ed25519 -C "your_email@example.com"

# Show public key
cat ~/.ssh/id_ed25519.pub

# Test SSH
ssh -T git@github.com

# Check SSH files
ls ~/.ssh

# Check/add SSH key in agent
ssh-add -l
ssh-add ~/.ssh/id_ed25519
```
::: 

## Git Clone

If you have a remote repository on GitHub and want to create a copy of it on your local machine, this process is called **cloning**.

Cloning downloads the entire repository, including its history, branches, and files, to your local environment. It also automatically connects your local repository to the remote repository.

The basic command to clone a repository is:
```bash
git clone git@github.com:OWNER/PROJECT.git

```
**After Cloning**

Once you clone a repository, Git automatically sets up a connection to the remote repository. You can verify this using:

```bash
git remote -v
```

**What Does `origin` Mean?**

After cloning, Git assigns a default name to the remote repository:
```
origin
```

- `origin` is simply a **conventional name** for the remote repository
- It refers to the source you cloned from
- You can rename it, but most developers keep it as `origin`

:::{note}
**Common Commands**

- **Push (send changes to GitHub)**

```bash
git push origin main
```

Sends your local commits to the main branch on the remote repository
- **Pull (get updates from GitHub)**

```bash
git pull
```
Fetches and merges changes from the remote repository into your local branch Key Concepts
:::

## GitHub Collaboration Models

There are two major GitHub collaboration models: **Fork** workflow and **collaboration** workfolw.  **Fork** Workflow
used when you do **not** have write access to the main repository and **Contributor** Workflow used when you **do** have write access to the main repository.

---
### Fork

A **fork** is a copy of a GitHub repository that is created under **your own GitHub account**. This is different from a local copy on your
computer. A fork lives on **GitHub**, not just on your machine.

**Mental Model**

Suppose there is an original repository:

-   original repository: `github.com/original-owner/project`
-   your fork: `github.com/your-username/project`

The original repository is often called:

-   **upstream**

Your personal copy is usually connected as:

-   **origin**

So in a typical fork workflow:

-   `upstream` = the original repository
-   `origin` = your fork on GitHub

---
**When Do We Use a Fork?**

Forks are commonly used when you **do not have direct write access** to the original repository.

**Typical Cases**

1.  **Open-source contribution**
    You want to improve a public project, but you are not part of the
    core team.

2.  **Safe experimentation**
    You want to try changes freely without affecting the original
    repository.

3.  **Personal customization**
    You want to maintain your own version of a project with custom
    changes.

4.  **External collaboration**
    You are contributing from outside the main organization or team.

---
**When a Fork Is Usually Not Needed**

If you are already a member of the project team and have write access, the team may prefer this workflow instead:

-   clone the main repository directly
-   create a branch inside that repository
-   push your branch
-   open a Pull Request from that branch

---

:::{note}

***Fork vs Clone***

These two concepts are related, but they are not the same.

- **Fork**: A **fork** creates a copy of a repository on **GitHub** under your account.

- **Clone**: A **clone** creates a copy of a repository on **your local computer**.

:::

**Standard Order**

In a fork-based contribution workflow, the normal order is:

``` text
Fork on GitHub → Clone to your computer
```

So first you create the fork online, and then you clone **your fork** locally.

**Do We Get the Full History After Forking?**

Yes. In normal GitHub usage, a fork preserves the repository history. That means your fork contains:

-   commits
-   branches and references relevant to the forked project
-   tags (depending on repository state and hosting behavior)
-   the complete evolution of the project

So conceptually, you inherit the project history. However, **having the history does not automatically make you a contributor** to the original project.

**Are You a Contributor Just Because You Forked?**

No. Forking a repository means:

-   you now have your own copy of the project
-   you can work on it independently
-   you can propose changes

But it does **not** mean that you are already a contributor to the original repository.

**When Are You Usually Considered a Contributor?**

In practice, you become a contributor when your changes are accepted into the original project, usually by:

-   opening a Pull Request
-   having it reviewed
-   and getting it merged

So this statement is accurate:

**You may have the full repository history in your fork, but you are not yet a contributor to the original project until your contribution is accepted there.**

---
**The Full Fork Workflow**

Here is the standard end-to-end workflow:

``` text
Fork → Clone → Add upstream → Create branch → Edit → Commit → Push → Pull Request → Review → Merge
```

We will now go through each part carefully.

- **Step 1: Fork the Repository on GitHub**

On GitHub:

1.  open the original repository
2.  click **Fork**
3.  GitHub creates a copy under your own account

Example:

-   original: `github.com/original-owner/project`
-   fork: `github.com/your-username/project`

At this point, you have your own GitHub-hosted copy of the project.

- **Step 2: Clone Your Fork Locally**

After forking, clone **your fork**, not the upstream repository.

``` bash
git clone git@github.com:YOUR_USERNAME/PROJECT.git
cd PROJECT
```

Now your local repository is connected to your fork as `origin`.

- **Step 3: Add the Original Repository as `upstream`**

This is a very important step. You usually want two remotes:

-   `origin` → your fork
-   `upstream` → the original repository

Add `upstream` like this:

``` bash
git remote add upstream git@github.com:ORIGINAL_OWNER/PROJECT.git
```

Then verify your remotes:

``` bash
git remote -v
```

Expected result:

``` text
origin    git@github.com:YOUR_USERNAME/PROJECT.git
upstream  git@github.com:ORIGINAL_OWNER/PROJECT.git
```

**Why Is `upstream` Important?**

Because the original project keeps moving forward. If you only work with your fork and never sync from upstream:

1. your fork becomes outdated
2. your branch may drift away from the current project state
3. your Pull Request may become harder to review or merge


- **Step 4: Create a New Branch for Your Change**

You should **not** usually work directly on `main` for a contribution. Instead, create a branch for each separate change.

**Why?**

Because branches help you:

-   isolate your work
-   keep `main` clean
-   open focused Pull Requests
-   revise one change without mixing it with another

**Recommended Command**

``` bash
git switch -c fix-readme-typo
```

This command does two things at once:

1.  creates a new branch named `fix-readme-typo`
2.  switches to that branch immediately

- **Step 5: Make Your Changes**

Now edit the project files.

Examples:

-   fix a typo
-   improve documentation
-   add a small feature
-   refactor a function
-   update tests

At this point, your changes are only on your local branch.

- **Stage and Commit the Changes**

After editing, stage and commit your work.

``` bash
git add .
git commit -m "Fix typo in installation guide"
```

**Good Commit Message Advice**

A good commit message is:

-   specific
-   short
-   action-oriented

Examples:

-   `Fix typo in README`
-   `Add validation for email field`
-   `Improve setup instructions for macOS`

Avoid vague messages like:

-   `update`
-   `change stuff`
-   `fix`

- **Step 6: Push the Branch to Your Fork**

Now push your branch to `origin`, which is your fork.

``` bash
git push origin fix-readme-typo
```

This sends your branch to GitHub under **your fork**.

- **Step 7: Open a Pull Request**

This is the key step. A **Pull Request (PR)** is a request asking the maintainers of the original project to review and potentially merge your changes.

**Meaning of a Pull Request**

A Pull Request is basically saying:

**I made these changes in my branch. Please review them and merge them into the main project if they are acceptable.**

**Typical Direction**

``` text
your fork / your branch  →  upstream / main
```
Example:

-   source: `your-username:fix-readme-typo`
-   target: `original-owner:main`

**How to Create a Pull Request on GitHub**

Typical steps:

1.  push your branch to your fork
2.  go to your fork on GitHub
3.  GitHub often shows a **Compare & pull request** button
4.  click it
5.  confirm the base repository and base branch
6.  confirm your source branch
7.  write a clear title
8.  write a helpful description
9.  submit the Pull Request

**Good PR Title**

-   `Fix typo in installation guide`
-   `Add missing null check in login flow`

**Good PR Description Should Explain**

-   **Problem**: What issue are you solving?
-   **Change**: What did you modify?
-   **Reasoning**: Why is this the right change?
-   **Testing**: How did you verify it?
-   **Scope**: Is this a small focused change or a broader change?


Example PR Description

``` bash
## Summary
This PR fixes a typo in the installation section of the README.

## Details
The command name was missing a dash, which could confuse new users.

## Testing
No code changes were made. Documentation reviewed manually.
```

This helps maintainers review quickly and confidently.

**What Happens After You Open a PR?**
After you submit the Pull Request, several things may happen:

1.  **Maintainers review it**
2.  **Automated checks may run**
    -   tests
    -   formatting
    -   linting
    -   CI pipelines
3.  **Reviewers may approve it**
4.  **Reviewers may request changes**
5.  **Maintainers may merge it**
6.  In some cases, they may close it without merging

This is normal. A closed PR is not necessarily a failure. Sometimes it simply means the project chose a different direction.

**What If Reviewers Request Changes?**

This is very common and completely normal. You do **not** usually create a new Pull Request for small requested changes.

Instead, you:

1.  stay on the same branch
2.  make the requested updates
3.  commit the new changes
4.  push again to the same branch

Example:

``` bash
git add .
git commit -m "Address review comments"
git push origin fix-readme-typo
```

When you push to the same branch, the existing Pull Request updates automatically.

**Why Branches Matter So Much for Pull Requests**

Branches are central to PR workflows. A Pull Request is typically tied to:

-   one source branch
-   one target branch

Because of this, branches give you:

-   clean separation of work
-   easier review
-   better rollback options
-   less risk of mixing unrelated changes

:::{warning}
**Keeping Your Fork Up to Date**
The upstream repository changes over time. If you do not sync your fork, you can end up working on an old base.

Common Sync Workflow is

``` bash
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

-   `git fetch upstream`\
    downloads the latest state from the original project

-   `git switch main`\
    returns you to your local main branch

-   `git merge upstream/main`\
    brings upstream changes into your local main

-   `git push origin main`\
    updates your fork\'s `main` on GitHub

:::


**Alternative: Rebase Instead of Merge**

Some projects prefer a cleaner linear history and may encourage `rebase` instead of `merge`.

Example:

``` bash
git fetch upstream
git switch my-feature
git rebase upstream/main
```

**Why Rebase?**

Rebase rewrites your branch so it appears to start from the latest upstream state. This can make history cleaner, but it is conceptually more advanced than merge.


#### Merge, Rebase, and Conflict Resolution

This section adresses three critical Git topics:

-   **merge**
-   **rebase**
-   **conflict resolution**

These concepts are essential after learning branching, forks, and Pull Requests, because real collaboration almost always involves integrating
changes from multiple branches. By the end of this section, you should be able to:

-   explain what **merge** does
-   explain what **rebase** does
-   distinguish the workflows and history produced by each
-   understand when a team may prefer **merge** or **rebase**
-   detect and resolve **merge conflicts**
-   continue or abort a merge or rebase safely
-   understand why conflicts happen
-   use practical commands in real project scenarios

**Why These Topics Matter**

As soon as more than one branch exists, integration becomes necessary.

Examples:

-   you created a feature branch and now want to bring it into `main`
-   the upstream repository moved forward while you were working
-   your Pull Request is out of date
-   two developers changed the same file in incompatible ways

At that point, Git must combine histories and file changes. That is where **merge**, **rebase**, and **conflict resolution** become central.

**The Big Picture**

There are two common ways to integrate branch histories:

1.  **Merge**: preserves the branching history and adds a merge commit
2.  **Rebase**: rewrites the branch so it appears to start from a new base

Neither is universally "better". The right choice depends on workflow, team preference, and whether the branch has already been shared.

Imagine this simplified history.

**Before Integration**

``` 
A---B---C   main
     \
      D---E   feature
```

**After Merge**

``` 
A---B---C--------M   main
     \          /
      D---E----/    feature
```

`M` is a merge commit.

**After Rebase**

``` 
A---B---C---D'---E'   feature
```

The rebased commits `D'` and `E'` are new versions of the original commits `D` and `E`.


**What Is a Merge?**

A **merge** combines the histories of two branches. Suppose you have:

-   `main`
-   `feature`

You worked on `feature`, and now you want those changes in `main`. A common workflow is:

``` bash
git switch main
git merge feature
```

Git then attempts to combine the histories.

If it succeeds without conflict:

-   your work is integrated into `main`
-   Git may create a **merge commit**
-   the branch structure remains visible in history


**What Is a Rebase?**

A **rebase** moves a branch so that it is replayed on top of another base.

Suppose this happened:

-   `main` moved ahead
-   your `feature` branch was created earlier
-   you now want your branch to sit on top of the latest `main`

You might run:

``` bash
git switch feature
git fetch origin
git rebase origin
```

Git then takes the commits from `feature` and **reapplies** them one by one on top of the newer base.


**Typical Rebase Workflow for a Feature Branch**

Suppose your branch is behind `main` and you want to update it before opening or finalizing a PR.

``` bash
git fetch origin
git switch feature
git rebase origin/main
```

Then, if the branch was already pushed before, you may need:

``` bash
git push --force-with-lease origin feature
```

Because after rebase, the branch history changed. A normal push may be rejected.

`--force-with-lease` is safer than plain `--force` because it checks that the remote state is what you expect before overwriting it.

``` bash
git fetch origin
git switch feature
git rebase origin/main
git push --force-with-lease origin feature
```

**What Is a Conflict?**

A **conflict** happens when Git cannot automatically decide how to combine changes.
This usually happens when:

-   two branches changed the same lines
-   one branch deleted a file that another branch edited
-   structural changes overlap in incompatible ways
-   two developers edited the same code block
-   branch A renamed or deleted something that branch B still uses
-   the code evolved in two directions simultaneously
-   a long-lived branch fell far behind `main`

Git is very good at automatic merging, but it cannot safely guess human intent in every case. The longer a branch lives without syncing, the more likely conflicts become.

**Example of a Conflict**

Imagine `main` has:

``` bash
def greet():
    return "Hello"
```

And your feature branch changed it to:

``` bash
def greet():
    return "Hello, user"
```

But meanwhile `main` changed it to:

``` python
def greet():
    return "Hi"
```

Git now sees that the same lines were changed differently. It cannot confidently choose one, so it reports a conflict.

When a conflict happens, Git inserts markers into the file, such as:

``` text
<<<<<<< HEAD
return "Hi"
=======
return "Hello, user"
>>>>>>> feature/login
```

That means

-   `<<<<<<< HEAD`\
    the current branch\'s version

-   `=======`\
    separator

-   `>>>>>>> feature/login`\
    the incoming branch\'s version

You must edit the file manually and remove these markers.
If a conflict happens during merge:

``` bash
git switch main
git merge feature
```

Git may stop and tell you which files are conflicted.

Then the normal process is:

1.  open conflicted files
2.  decide the correct final content
3.  remove conflict markers
4.  stage the resolved files
5.  complete the merge

---
**Conflict During merge**
Here is the standard sequence.

- **Step 1:Attempt the merge**

``` bash
git switch main
git merge feature/login
```

- **Step 2: Check status**

``` bash
git status
```

Git shows which files are unmerged.

- **Step 3: Open each conflicted file**

Look for markers like:

``` text
<<<<<<<
=======
>>>>>>>
```

- **Step 4: Edit to the correct final result**

Choose:

-   your version
-   their version
-   or a combination

- **Step 5: Stage resolved files**

``` bash
git add path/to/file
```

- **Step 6: Finish the merge**

If needed:

``` bash
git commit
```

Sometimes Git prepares the merge commit automatically after staging, depending on the conflict path and tooling.


**Abort a Merge**

Sometimes you decide the current integration attempt is not worth continuing right now.


``` bash
git merge --abort
```

This attempts to return the repository to the pre-merge state.

---
**Conflict During Rebase**

If a conflict happens during rebase:

``` bash
git switch feature/login
git rebase origin/main
```

Git stops at the conflicting commit.

Then you:

1.  open the conflicted files
2.  resolve the conflict
3.  stage the fixed files
4.  continue the rebase

Command:

``` bash
git add .
git rebase --continue
```

If more conflicts appear, repeat the same process.
**Optional: Skip a problematic commit**

``` bash
git rebase --skip
```

**Optional: Abort the whole rebase**

``` bash
git rebase --abort
```
---
**Useful Commands During Conflict Resolution**

- **See current status**

``` bash
git status
```

- **See differences**

``` bash
git diff
```

- **See staged differences**

``` bash
git diff --staged
```

**Conflict Prevention Strategies** 

You cannot eliminate all conflicts, but you can reduce them.

Good Practices

1.  **Keep branches short-lived**
    Long-lived branches drift and conflict more.

2.  **Sync frequently with main or upstream**

    -   merge from main regularly
    -   or rebase regularly if that is the team workflow

3.  **Make smaller Pull Requests**
    Small focused changes are easier to integrate.

4.  **Communicate with teammates**
    If two people plan to edit the same subsystem, coordinate early.

5.  **Avoid giant unrelated commits**
    Big mixed commits make conflict analysis much harder.


**What Is Squash Merge?**

A related concept is **squash merge**. Instead of preserving all commits from the feature branch, the branch is merged as **one single commit**.

This is often offered in GitHub Pull Requests.

**Why Teams Use It**

-   cleaner history on `main`
-   less noise from many small "work in progress" commits
-   easier project history reading

**Important Distinction**

-   squash merge is not the same as rebase
-   rebase rewrites branch history before integration
-   squash merge compresses branch history at integration time


### Contributor

This section is for the case where you are already a **contributor** or team member and have **write access** to the main repository. In this
situation, the workflow is usually different from the fork-based open-source workflow. Instead of:

``` text
Fork → Clone → Branch → Push to your fork → Pull Request
```

you often use:

``` text
Clone main repository → Create branch → Commit → Push branch → Pull Request
```

or, in some teams:

``` text
Clone main repository → Create branch → Commit → Push branch → Merge directly
```

depending on team policy.


By the end of this section, you should be able to:

-   understand how contributor workflow differs from fork workflow
-   know when a fork is unnecessary
-   clone the main repository directly
-   create and manage contribution branches inside the main repository
-   push branches to the shared repository
-   open Pull Requests from a branch in the same repository
-   understand when direct pushes to `main` are discouraged
-   work safely in a collaborative team environment

**What Changes When You Are a Contributor?**

When you are a **contributor with write access**, you no longer need your own fork in order to propose changes.
That is the key difference.

**Fork-Based Workflow**

Used when:

-   you do **not** have write access
-   you contribute from outside the core team
-   you submit changes through your own fork

**Contributor Workflow**

Used when:

-   you **do** have write access
-   you are part of the organization or trusted team
-   you can push branches directly to the main repository

So if you are already a contributor, the repository itself can usually act as the shared collaboration space.

**Do Contributors Still Use Pull Requests?**

Very often, yes.

Having write access does **not** always mean:

-   push directly to `main`
-   merge without review
-   skip collaboration rules

In many professional teams, even contributors with write access still:

-   create branches
-   push those branches to the shared repository
-   open Pull Requests
-   wait for review
-   merge only after approval

Why?

Because Pull Requests are not only about permissions.
They are also about:

-   review quality
-   design discussion
-   CI checks
-   documentation of why a change happened
-   protecting the stability of `main`

**High-Level Contributor Workflow**

A common contributor workflow looks like this:

``` text
Clone main repository → Create feature branch → Edit → Commit → Push branch → Open Pull Request → Review → Merge
```

This is similar to the fork workflow, but the difference is where the branch lives:

-   in fork workflow, the branch lives in **your fork**
-   in contributor workflow, the branch lives in the **main shared repository**

- **Step 1: Clone the Main Repository**

Since you already have access, you normally clone the main repository directly.

``` bash
git clone git@github.com:ORGANIZATION/PROJECT.git
cd PROJECT
```

There is no need to fork first.

Your `origin` now points directly to the shared repository.

**Understanding `origin` in Contributor Workflow**

In a fork-based workflow:

-   `origin` usually points to your fork
-   `upstream` points to the original repository

But in a contributor workflow:

-   `origin` usually points directly to the shared repository

Example:

``` bash
git remote -v
```

Possible output:

``` text
origin  git@github.com:ORGANIZATION/PROJECT.git
```

In many cases, there is no need for an `upstream` remote at all, because you are already working directly with the main repository.

- **Step 2: Update Your Local Main Branch**

Before starting new work, make sure your local `main` is up to date.

``` bash
git switch main
git pull
```

This is important because you usually want to branch from the latest project state.

- **Step 3: Create a Feature Branch**

Even when you are a contributor, it is usually best **not** to work directly on `main`. Instead, create a focused branch:

``` bash
git switch -c feature/improve-login-validation
```

or:

``` bash
git switch -c fix/readme-typo
```

**Why Branches Still Matter**

Branches let you:

-   isolate one change
-   keep `main` stable
-   make review easier
-   avoid mixing unrelated work
-   simplify rollback and debugging

**Should Contributors Push Directly to `main`?**

Usually, no.

Even if technically allowed, many teams discourage or forbid direct
pushes to `main`.

**Why Direct Pushes Are Risky**

They can:

-   bypass code review
-   skip discussion
-   introduce unstable code
-   make auditing harder
-   break CI expectations
-   surprise teammates

**Better Practice**

Use:

-   short-lived feature branches
-   Pull Requests
-   review gates
-   protected branch rules

In modern team workflows, `main` is often protected specifically to prevent accidental direct pushes.

**Protected Branches**

Many GitHub repositories use **protected branches**.

A protected branch may require:

-   Pull Request before merge
-   at least one approval
-   passing CI checks
-   resolved conversations
-   linear history
-   no force-pushes
-   no direct pushes

This means that even contributors with write access still follow a formal workflow.
So contributor status gives you **access**, but not necessarily unrestricted freedom.

- **Step 4: Make Changes and Commit**

After creating your branch, edit the necessary files and commit your work.

``` bash
git add .
git commit -m "Improve login validation for empty email input"
```

**Good Commit Message Advice**

Strong commit messages are:

-   clear
-   focused
-   action-based
-   easy for reviewers to understand

``` bash
git add .
git commit -m "Improve login validation for empty email input"
```

- **Step 5: Push Your Branch to the Shared Repository**

Now push your branch to the main shared repository:

``` bash
git push origin feature/improve-login-validation
```

This is a major difference from the fork workflow.

:::{note}

**In Fork Workflow**

You push to:

``` text
your fork
```

**In Contributor Workflow**

You push to:

``` text
the main repository itself
```
:::


- **Step 6: Open a Pull Request from the Same Repository**
  
Now create a Pull Request. In this case, both the source branch and target branch are usually in the **same repository**.

Example direction:

``` text
ORGANIZATION/PROJECT:feature/improve-login-validation
    →
ORGANIZATION/PROJECT:main
```

This is different from the fork model, where the source branch lives in your fork.

**Why PRs Still Matter for Contributors**

A Pull Request is useful even when both branches are in the same repository.

PRs provide:

-   peer review
-   architectural feedback
-   CI validation
-   historical record of discussion
-   visibility for teammates
-   a checkpoint before code reaches `main`

In strong engineering teams, the Pull Request is a collaboration tool, not just a permission workaround.

:::{note}
**Same-Repository PR vs Fork PR**

**Fork PR**

``` text
your-username/PROJECT:my-branch
    →
ORIGINAL_OWNER/PROJECT:main
```

**Contributor PR**

``` text
ORGANIZATION/PROJECT:my-branch
    →
ORGANIZATION/PROJECT:main
```

**Key Difference**

The branch source lives in a different place:

-   fork PR → branch in your fork
-   contributor PR → branch in the main repository

:::

**Team Naming Conventions for Branches**

Professional teams often use naming conventions such as:

-   `feature/add-export-button`
-   `fix/login-timeout`
-   `docs/update-installation-guide`
-   `refactor/auth-service`
-   `test/add-user-service-tests`

These names help reviewers understand branch intent quickly.

---
**Typical Contributor Workflow Example**

Let us imagine you are part of the team and want to fix a bug.

**Steps**

1.  clone the main repository
2.  switch to `main`
3.  pull latest changes
4.  create branch `fix/login-timeout`
5.  make your changes
6.  commit
7.  push branch to `origin`
8.  open Pull Request
9.  address review comments
10. merge after approval

``` bash
git clone git@github.com:ORGANIZATION/PROJECT.git
cd PROJECT
git switch main
git pull
git switch -c fix/login-timeout
git add .
git commit -m "Fix login timeout handling"
git push origin fix/login-timeout
```
---

**What If Reviewers Request Changes?**

Just like in fork workflow, you usually stay on the same branch.

Example:

``` bash
git add .
git commit -m "Address review comments"
git push origin fix/login-timeout
```

The Pull Request updates automatically because it is tracking that same branch.

**Syncing Your Branch with Main**

While your Pull Request is open, `main` may move forward. You may need to update your branch.

- Option A: Merge `main` into your branch

``` bash
git switch fix/login-timeout
git fetch origin
git merge origin/main
```

- Option B: Rebase onto `main`

``` bash
git switch fix/login-timeout
git fetch origin
git rebase origin/main
```

Which option to use depends on team policy.

**What If the Team Allows Direct Pushes?**

Some small teams or personal team projects do allow direct pushes to `main`. That workflow might look like:

``` bash
git switch main
git pull
git add .
git commit -m "Small update"
git push origin main
```

:::{warning}

Even if allowed, direct pushes are usually best reserved for:

-   tiny low-risk changes
-   emergency hotfixes
-   trusted internal workflows
-   solo-maintainer repositories

For most collaborative work, feature branches and PRs are still safer.
:::


:::{warning}
**Common Mistakes Contributors Make**

1.  Working directly on `main`. This increases risk and reduces review quality.

2. Pushing half-finished work to shared branches without clarity. Teammates may review unstable code too early.

3. Opening giant PRs. Smaller changes are easier to review and merge.

4. Ignoring branch naming conventions. This makes the shared repository harder to navigate.

5. Forgetting to pull before creating a branch. You may branch from outdated `main`.

6. Rebasing shared team branches carelessly. If others use the same branch, history rewriting becomes dangerous.

:::

:::{summary}

**Suggested Best Practices for Contributors** 
1.  always update `main` before branching
2.  create one branch per topic or fix
3.  use clear names for branches
4.  commit logically, not randomly
5.  open Pull Requests early enough for review
6.  keep PRs focused and readable
7.  merge or rebase from `main` regularly if the branch stays open
8.  follow repository rules and templates
9.  do not treat write access as permission to skip discipline
10. protect `main` whenever possible

:::

## Correction using  `Git reset`, `revert`, `restore`, `amend`, and `cherry-pick`

This section help to answer real-world questions such as:

-   How do I undo something safely?
-   How do I fix the last commit?
-   How do I restore a file?
-   How do I move one specific commit to another branch?
-   When should I rewrite history, and when should I preserve it?

By the end of this section, you should be able to:

-   explain the difference between **reset** and **revert**
-   understand the purpose of **restore**
-   amend the most recent commit safely
-   move a specific commit with **cherry-pick**
-   choose the correct tool depending on whether history should be rewritten or preserved
-   avoid common mistakes when undoing or reusing work

**Why These Commands Matter** 

In real Git usage, people often need to correct mistakes.

Examples:

-   you committed too early
-   you forgot to include one file in the last commit
-   you want to undo a bad commit without deleting history
-   you accidentally staged the wrong file
-   you want to bring one bug fix from one branch into another
-   you want to discard local edits and go back to the committed state

These are not rare situations. They are part of normal Git work. That is why `reset`, `revert`, `restore`, `amend`, and `cherry-pick` are
core professional tools. A useful mental model is this:

-   **reset** → move branch pointers and optionally unstage or discard changes
-   **revert** → create a new commit that undoes an earlier commit
-   **restore** → restore file contents or unstage changes
-   **amend** → modify the most recent commit
-   **cherry-pick** → apply one specific commit onto another branch

### Another Useful Distinction

Some commands primarily **rewrite local history**:

-   `reset`
-   `commit --amend`

Some commands primarily **preserve history while undoing effects**:

-   `revert`

Some commands primarily **manipulate working tree or staging area
state**:

-   `restore`

Some commands **reuse existing work across branches**:

-   `cherry-pick`

## 3. Working Tree, Staging Area, and Commit History {#3-working-tree-staging-area-and-commit-history}

Before using undo commands well, you need this model:

### Working Tree

The actual files in your project directory.

### Staging Area

The set of changes prepared for the next commit.

### Commit History

The already-recorded snapshots in Git.

Many Git commands differ because they affect different layers.

For example:

-   one command may only unstage files
-   another may change the working tree
-   another may move the branch pointer
-   another may add a brand-new commit

That is why these commands can feel similar at first but behave very
differently.

## 4. `git reset`: Core Idea {#4-git-reset-core-idea}

`git reset` is one of the most powerful and potentially dangerous Git
commands.

Its core purpose is to move **HEAD** and sometimes also affect:

-   the staging area
-   the working tree

### Conceptual Meaning

At a high level, `reset` says:

> Move my current branch to a different commit, and possibly adjust
> staged or working changes to match.

Because of that, `reset` can be:

-   very helpful
-   very destructive if used carelessly

## 5. Common Forms of `git reset` {#5-common-forms-of-git-reset}

Three common modes are:

-   `--soft`
-   `--mixed`
-   `--hard`

### `--soft` {#--soft}

Moves the branch pointer, but keeps changes staged.

### `--mixed` {#--mixed}

Moves the branch pointer and unstages changes, but keeps file
modifications in the working tree.

### `--hard` {#--hard}

Moves the branch pointer, unstages changes, and resets files in the
working tree.

This is the most dangerous form because it can discard local work.

## 6. `git reset --soft` {#6-git-reset---soft}

Example:

``` bash
git reset --soft HEAD~1
```

This means:

-   move the current branch back by one commit
-   keep the changes from that commit staged

### Use Case

You made a commit, but now want to:

-   rewrite the message
-   split the work differently
-   recommit in a cleaner way

This is useful when the commit itself was premature, but the content is
still correct.
:::

::: {#a67a9dc3-4f22-4361-80ce-e05a0eaf2d08 .cell .code}
``` python
git reset --soft HEAD~1
```
:::

::: {#fb1bd8cc-f7d3-4c81-bd52-9e919a8d5fa4 .cell .markdown}
## 7. `git reset --mixed` {#7-git-reset---mixed}

This is the default if you do not specify a mode.

Example:

``` bash
git reset HEAD~1
```

or explicitly:

``` bash
git reset --mixed HEAD~1
```

This means:

-   move back one commit
-   keep file changes in your working directory
-   unstage them

### Use Case {#use-case}

You want to undo a commit, but then re-stage the files more selectively.
:::

::: {#e31a3d6c-46d6-4517-ab9e-913da1759888 .cell .code}
``` python
git reset HEAD~1
git reset --mixed HEAD~1
```
:::

::: {#ce056674-9d3f-4dc3-a963-a7d8bf353620 .cell .markdown}
## 8. `git reset --hard` {#8-git-reset---hard}

Example:

``` bash
git reset --hard HEAD~1
```

This means:

-   move back one commit
-   reset the staging area
-   reset the working tree
-   discard local changes that were part of that state transition

### Important Warning {#important-warning}

`--hard` can permanently destroy local work that is not otherwise
recoverable through reflog or other advanced recovery methods.

Use it only when you are sure.
:::

::: {#77986ca3-ff08-4f3c-86e6-c3c8f98f84da .cell .code}
``` python
git reset --hard HEAD~1
```
:::

::: {#ac3788c3-fe14-411c-abf4-3d2260eea761 .cell .markdown}
## 9. When Is `reset` Appropriate? {#9-when-is-reset-appropriate}

`reset` is most appropriate when:

-   you are cleaning up **local history**
-   you have **not safely shared** the commits yet
-   you want to rewrite or remove recent local commits
-   you want to unstage files
-   you want to discard local changes intentionally

### Practical Rule

`reset` is usually best for **local correction**.\
It is more dangerous when the commits were already pushed and other
people may rely on them.

## 10. Why `reset` Can Be Dangerous on Shared Branches {#10-why-reset-can-be-dangerous-on-shared-branches}

If you use `reset` on commits that were already pushed to a shared
branch:

-   history changes
-   commit IDs may disappear from the visible branch
-   collaborators may still have the old history
-   force-pushing may become necessary
-   confusion and integration problems can follow

### Safer Rule

Use `reset` freely on your own local mistakes.\
Use it very carefully on anything already shared.

## 11. `git revert`: Core Idea {#11-git-revert-core-idea}

`git revert` is different from `reset`.

Instead of moving history backward, `revert` creates a **new commit**
that undoes the effect of an earlier commit.

### Conceptual Meaning {#conceptual-meaning}

At a high level:

> Keep the history, but add a new commit that reverses the change.

This makes `revert` much safer for shared history.

## 12. Example of `git revert` {#12-example-of-git-revert}

``` bash
git revert abc1234
```

This tells Git:

-   find commit `abc1234`
-   compute the inverse of its effect
-   create a new commit applying that inverse

### Result

The original commit remains in history.\
The new revert commit records that its effects were undone.
:::

::: {#4803fbd8-6c9b-4a92-a5a0-e902327fcfd1 .cell .code}
``` python
git revert abc1234
```
:::

::: {#8634c86e-1b5d-474e-9b15-42a70ecb34cb .cell .markdown}
## 13. When Should You Prefer `revert`? {#13-when-should-you-prefer-revert}

Prefer `revert` when:

-   the bad commit was already pushed
-   the branch is shared
-   you want a clear audit trail
-   you need to undo a change safely without rewriting history

### Typical Professional Rule

-   **local mistake not yet shared** → often `reset`
-   **shared bad commit** → often `revert`

## 14. `reset` vs `revert` {#14-reset-vs-revert}

This distinction is fundamental.

### `reset`

-   rewrites branch position
-   can rewrite visible history
-   best for local cleanup
-   can be dangerous on shared branches

### `revert`

-   does not remove the original commit
-   adds a new undo commit
-   safer for shared history
-   leaves a clear record of what happened

### Mental Shortcut

-   **reset** = "pretend the branch pointer moved back"
-   **revert** = "add a new commit that cancels an old one"

## 15. `git restore`: Core Idea {#15-git-restore-core-idea}

`git restore` was introduced to make certain file-level operations
clearer.

It is mainly used to:

-   restore file content in the working tree
-   unstage files from the index

It helps separate these actions from older overloaded `checkout`
behavior.

## 16. Restore a File in the Working Tree {#16-restore-a-file-in-the-working-tree}

Example:

``` bash
git restore app.py
```

This means:

-   discard local modifications in `app.py`
-   restore it from the current `HEAD` state

### Use Case {#use-case}

You edited a file locally and want to abandon those uncommitted edits.
:::

::: {#cf0167f0-1595-4c1f-9237-0ef7cf466593 .cell .code}
``` python
git restore app.py
```
:::

::: {#5ae2a60a-eaff-48fd-9dfc-3e666b2253dd .cell .markdown}
## 17. Unstage a File with `restore` {#17-unstage-a-file-with-restore}

Example:

``` bash
git restore --staged app.py
```

This means:

-   remove `app.py` from the staging area
-   keep the file changes in the working tree

### Use Case {#use-case}

You accidentally staged a file and want to unstage it without losing the
edit.
:::

::: {#62776468-c5ce-4d43-af4b-d985c0944ed2 .cell .code}
``` python
git restore --staged app.py
```
:::

::: {#1a2fb97e-f813-471d-b5aa-2472add410cf .cell .markdown}
## 18. Restore from Another Source {#18-restore-from-another-source}

You can also restore a file from a particular commit.

Example:

``` bash
git restore --source=abc1234 README.md
```

This means:

-   take `README.md` as it existed in commit `abc1234`
-   restore it into the working tree

This is useful when you want one file from an earlier point without
changing the whole branch history.
:::

::: {#02a652b9-15fb-49e4-8ea1-b3c0667bf935 .cell .code}
``` python
git restore --source=abc1234 README.md
```
:::

::: {#ac560565-f0b6-4975-826a-ca95c7a87c54 .cell .markdown}
## 19. Why `restore` Is Useful {#19-why-restore-is-useful}

`restore` is useful because it lets you think in terms of **file
state**, not only commit history.

It is especially helpful for:

-   undoing accidental local edits
-   unstaging files cleanly
-   restoring a single file from a chosen commit

It is often safer and clearer than reaching for `reset` when your goal
is only file-level correction.

## 20. `git commit --amend`: Core Idea {#20-git-commit---amend-core-idea}

Amend changes the **most recent commit**.

It is commonly used when:

-   the last commit message is wrong
-   you forgot to include one file
-   you want to slightly adjust the most recent commit

### Example {#example}

``` bash
git commit --amend
```

Git opens the commit message editor and lets you replace the last commit
with a new version.
:::

::: {#cfb72ee6-18e7-4633-acfb-f9be287e4216 .cell .code}
``` python
git commit --amend
```
:::

::: {#641c354e-edc8-4507-b481-9c058ad58d3f .cell .markdown}
## 21. Amend the Last Commit Message {#21-amend-the-last-commit-message}

Example:

``` bash
git commit --amend -m "Fix login validation for empty input"
```

This replaces the latest commit message with a new one.

### Important Note

Even changing only the message rewrites the commit.\
That means the commit hash changes.
:::

::: {#bf6b926e-8581-4ea5-b9d6-0911c01d0965 .cell .code}
``` python
git commit --amend -m "Fix login validation for empty input"
```
:::

::: {#e77dd471-ca38-41af-a09c-a1efa5f77b7d .cell .markdown}
## 22. Amend to Add a Forgotten File {#22-amend-to-add-a-forgotten-file}

Suppose you made a commit but forgot one file.

Workflow:

``` bash
git add missing_file.py
git commit --amend
```

Now Git rebuilds the latest commit to include that file.

This is a very common and useful correction pattern.
:::

::: {#fe543d25-2fae-45e3-b037-cc19f81db047 .cell .code}
``` python
git add missing_file.py
git commit --amend
```
:::

::: {#d5cbafca-9528-41d0-8aa7-8d76e1c1f15e .cell .markdown}
## 23. When Is Amend Safe? {#23-when-is-amend-safe}

Amend is safest when:

-   the commit is still local
-   you have not pushed it yet
-   nobody else is depending on that exact commit hash

### Warning

If you amend a commit that was already pushed:

-   the commit hash changes
-   force-push may be needed
-   collaborators can be confused if they already used the old commit

## 24. `git cherry-pick`: Core Idea {#24-git-cherry-pick-core-idea}

`git cherry-pick` applies the effect of one specific commit onto your
current branch.

### Conceptual Meaning {#conceptual-meaning}

At a high level:

> Take that commit over there, and replay its effect here.

This is very useful when you do **not** want to merge a whole branch,
but only want one particular change.

## 25. Example of `cherry-pick` {#25-example-of-cherry-pick}

Suppose commit `abc1234` on another branch contains an important bug
fix.

You are on your current branch and run:

``` bash
git cherry-pick abc1234
```

Git then attempts to apply that commit here as a new commit.

### Result {#result}

You get the effect of the original commit, but the new branch history
remains separate.
:::

::: {#3cfa6b51-06f7-4432-a0c5-d133d30d8ede .cell .code}
``` python
git cherry-pick abc1234
```
:::

::: {#a6d285fc-2088-4828-a256-bd412aae8987 .cell .markdown}
## 26. When Is `cherry-pick` Useful? {#26-when-is-cherry-pick-useful}

Cherry-pick is useful when:

-   one branch contains a bug fix you need elsewhere
-   you want a single commit, not the whole branch history
-   you are backporting a fix to a release branch
-   you want to reuse a precise change selectively

### Common Professional Scenario

A bug is fixed on `main`, and the same fix is needed on a maintenance
branch:

``` bash
git switch release/1.2
git cherry-pick abc1234
```
:::

::: {#7916bbaa-6eb5-4713-b2c1-0861ad839bcc .cell .code}
``` python
git switch release/1.2
git cherry-pick abc1234
```
:::

::: {#d15a8ab6-c13f-4559-991d-be0bd8b62cfa .cell .markdown}
## 27. Cherry-Pick Can Also Conflict {#27-cherry-pick-can-also-conflict}

Cherry-pick is not magic.\
If the target branch differs too much, the commit may not apply cleanly.

Then Git may stop with conflicts, and you resolve them similarly to
merge or rebase conflicts:

``` bash
git status
git add .
git cherry-pick --continue
```

Or, if needed:

``` bash
git cherry-pick --abort
```
:::

::: {#651d89a6-7a5a-4acc-8b89-fe1a8abc483a .cell .code}
``` python
git status
git add .
git cherry-pick --continue
git cherry-pick --abort
```
:::

::: {#468bca49-638c-4f1c-8672-af45fabfb474 .cell .markdown}
## 28. Choosing the Right Tool {#28-choosing-the-right-tool}

A professional Git user learns to choose based on intent.

### Use `reset` when:

-   you want to rewrite recent local history
-   you want to undo local commits before sharing
-   you want to unstage or discard local state

### Use `revert` when:

-   a bad commit is already shared
-   you want a safe undo with history preserved

### Use `restore` when:

-   you want to restore one file
-   you want to unstage a file
-   you want file-level correction without branch history surgery

### Use `amend` when:

-   you only need to fix the last commit

### Use `cherry-pick` when:

-   you want one specific commit on another branch

## 29. Common Mistakes {#29-common-mistakes}

### 1. Using `reset --hard` too casually {#1-using-reset---hard-too-casually}

This can destroy local work.

### 2. Using `reset` instead of `revert` on shared branches {#2-using-reset-instead-of-revert-on-shared-branches}

This can make collaboration messy.

### 3. Forgetting that `amend` rewrites history {#3-forgetting-that-amend-rewrites-history}

Even a message-only change creates a new commit hash.

### 4. Cherry-picking large sequences without thinking {#4-cherry-picking-large-sequences-without-thinking}

This can create duplicated or confusing history if done carelessly.

### 5. Using history-rewriting commands after public sharing without coordination {#5-using-history-rewriting-commands-after-public-sharing-without-coordination}

This is one of the most common professional Git mistakes.

## 30. Practical Scenario 1: Undo the Last Local Commit, Keep the Changes {#30-practical-scenario-1-undo-the-last-local-commit-keep-the-changes}

You committed too early, but want to keep working.

A good option is:

``` bash
git reset --mixed HEAD~1
```

Now:

-   the commit is removed from visible branch history
-   the file changes remain in your working tree
-   nothing is staged

You can now stage files more selectively and recommit properly.
:::

::: {#03f1ad28-1737-4931-b1f0-dca57160abba .cell .code}
``` python
git reset --mixed HEAD~1
```
:::

::: {#641a7532-dc9a-49e7-89a7-d8b1923f25e5 .cell .markdown}
## 31. Practical Scenario 2: Undo a Shared Commit Safely {#31-practical-scenario-2-undo-a-shared-commit-safely}

A bad commit is already on `main` and must be undone.

A safer option is:

``` bash
git revert abc1234
```

This creates a new commit that reverses the earlier one.

This is the professional-safe pattern for shared history.
:::

::: {#eb8f77c6-36ef-47dd-a569-9fc594abb2c7 .cell .code}
``` python
git revert abc1234
```
:::

::: {#55b36ac5-dbaa-4ee2-97cd-2d1e70f964eb .cell .markdown}
## 32. Practical Scenario 3: Remove a File from Staging {#32-practical-scenario-3-remove-a-file-from-staging}

You accidentally staged `config.local.json`, but you do not want it in
the next commit.

Use:

``` bash
git restore --staged config.local.json
```

Now:

-   the file is unstaged
-   your local edits remain
:::

::: {#6657179e-aef3-49c0-b37a-5a359bec41a7 .cell .code}
``` python
git restore --staged config.local.json
```
:::

::: {#18ab30f3-6c3b-42c6-a2bc-6710d8f99e93 .cell .markdown}
## 33. Practical Scenario 4: Fix the Last Commit {#33-practical-scenario-4-fix-the-last-commit}

You committed, then realized the message is weak or one file is missing.

A common correction is:

``` bash
git add forgotten_file.py
git commit --amend -m "Add validation and tests for empty email input"
```

This replaces the previous final commit with a better version.
:::

::: {#ee488f67-5f7c-4dc0-b605-9099aa3bcafa .cell .code}
``` python
git add forgotten_file.py
git commit --amend -m "Add validation and tests for empty email input"
```
:::

::: {#aa0b7bf6-53d2-47ae-a544-c53eac8b4560 .cell .markdown}
## 34. Practical Scenario 5: Backport a Fix to Another Branch {#34-practical-scenario-5-backport-a-fix-to-another-branch}

Suppose `main` has a bug fix, but you also need it on `release/1.2`.

Workflow:

``` bash
git switch release/1.2
git cherry-pick abc1234
```

This applies only that selected fix to the release branch, without
merging unrelated work from `main`.
:::

::: {#4c1aaff6-e9c3-49a1-b3ea-23d8c7f32df4 .cell .code}
``` python
git switch release/1.2
git cherry-pick abc1234
```
:::

::: {#4bb32931-b4b0-49e1-9114-c478f9c1baed .cell .markdown}
## 35. A Useful Safety Principle {#35-a-useful-safety-principle}

Before running an undo-related command, ask:

### Question 1

Do I want to **rewrite history**, or do I want to **preserve history**?

-   rewrite history → maybe `reset` or `amend`
-   preserve history → maybe `revert`

### Question 2

Am I changing a **file state**, or am I changing **commit history**?

-   file state → maybe `restore`
-   commit history → maybe `reset`, `revert`, `amend`, or `cherry-pick`

### Question 3

Has this already been shared with others?

-   not shared → more freedom
-   shared → prefer safer history-preserving choices

## 36. Quick Comparison Table {#36-quick-comparison-table}

### `reset` {#reset}

-   moves branch pointer
-   can affect staging and working tree
-   good for local cleanup

### `revert` {#revert}

-   adds a new inverse commit
-   good for undoing shared commits safely

### `restore`

-   restores files or unstages them
-   good for file-level corrections

### `amend`

-   replaces the most recent commit
-   good for fixing the latest commit

### `cherry-pick`

-   applies one specific commit onto current branch
-   good for selective reuse across branches

## 37. Quick Reference Commands {#37-quick-reference-commands}

### Undo last local commit, keep changes staged

``` bash
git reset --soft HEAD~1
```

### Undo last local commit, keep changes unstaged

``` bash
git reset --mixed HEAD~1
```

### Undo last local commit and discard changes

``` bash
git reset --hard HEAD~1
```

### Safely undo a shared commit

``` bash
git revert <commit>
```

### Discard local changes in one file

``` bash
git restore path/to/file
```

### Unstage one file

``` bash
git restore --staged path/to/file
```

### Fix the latest commit

``` bash
git commit --amend
```

### Move one commit to another branch

``` bash
git cherry-pick <commit>
```

## 38. Final Summary {#38-final-summary}

These five commands solve different classes of Git problems.

### `reset` {#reset-1}

Use for local history cleanup and pointer movement.

### `revert` {#revert-1}

Use for safe undo in shared history.

### `restore`

Use for file-level restoration or unstaging.

### `amend`

Use to improve or correct the last commit.

### `cherry-pick`

Use to apply one specific commit somewhere else.

The deeper lesson is this:

> Good Git usage is not just knowing commands. It is knowing which layer
> you intend to change: file state, staging state, branch history, or
> shared history.

## 39. Suggested Practice Exercises {#39-suggested-practice-exercises}

1.  make two commits in a test repository
2.  use `git reset --soft HEAD~1` and observe the staged state
3.  repeat with `git reset --mixed HEAD~1`
4.  create a new commit and undo it with `git revert`
5.  modify a file and discard changes using `git restore`
6.  stage a file and unstage it using `git restore --staged`
7.  make a commit with a bad message and fix it using
    `git commit --amend`
8.  create two branches and use `git cherry-pick` to move one fix from
    one branch to another
9.  compare histories using:

``` bash
git log --oneline --graph --all
```

These exercises build intuition much faster than memorizing definitions.
:::

::: {#bfe57d3f-3b39-45ae-852b-5373cf3d9cbe .cell .code}
``` python
git log --oneline --graph --all
```
:::

::: {#d0214bd7-f33f-4b15-8791-ce29b141029d .cell .markdown}
# GitHub Notes

## Forking a Repository

-   We should **fork** a repository of interest.
-   Forking allows us to:
    -   Create our own copy of the repository
    -   Modify its content and code independently
    -   Experiment without affecting the original project

------------------------------------------------------------------------

## Using Git Locally vs GitHub

-   Git can be used **locally** on your system:
    -   Useful when working with **sensitive datasets**
    -   No need to upload data online
-   When using GitHub:
    -   Use a `.gitignore` file to exclude sensitive or unnecessary
        files
    -   Helps keep the repository clean and secure

------------------------------------------------------------------------

## Pull Command

-   The **pull** command means:
    -   Bringing changes from a **remote repository** into your local
        branch
    -   Or updating your branch with changes from another branch

\`\`\`bash git pull
:::

::: {#25cd447a-2014-42fc-baab-22bf9b684a2c .cell .markdown}
# Git Supplement Notebook: Finding the Right Commit and Debugging Regressions

This section is a **supplement** to the linked Git section.\
It focuses on the practical points we discussed **that are not
explicitly covered inside the reset/revert/restore/amend/cherry-pick
section itself**:

1.  How to find the commit ID (hash) you need
2.  How to refer to commits without memorizing full hashes
3.  How to identify the exact commit where code stopped working
4.  How to use `git bisect` to find the first bad commit efficiently

Use this notebook as a hands-on companion after reading the main Git
guide.

## 1) How to get the commit number (commit hash) {#1-how-to-get-the-commit-number-commit-hash}

Many Git commands need a target commit, for example:

-   `git revert <commit>`
-   `git reset --hard <commit>`
-   `git cherry-pick <commit>`

In Git, the "commit number" is usually called the **commit hash** or
**commit ID**.

### Fast way to list commits

``` bash
git log --oneline
```

Example output:

``` text
a1b2c3d add login validation
f6g7h8i fix navbar layout
91ab234 initial API integration
```

Here:

-   `a1b2c3d`
-   `f6g7h8i`
-   `91ab234`

are short commit hashes.

You can usually use the short form as long as it is unique in your
repository.

## 2) Which log view should you use? {#2-which-log-view-should-you-use}

### Basic history

``` bash
git log
```

Shows:

-   full commit hash
-   author
-   date
-   message

### Short history

``` bash
git log --oneline
```

Best when you just want the commit IDs quickly.

### Visual branch history

``` bash
git log --oneline --graph --all
```

Very useful when branches or merges are involved.

Example mental picture:

``` text
* 8f4c2ab fix test on release branch
| * c7d9120 add login retry
| * 13bc991 refactor login flow
|/
* 91ab234 initial API integration
```

This helps you answer:

-   Which commit is newest?
-   Which branch introduced the change?
-   Which commit should I revert or cherry-pick?

## 3) You do not always need the full commit hash {#3-you-do-not-always-need-the-full-commit-hash}

Instead of typing a full hash, Git also lets you refer to recent commits
relative to `HEAD`.

`HEAD` means: **the commit I am currently on**.

### Common relative references

-   `HEAD` → current commit
-   `HEAD~1` → one commit before current
-   `HEAD~2` → two commits before current
-   `HEAD~3` → three commits before current

Examples:

``` bash
git reset --soft HEAD~1
git reset --hard HEAD~2
git show HEAD~3
```

This is useful when the bad change is recent and you just want to move
back a few commits.

## 4) When code worked before but is broken now {#4-when-code-worked-before-but-is-broken-now}

This is a classic debugging situation:

-   an older commit worked
-   the current commit is broken
-   you want to know **exactly which commit introduced the problem**

There are two approaches:

### A. Manual checking {#a-manual-checking}

You move between commits and test the code yourself.

Example:

``` bash
git checkout <commit>
```

Then run your program or tests.

If that commit is good, move forward.\
If it is bad, move backward.

This works, but it can be slow in repositories with many commits.

### B. `git bisect` (recommended) {#b-git-bisect-recommended}

This is the professional way when you know:

-   one commit that is definitely **good**
-   one commit that is definitely **bad**

Git then performs a binary search through history to find the first bad
commit much faster.

## 5) `git bisect` step by step {#5-git-bisect-step-by-step}

### Step 1: Start bisect

``` bash
git bisect start
```

### Step 2: Mark the current state as bad

``` bash
git bisect bad
```

This usually means: "the version I am on right now is broken."

### Step 3: Mark an older known-good commit

``` bash
git bisect good <commit-id>
```

Now Git chooses a commit in the middle.

### Step 4: Test that commit

Run your program or your tests.

-   If it works:

    ``` bash
    git bisect good
    ```

-   If it fails:

    ``` bash
    git bisect bad
    ```

Git keeps narrowing the search until it prints something like:

``` text
<commit-id> is the first bad commit
```

## 6) Real example of `git bisect` {#6-real-example-of-git-bisect}

Suppose your history is:

``` text
A - B - C - D - E - F
```

You know:

-   `B` was still good
-   `F` is bad

You run:

``` bash
git bisect start
git bisect bad
git bisect good B
```

Git may test `D` first.

-   If `D` is good, then the bug must be in `E` or `F`
-   If `D` is bad, then the bug must be in `C` or `D`

So instead of checking every commit one by one, Git cuts the search
space in half each time.

## 7) If you already have automated tests, bisect becomes much stronger {#7-if-you-already-have-automated-tests-bisect-becomes-much-stronger}

If your project has a command that returns success/failure, Git can
automate the search.

Examples:

``` bash
git bisect run pytest
git bisect run npm test
git bisect run python -m unittest
```

In this mode:

-   test passes → Git treats commit as good
-   test fails → Git treats commit as bad

This is one of the best tools for tracking down regressions.

## 8) How this connects to reset, revert, restore, amend, and cherry-pick {#8-how-this-connects-to-reset-revert-restore-amend-and-cherry-pick}

After you find the problematic commit, you can decide what to do:

### If the bad commit was already shared

Use:

``` bash
git revert <bad-commit>
```

### If the bad commit is only local and you want to rewrite history

Use one of:

``` bash
git reset --soft <target>
git reset --mixed <target>
git reset --hard <target>
```

### If the problem is just one file

Use:

``` bash
git restore path/to/file
```

### If the issue is only in the latest commit message or contents

Use:

``` bash
git commit --amend
```

### If you want to bring a good fix from another branch

Use:

``` bash
git cherry-pick <good-commit>
```

So the missing skill is often not the command itself, but **finding the
correct commit first**.

## 9) Practical workflow you can follow in real projects {#9-practical-workflow-you-can-follow-in-real-projects}

When you suspect a regression:

1.  Inspect history

    ``` bash
    git log --oneline --graph --all
    ```

2.  Identify:

    -   one known good commit
    -   one known bad commit

3.  Use `git bisect`

    ``` bash
    git bisect start
    git bisect bad
    git bisect good <known-good-commit>
    ```

4.  Test each checked-out revision

5.  Finish with:

    ``` bash
    git bisect reset
    ```

6.  Then choose the appropriate fix:

    -   `revert`
    -   `reset`
    -   `restore`
    -   `amend`
    -   `cherry-pick`

## 10) Quick command summary {#10-quick-command-summary}

### Find commit IDs

``` bash
git log --oneline
git log --oneline --graph --all
```

### Refer to recent commits

``` bash
HEAD
HEAD~1
HEAD~2
HEAD~3
```

### Move to a specific commit temporarily

``` bash
git checkout <commit-id>
```

### Find the first bad commit

``` bash
git bisect start
git bisect bad
git bisect good <commit-id>
git bisect good
git bisect bad
git bisect reset
```

### Automated bisect

``` bash
git bisect run pytest
```

## 11) Final takeaway {#11-final-takeaway}

The linked section explains **what** `reset`, `revert`, `restore`,
`amend`, and `cherry-pick` do.

This supplement adds the missing practical layer we discussed:

-   how to **find the commit hash**
-   how to **target recent commits using `HEAD~n`**
-   how to **detect where the code started breaking**
-   how to **use `git bisect` to find the first bad commit efficiently**

That combination is what makes these Git commands truly usable in real
debugging work.
:::

::: {#cad3ed93-71d2-4327-82c5-ddbf5fc2d90b .cell .markdown}
# 📓 Git Workflow: Connect Local Folder to GitHub Repository {#-git-workflow-connect-local-folder-to-github-repository}

🧠 Goal We will:

Create a repository on GitHub Create a local folder Initialize Git
Connect to GitHub Push code

## 💻 Step 1: Create Local Folder {#-step-1-create-local-folder}

mkdir my-project cd my-project

## ⚙️ Step 2: Initialize Git {#️-step-2-initialize-git}

git init

## 📄 Step 3: Add Files {#-step-3-add-files}

touch README.md

## ➕ Step 4: Stage Files {#-step-4-stage-files}

git add .

## 📝 Step 5: Commit {#-step-5-commit}

git commit -m \"Initial commit\"

## 🔗 Step 6: Connect to GitHub {#-step-6-connect-to-github}

git remote add origin <https://github.com/username/my-project.git>

## 🔍 Step 7: Check Remote {#-step-7-check-remote}

git remote -v

the result should be in the form of

origin <git@github.com>:username/myproject.git (fetch) origin
<git@github.com>:username/myproject.git (push)

## 🚀 Step 8: Push {#-step-8-push}

git branch -M main git push -u origin main
:::

::: {#a1d52be5-ff85-4558-8fbc-a491bc214265 .cell .markdown}
# Git Archaeology Example Notebook

This notebook explains a practical **Git archaeology** workflow using
the `networkx` repository.

## Goal {#goal}

We want to:

1.  Switch to a specific historical release branch/context
2.  Search for a specific text in the repository
3.  Find the file and line where it appears
4.  Identify the commit related to that line
5.  Inspect the commit
6.  Create a branch from that historical commit

This is useful when you want to understand:

-   where a bug message came from
-   which commit introduced or changed something
-   how the code looked at that point in time

## Step 1: Create a working branch from networkx-2.6.3 {#step-1-create-a-working-branch-from-networkx-263}

We start by creating a new branch called exercise from networkx-2.6.3.

This gives us a safe branch to work on without changing the original
branch.

git switch \--create exercise networkx-2.6.3

## Step 2: Search for the text in the repository

Now we search for the phrase:

`Logic error in degree_correlation`

This tells us **which file(s)** contain that text.

git grep \"Logic error in degree_correlation\"

git grep -n \"Logic error in degree_correlation\"

## Step 4: Annotate the file

After identifying the file, we run `git annotate` on it:

`networkx/algorithms/threshold.py`

This shows, line by line:

-   the commit hash
-   the author
-   the date
-   the content of the line

This helps us discover **which commit last changed the line** containing
the text.

git annotate networkx/algorithms/threshold.py

## Step 5: Search inside the annotate output

Inside the `git annotate` viewer, search for the phrase:

`Logic error`

You typically type:

``` text
/Logic error
```

This is not a shell command. It is a search command used inside the
pager (`less`) opened by Git.

/Logic error

## Step 6: Inspect the relevant commit

From the annotation output, we find the commit hash:

`90544b4fa`

Now we inspect that commit with `git show`.

This displays:

-   commit metadata
-   author and date
-   commit message
-   the diff (what changed)

git show 90544b4fa

## Step 7: Create a branch from that historical commit

Now we create a new branch called `past-code` from commit `90544b4fa`.

This lets us explore the code exactly as it existed at that point in
history.

git branch past-code 90544b4fa

## What this example demonstrates

This example solves the following investigation workflow:

-   Find a text in the repository
-   Locate the exact file and line
-   Identify the commit responsible for that line
-   Inspect the commit
-   Create a branch from that point in history

In short:

``` bash
git grep -> find the text
git annotate -> find the responsible commit
git show -> inspect the commit
git branch -> create a branch from that commit
```

This is a classic Git archaeology workflow.

## Command Summary

``` bash
git switch --create exercise networkx-2.6.3
git grep "Logic error in degree_correlation"
git grep -n "Logic error in degree_correlation"
git annotate networkx/algorithms/threshold.py
/Logic error
git show 90544b4fa
git branch past-code 90544b4fa
```
:::

::: {#e5123f6f-2524-4186-9612-eddf9bf68518 .cell .code}
``` python
```
:::
