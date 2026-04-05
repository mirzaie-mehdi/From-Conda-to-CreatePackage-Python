


# Git and GitHub 3

This section discuss about:

-   What is Git?
-   Creating a GitHub account
-   Initializing Git repositories
-   Core Git commands
-   Configuring Git
-   Using SSH with GitHub
-   Generating SSH keys
-   Testing SSH
-   Understanding how public/private keys work
-   When to run SSH tests


::: {#d516a195-cf06-4dc5-b932-93b4cac7c997 .cell .markdown}
# What is Git?

**Git** is a version control system. It helps you:

-   track changes in your files
-   save project history
-   collaborate with others
-   restore older versions when needed
-   make new branches of your project and work there without affecting
    the main files of project.

Git runs on your own computer. GitHub is an online platform that hosts
Git repositories. In order to turn a floder into a Git repository, we do
as follows:
:::

::: {#94effc22-376c-4468-9db8-de90c5d5d286 .cell .markdown}
## Initialize a Repository

To turn a folder into a Git repository, go to the folder path and type
the following command in the terminal:

``` bash
git init
```

If you want the default branch to be `main` immediately, use:

``` bash
git init -b main
```

### Meaning of `git init -b main`

This command:

1.  creates a new Git repository
2.  creates the initial branch with the name `main`

This is cleaner than using `git init` first and renaming the branch
later.
:::

::: {#4d8836a8-5052-4d01-b985-6600a51e2a2b .cell .markdown}
## 4. Stage and Commit Changes {#4-stage-and-commit-changes}

A common Git workflow looks like this:

``` bash
git add .
git commit -m "your message"
```

### Explanation

-   `git add .` stages the changes
-   `git commit -m "..."` records those staged changes into Git history

### Example

``` bash
git add app.js
git commit -m "Fix login bug"
```

This means the changes in `app.js` are now stored as a commit with the
message `Fix login bug`.
:::

::: {#05213d49-2650-4247-9531-e359abfa0862 .cell .markdown}
## 5. View Commit History with `git log` {#5-view-commit-history-with-git-log}

To see commit history, use:

``` bash
git log
```

### What it shows

For each commit, Git usually shows:

-   commit hash
-   author
-   date
-   commit message

### Useful variations

#### One-line history

``` bash
git log --oneline
```

#### Graph view

``` bash
git log --oneline --graph
```

#### Last 5 commits

``` bash
git log -n 5
```

#### Show full patch with each commit

``` bash
git log -p
```

#### Show history for one file

``` bash
git log file.txt
```
:::

::: {#08ac4b52-2419-41da-8a28-37110bddffc8 .cell .markdown}
## 6. Compare Changes with `git diff` {#6-compare-changes-with-git-diff}

To compare changes, use:

``` bash
git diff
```

### Basic meaning

This shows changes that are **not staged yet**.

### Example {#example}

``` diff
- console.log("Hello");
+ console.log("Hello World");
```

### Common forms

#### Unstaged changes

``` bash
git diff
```

#### Staged changes

``` bash
git diff --staged
```

or

``` bash
git diff --cached
```

#### Compare with the latest commit

``` bash
git diff HEAD
```

#### Compare two commits

``` bash
git diff commit1 commit2
```

#### Compare one file

``` bash
git diff file.txt
```

#### Show only file names

``` bash
git diff --name-only
```
:::

::: {#b33f142f-93f8-4205-932e-824729c7e68e .cell .markdown}
## 7. Configure Git with `git config` {#7-configure-git-with-git-config}

Git uses configuration settings such as your name, email, editor, pager
behavior, and aliases.

### Set your identity

``` bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

These values appear in your commits.

### View all configuration

``` bash
git config --list
```

### View one specific value

``` bash
git config user.name
```

### Set the default editor

``` bash
git config --global core.editor "code --wait"
```

### Enable color output

``` bash
git config --global color.ui auto
```

### Create aliases

``` bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
```
:::

::: {#3922b3c6-2d23-48f6-8369-8892cc301fb2 .cell .markdown}
## 8. Git Configuration Levels {#8-git-configuration-levels}

Git supports three configuration levels:

-   `--system` → applies to the whole system
-   `--global` → applies to the current user
-   `--local` → applies only to the current repository

### Priority

If the same setting exists in multiple places, the order is:

``` text
local > global > system
```

This means local settings override global settings, and global settings
override system settings.
:::

::: {#4527f3be-469a-45c4-89c0-b20d75d99629 .cell .markdown}
## 9. Show Configuration Sources {#9-show-configuration-sources}

To see both the configuration values and where they come from, use:

``` bash
git config --list --show-origin
```

### Example output

``` text
file:/etc/gitconfig        core.editor=vim
file:/home/user/.gitconfig user.name=Ali
file:.git/config           core.repositoryformatversion=0
```

### Why this is useful

This helps you debug configuration problems. For example, if the wrong
email is used in commits, you can find out exactly which file defined
it.
:::

::: {#2160e7ff-abef-460b-8b06-400ba37dc66f .cell .markdown}
## 10. Disabling Git Pagers {#10-disabling-git-pagers}

Git often uses a pager such as `less` to display long output. For
example, `git log` may open inside a scrollable interface.

You can disable the pager for a specific command:

``` bash
git config --global pager.log off
```

This makes `git log` print directly in the terminal.

### Other examples

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

### Effect

This disables pagers for many common Git commands.

### Advantage

-   simpler output
-   useful for scripts
-   easier for some beginners

### Disadvantage

-   very long output can flood the terminal
-   searching inside output becomes harder
-   less convenient for large histories
:::

::: {#30f280e2-8418-4d6d-b505-0e77a98aee95 .cell .markdown}
## 11. A Better Pager Setup {#11-a-better-pager-setup}

Instead of disabling all pagers, many developers prefer a smarter pager
configuration:

``` bash
git config --global core.pager "less -FRX"
```

This keeps the benefits of a pager while making it less intrusive.
:::

::: {#54f5cc10-8b22-4351-aec8-2a636dd31d43 .cell .markdown}
## Create a GitHub Account

To use GitHub effectively, the first step is to create a user account.

### Steps

1.  Go to GitHub.
2.  Click **Sign up**.
3.  Enter:
    -   your email address
    -   a password
    -   a username
4.  Verify your email address.
5.  Complete the sign-up process.

After creating your account, you can create repositories, upload code,
and connect Git on your computer to GitHub.
:::

::: {#d7358f16-6d62-4c76-add4-0ca07bf62ec6 .cell .markdown}
## 12. HTTPS vs SSH for GitHub {#12-https-vs-ssh-for-github}

There are two common ways to connect Git to GitHub:

### HTTPS

``` bash
git clone https://github.com/user/repo.git
```

### SSH

``` bash
git clone git@github.com:user/repo.git
```

### HTTPS

-   simpler to start with
-   commonly available by default
-   often uses a token for authentication

### SSH

-   very common on Linux and macOS
-   excellent for repeated use
-   avoids typing credentials repeatedly
-   useful beyond Git, for server administration as well

SSH is widely considered worth learning.
:::

::: {#6e03b733-3779-4239-b870-02689213e6fd .cell .markdown}
## 13. What Is SSH? {#13-what-is-ssh}

**SSH** stands for **Secure Shell**.

It is a **secure network protocol** used to connect to remote systems
over the internet.

### Important clarification

SSH is **not something separate from the internet**.\
It is a secure way of communicating **over** the internet.

### Why SSH matters

Without encryption, data may be exposed.\
With SSH, communication is encrypted and authenticated.
:::

::: {#94d162f5-812c-4e35-aba5-73720546183a .cell .markdown}
## 14. Generate SSH Keys {#14-generate-ssh-keys}

To use SSH with GitHub, you normally create an SSH key pair on your own
computer.

### Command

``` bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

### What this does

Your operating system creates two files:

-   **private key**
-   **public key**

Usually they are stored in:

``` bash
~/.ssh/
```

For example:

-   `~/.ssh/id_ed25519` → private key
-   `~/.ssh/id_ed25519.pub` → public key
:::

::: {#2546f67a-1185-4e18-9789-cdd9cff09b27 .cell .markdown}
## 15. Which Part Stays Private and Which Part Goes to GitHub? {#15-which-part-stays-private-and-which-part-goes-to-github}

### Private key

-   stays on your computer
-   must never be shared
-   is used to prove your identity

### Public key

-   can be shared
-   is copied to GitHub
-   allows GitHub to recognize your computer

You do **not** copy or upload the private key to GitHub.
:::

::: {#1d012d2b-0f80-48d4-8825-23f721998f04 .cell .markdown}
## 16. How GitHub Verifies That You Have the Correct Private Key {#16-how-github-verifies-that-you-have-the-correct-private-key}

A common question is:

> If GitHub only has my public key, how does it know I have the right
> private key?

The answer is cryptographic authentication.

### Simplified process

1.  GitHub already has your **public key**
2.  During login/authentication, GitHub sends a challenge
3.  Your computer uses the **private key** to sign that challenge
4.  GitHub checks the signature using the **public key**
5.  If the signature is valid, authentication succeeds

### Key point

The **private key never leaves your computer**.\
You do not paste it anywhere.\
Only the cryptographic proof is sent.
:::

::: {#cabe03ac-eafd-487f-b96e-6a93f44e8d20 .cell .markdown}
## 17. Add the Public Key to GitHub {#17-add-the-public-key-to-github}

To display your public key so you can copy it:

``` bash
cat ~/.ssh/id_ed25519.pub
```

Copy the output and add it to your GitHub account under SSH keys.

Only the `.pub` file should be copied to GitHub.
:::

::: {#ebacc085-3c15-4dd4-9462-cec1e6b6cc51 .cell .markdown}
## 18. Test the SSH Connection {#18-test-the-ssh-connection}

After adding the public key to GitHub, test the connection with:

``` bash
ssh -T git@github.com
```

### Meaning of `-T`

`-T` means:

-   do not open an interactive shell
-   only test authentication

### Successful output

``` text
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

This means:

-   SSH is configured correctly
-   GitHub recognizes your key
-   Git operations over SSH should work
:::

::: {#c0665044-39b2-4520-b5eb-552b3cdc66fb .cell .markdown}
## 19. Common SSH Test Errors {#19-common-ssh-test-errors}

### First-time host verification

You may see:

``` text
Are you sure you want to continue connecting (yes/no)?
```

Type:

``` bash
yes
```

This stores GitHub\'s host fingerprint on your machine.

### Permission denied

You may see:

``` text
Permission denied (publickey).
```

This usually means one of the following:

-   the public key was not added to GitHub
-   the wrong key is being used
-   the key was not loaded by the SSH agent
-   the file path is wrong
:::

::: {#4e71d8b3-054f-4fe5-87e6-b043b619f2c5 .cell .markdown}
## 20. Check Whether Your SSH Key Exists {#20-check-whether-your-ssh-key-exists}

To see files in your SSH directory:

``` bash
ls ~/.ssh
```

You should usually see files like:

``` text
id_ed25519
id_ed25519.pub
```
:::

::: {#83f758b8-cc72-4479-8d6e-3944d28f27dc .cell .markdown}
## 21. Check Whether the Key Is Loaded {#21-check-whether-the-key-is-loaded}

To list keys currently loaded in the SSH agent:

``` bash
ssh-add -l
```

If your key is not loaded, add it:

``` bash
ssh-add ~/.ssh/id_ed25519
```
:::

::: {#5db2a575-ed9c-4d86-9297-bba43016347e .cell .markdown}
## 22. Do You Need to Run `ssh -T git@github.com` Every Time? {#22-do-you-need-to-run-ssh--t-gitgithubcom-every-time}

No.

You usually run:

``` bash
ssh -T git@github.com
```

only for:

-   initial setup
-   troubleshooting
-   verifying that authentication works

### In normal daily use

You do **not** run the SSH test every time.

Instead, you just use Git normally:

``` bash
git clone git@github.com:user/repo.git
git pull
git push
```

SSH authentication happens automatically in the background.
:::

::: {#1a14036e-e7dc-4d47-aa11-c27f7f2512c7 .cell .markdown}
## 23. Daily Workflow Summary {#23-daily-workflow-summary}

A typical workflow after setup looks like this:

1.  create or enter your project folder
2.  initialize Git
3.  make changes
4.  stage changes
5.  commit
6.  push to GitHub using SSH

### Example {#example}

``` bash
git init -b main
git add .
git commit -m "Initial commit"
git remote add origin git@github.com:username/repository.git
git push -u origin main
```
:::

::: {#27650d02-57fd-400b-ab67-4dbe5bfd5408 .cell .markdown}
## 24. Recommended Mental Model {#24-recommended-mental-model}

You can think of the SSH process like this:

-   your computer keeps the **private key**
-   GitHub stores the **public key**
-   when needed, your computer proves ownership of the private key
-   GitHub verifies the proof with the public key

This is why SSH is secure and convenient.
:::

::: {#2a1b079a-c863-48b2-9bdf-50a6507f66b6 .cell .markdown}
## 25. Final Notes {#25-final-notes}

### Good security habits

-   never share the private key
-   only upload the public key
-   protect the private key with a passphrase if possible

### Why learning SSH is valuable

SSH is useful not only for GitHub, but also for:

-   remote server access
-   cloud systems
-   DevOps workflows
-   secure file transfer
:::

::: {#553a4665-f946-4451-aced-24607cc37c49 .cell .markdown}
## 26. Command Reference {#26-command-reference}

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

### Connecting an Existing Local Git Repository to GitHub

#### 📌 Goal {#-goal}

You already have a local Git repository and want to:

1.  Create a repository on GitHub
2.  Link (connect) it to your local repo
3.  Push your code to GitHub

------------------------------------------------------------------------

### 🧱 Step 1 --- Create a Repository on GitHub {#-step-1--create-a-repository-on-github}

1.  Go to GitHub
2.  Click **New repository**
3.  Choose a name (e.g., `my-project`)
4.  **Do NOT** initialize with README, `.gitignore`, or license
5.  Click **Create repository**

------------------------------------------------------------------------

### 🔗 Step 2 --- Add Remote to Your Local Repository {#-step-2--add-remote-to-your-local-repository}

In your terminal (inside your project folder):

``` bash
git remote add origin git@github.com:USERNAME/REPOSITORY.git
```

Example:

``` bash
git remote add origin git@github.com:john/my-project.git
```

✔ This connects your local repo → GitHub repo

------------------------------------------------------------------------

### 🔍 Step 3 --- Verify Remote Connection {#-step-3--verify-remote-connection}

``` bash
git remote -v
```

Expected output:

``` bash
origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
origin  git@github.com:USERNAME/REPOSITORY.git (push)
```

------------------------------------------------------------------------

### 🚀 Step 4 --- Push Your Code to GitHub {#-step-4--push-your-code-to-github}

If your main branch is `main`:

``` bash
git push -u origin main
```

If it\'s `master`:

``` bash
git push -u origin master
```

✔ `-u` sets upstream so future pushes are simpler:

``` bash
git push
```

------------------------------------------------------------------------

### 🧠 What just happened {#-what-just-happened}

-   You created a remote repository on GitHub
-   You linked it using `origin`
-   You uploaded your local commits to the remote

------------------------------------------------------------------------

### ⚠️ Common Issues {#️-common-issues}

-   ❌ **Permission denied (publickey)** → SSH key not set up

-   ❌ **Repository not found** → Wrong URL or repo name

-   ❌ **Branch mismatch** → Use correct branch (`main` vs `master`)

------------------------------------------------------------------------

### ✅ Final State {#-final-state}

Your workflow now becomes:

``` bash
git add .
git commit -m "your message"
git push
```

And to get updates:

``` bash
git pull
```

------------------------------------------------------------------------
:::

::: {#3d712cfc-cd3a-40bc-9c86-679ebdafbcc4 .cell .markdown}
# Git Forks and Pull Requests

This section is about **Forks**, **Branches**, and **Pull Requests
(PRs)** in Git and GitHub.

It is designed for learners who already know the basics of Git and now
want to understand the standard contribution workflow used in
open-source and collaborative development.

------------------------------------------------------------------------

## Learning Goals

By the end of this section, you should be able to:

-   explain what a **fork** is
-   distinguish **fork** from **clone**
-   understand when to use a **fork-based workflow**
-   use `git branch` and `git switch` correctly in this workflow
-   create and push a feature branch from your fork
-   open a **Pull Request**
-   respond to review comments and update the same PR
-   keep your fork synchronized with the original repository
-   understand what makes someone a **contributor**

## 1. What Is a Fork? {#1-what-is-a-fork}

A **fork** is a copy of a GitHub repository that is created under **your
own GitHub account**. This is different from a local copy on your
computer. A fork lives on **GitHub**, not just on your machine.

### Mental Model

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

## 2. When Do We Use a Fork? {#2-when-do-we-use-a-fork}

Forks are commonly used when you **do not have direct write access** to
the original repository.

### Typical Cases

1.  **Open-source contribution**\
    You want to improve a public project, but you are not part of the
    core team.

2.  **Safe experimentation**\
    You want to try changes freely without affecting the original
    repository.

3.  **Personal customization**\
    You want to maintain your own version of a project with custom
    changes.

4.  **External collaboration**\
    You are contributing from outside the main organization or team.

### When a Fork Is Usually Not Needed

If you are already a member of the project team and have write access,
the team may prefer this workflow instead:

-   clone the main repository directly
-   create a branch inside that repository
-   push your branch
-   open a Pull Request from that branch

In that case, no fork is necessary.

## 3. Fork vs Clone {#3-fork-vs-clone}

These two concepts are related, but they are not the same.

### Fork

A **fork** creates a copy of a repository on **GitHub** under your
account.

### Clone

A **clone** creates a copy of a repository on **your local computer**.

### Standard Order

In a fork-based contribution workflow, the normal order is:

``` text
Fork on GitHub → Clone to your computer
```

So first you create the fork online, and then you clone **your fork**
locally.

## 4. Do We Get the Full History After Forking? {#4-do-we-get-the-full-history-after-forking}

Yes. In normal GitHub usage, a fork preserves the repository history.

That means your fork contains:

-   commits
-   branches and references relevant to the forked project
-   tags (depending on repository state and hosting behavior)
-   the complete evolution of the project

So conceptually, you inherit the project history.

However, **having the history does not automatically make you a
contributor** to the original project.

## 5. Are You a Contributor Just Because You Forked? {#5-are-you-a-contributor-just-because-you-forked}

No.

Forking a repository means:

-   you now have your own copy of the project
-   you can work on it independently
-   you can propose changes

But it does **not** mean that you are already a contributor to the
original repository.

### When Are You Usually Considered a Contributor?

In practice, you become a contributor when your changes are accepted
into the original project, usually by:

-   opening a Pull Request
-   having it reviewed
-   and getting it merged

So this statement is accurate:

> You may have the full repository history in your fork, but you are not
> yet a contributor to the original project until your contribution is
> accepted there.

## 6. The Full Fork Workflow {#6-the-full-fork-workflow}

Here is the standard end-to-end workflow:

``` text
Fork → Clone → Add upstream → Create branch → Edit → Commit → Push → Pull Request → Review → Merge
```

We will now go through each part carefully.

## 7. Step 1: Fork the Repository on GitHub {#7-step-1-fork-the-repository-on-github}

On GitHub:

1.  open the original repository
2.  click **Fork**
3.  GitHub creates a copy under your own account

Example:

-   original: `github.com/original-owner/project`
-   fork: `github.com/your-username/project`

At this point, you have your own GitHub-hosted copy of the project.

## 8. Step 2: Clone Your Fork Locally {#8-step-2-clone-your-fork-locally}

After forking, clone **your fork**, not the upstream repository.

``` bash
git clone git@github.com:YOUR_USERNAME/PROJECT.git
cd PROJECT
```

Now your local repository is connected to your fork as `origin`.

## 9. Step 3: Add the Original Repository as `upstream` {#9-step-3-add-the-original-repository-as-upstream}

This is a very important step.

You usually want two remotes:

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

Expected idea:

``` text
origin    git@github.com:YOUR_USERNAME/PROJECT.git
upstream  git@github.com:ORIGINAL_OWNER/PROJECT.git
```

### Why Is `upstream` Important?

Because the original project keeps moving forward.

If you only work with your fork and never sync from upstream:

-   your fork becomes outdated
-   your branch may drift away from the current project state
-   your Pull Request may become harder to review or merge
:::

::: {#eff35328-b6cc-4237-9417-60cb46baeb8b .cell .code}
``` python
git clone git@github.com:YOUR_USERNAME/PROJECT.git
cd PROJECT

git remote add upstream git@github.com:ORIGINAL_OWNER/PROJECT.git
git remote -v
```
:::

::: {#9ffe7e58-85c3-43e2-998d-7e07d949cebc .cell .markdown}
## 10. Step 4: Create a New Branch for Your Change {#10-step-4-create-a-new-branch-for-your-change}

You should **not** usually work directly on `main` for a contribution.

Instead, create a branch for each separate change.

### Why?

Because branches help you:

-   isolate your work
-   keep `main` clean
-   open focused Pull Requests
-   revise one change without mixing it with another

### Recommended Command

``` bash
git switch -c fix-readme-typo
```

This command does two things at once:

1.  creates a new branch named `fix-readme-typo`
2.  switches to that branch immediately

## 11. Where Do `git branch` and `git switch` Fit In? {#11-where-do-git-branch-and-git-switch-fit-in}

These two commands are very important in the fork workflow.

### `git branch`

Used to:

-   list branches
-   create branches
-   delete branches

Examples:

``` bash
git branch
git branch fix-readme-typo
```

If you run:

``` bash
git branch fix-readme-typo
```

it creates the branch, but does **not** move you into it.

### `git switch`

Used to move between branches.

Examples:

``` bash
git switch main
git switch fix-readme-typo
```

### Best Combined Form

``` bash
git switch -c fix-readme-typo
```

This is usually the cleanest modern command for contribution work.

### Comparison with Older Syntax

Older style:

``` bash
git checkout -b fix-readme-typo
```

Modern clearer style:

``` bash
git switch -c fix-readme-typo
```
:::

::: {#03e166d4-2b99-4a1b-85a3-71c86c7856b5 .cell .code}
``` python
git branch
git switch -c fix-readme-typo
git branch
```
:::

::: {#76aa5817-dcff-473e-a7e2-c9a6ca2fbba5 .cell .markdown}
## 12. Step 5: Make Your Changes {#12-step-5-make-your-changes}

Now edit the project files.

Examples:

-   fix a typo
-   improve documentation
-   add a small feature
-   refactor a function
-   update tests

At this point, your changes are only on your local branch.

## 13. Step 6: Stage and Commit the Changes {#13-step-6-stage-and-commit-the-changes}

After editing, stage and commit your work.

``` bash
git add .
git commit -m "Fix typo in installation guide"
```

### Good Commit Message Advice

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
:::

::: {#5156d28a-c9c7-4638-8d61-25de0d83cd8f .cell .code}
``` python
git add .
git commit -m "Fix typo in installation guide"
```
:::

::: {#e7cb9ebb-3211-4cc8-9c25-20d83c28c854 .cell .markdown}
## 14. Step 7: Push the Branch to Your Fork {#14-step-7-push-the-branch-to-your-fork}

Now push your branch to `origin`, which is your fork.

``` bash
git push origin fix-readme-typo
```

This sends your branch to GitHub under **your fork**.
:::

::: {#f377c8dd-82fc-4298-9cc3-8fb4bd055f1d .cell .code}
``` python
git push origin fix-readme-typo
```
:::

::: {#944f7313-d6b3-44ae-805c-e62cdbe41cc2 .cell .markdown}
## 15. Step 8: Open a Pull Request {#15-step-8-open-a-pull-request}

This is the key step.

A **Pull Request (PR)** is a request asking the maintainers of the
original project to review and potentially merge your changes.

### Meaning of a Pull Request

A Pull Request is basically saying:

> I made these changes in my branch. Please review them and merge them
> into the main project if they are acceptable.

### Typical Direction

``` text
your fork / your branch  →  upstream / main
```

Example:

-   source: `your-username:fix-readme-typo`
-   target: `original-owner:main`
:::

::: {#db81306b-af06-4da6-8d3c-ec20c1bd7592 .cell .markdown}
## 16. How to Create a Pull Request on GitHub {#16-how-to-create-a-pull-request-on-github}

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

### Good PR Title

-   `Fix typo in installation guide`
-   `Add missing null check in login flow`

### Good PR Description Should Explain

-   what changed
-   why it changed
-   any special context reviewers should know
-   how to test it, if relevant
:::

::: {#1b85457d-875e-494c-b1e5-7fb3945db2dd .cell .markdown}
## 17. Professional Pull Request Writing {#17-professional-pull-request-writing}

A strong Pull Request is not just code. It is also communication.

### A Good PR Usually Includes

-   **Problem**: What issue are you solving?
-   **Change**: What did you modify?
-   **Reasoning**: Why is this the right change?
-   **Testing**: How did you verify it?
-   **Scope**: Is this a small focused change or a broader change?

### Example PR Description

``` text
## Summary
This PR fixes a typo in the installation section of the README.

## Details
The command name was missing a dash, which could confuse new users.

## Testing
No code changes were made. Documentation reviewed manually.
```

This helps maintainers review quickly and confidently.
:::

::: {#a8072aa8-d14d-4526-b09a-dd8c02628094 .cell .markdown}
## 18. What Happens After You Open a PR? {#18-what-happens-after-you-open-a-pr}

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

This is normal. A closed PR is not necessarily a failure. Sometimes it
simply means the project chose a different direction.
:::

::: {#858b4079-be05-423b-812d-5fad66dbdb8f .cell .markdown}
## 19. What If Reviewers Request Changes? {#19-what-if-reviewers-request-changes}

This is very common and completely normal.

You do **not** usually create a new Pull Request for small requested
changes.

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

When you push to the same branch, the existing Pull Request updates
automatically.
:::

::: {#f8e5eb68-ad8f-47e8-b9cc-eb8159700851 .cell .code}
``` python
git add .
git commit -m "Address review comments"
git push origin fix-readme-typo
```
:::

::: {#8080e696-4039-443e-9572-9ad056122e5c .cell .markdown}
## 20. Why Branches Matter So Much for Pull Requests {#20-why-branches-matter-so-much-for-pull-requests}

Branches are central to PR workflows.

A Pull Request is typically tied to:

-   one source branch
-   one target branch

Because of this, branches give you:

-   clean separation of work
-   easier review
-   better rollback options
-   less risk of mixing unrelated changes

### Best Practice

Create one branch per topic, fix, or feature.

Examples:

-   `fix-readme-typo`
-   `docs-installation-update`
-   `bugfix-login-timeout`
-   `feature-export-csv`
:::

::: {#b8ec7434-125d-4af6-856a-16684a8bdca9 .cell .markdown}
## 21. Keeping Your Fork Up to Date {#21-keeping-your-fork-up-to-date}

The upstream repository changes over time.

If you do not sync your fork, you can end up working on an old base.

### Common Sync Workflow

``` bash
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```

### What This Does {#what-this-does}

-   `git fetch upstream`\
    downloads the latest state from the original project

-   `git switch main`\
    returns you to your local main branch

-   `git merge upstream/main`\
    brings upstream changes into your local main

-   `git push origin main`\
    updates your fork\'s `main` on GitHub
:::

::: {#944ddbf0-4084-44e7-a0d0-fe3803a02325 .cell .code}
``` python
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```
:::

::: {#932e07d4-fa43-4e08-aa4c-e3d9bb4ce629 .cell .markdown}
## 22. Alternative: Rebase Instead of Merge {#22-alternative-rebase-instead-of-merge}

Some projects prefer a cleaner linear history and may encourage
`rebase`.

Example:

``` bash
git fetch upstream
git switch my-feature
git rebase upstream/main
```

### Why Rebase?

Rebase rewrites your branch so it appears to start from the latest
upstream state.

This can make history cleaner, but it is conceptually more advanced than
merge.

### Practical Advice for Learners

-   understand `merge` first
-   use `rebase` when the project workflow expects it
-   read the contribution guide of the repository
:::

::: {#af4b09cc-61f5-4a02-a47f-652a6aa76ea0 .cell .code}
``` python
git fetch upstream
git switch my-feature
git rebase upstream/main
```
:::

::: {#b04b852c-38ff-47c3-bddc-c540b71056d6 .cell .markdown}
## 23. Common Fork Workflow Scenarios {#23-common-fork-workflow-scenarios}

### Scenario A: Small Documentation Fix

This is the easiest contribution path.

Example:

-   fork the repository
-   clone your fork
-   create branch `fix-docs-typo`
-   fix typo
-   commit
-   push
-   open PR

### Scenario B: Feature Proposal

This requires more communication.

Often you should:

-   check existing issues
-   read the contribution guide
-   discuss the idea first if the project is strict about scope
-   then implement on a branch and submit a PR

### Scenario C: Personal Long-Term Fork

In this case, you may not intend to send changes back upstream. You
still benefit from keeping `upstream` configured so you can selectively
sync useful updates.
:::

::: {#f169c873-af63-4c4c-bf8b-bbabcd3dc395 .cell .markdown}
## 24. Common Mistakes to Avoid {#24-common-mistakes-to-avoid}

### 1. Working directly on `main` {#1-working-directly-on-main}

This makes your contribution workflow messy.

### 2. Forgetting to add `upstream` {#2-forgetting-to-add-upstream}

Then syncing with the original project becomes harder.

### 3. Creating giant PRs {#3-creating-giant-prs}

Large PRs are harder to review and more likely to be delayed.

### 4. Using vague commit messages {#4-using-vague-commit-messages}

Reviewers need clarity.

### 5. Mixing unrelated changes {#5-mixing-unrelated-changes}

One PR should solve one focused problem when possible.

### 6. Ignoring project guidelines {#6-ignoring-project-guidelines}

Always check:

-   `CONTRIBUTING.md`
-   issue templates
-   PR templates
-   code style rules
:::

::: {#4de68ff4-3ce8-4fc9-a3c4-027896491f92 .cell .markdown}
## 25. How Does This Relate to Being a Contributor? {#25-how-does-this-relate-to-being-a-contributor}

This point is important enough to state clearly.

### You are **not** a contributor merely because:

-   you forked the repository
-   you cloned it
-   you changed files in your copy
-   you pushed to your fork

### You generally become a contributor to the original project when:

-   you submit a Pull Request
-   the project accepts it
-   the changes are merged into the original repository

So the true contribution happens at the point where your work enters the
upstream project.
:::

::: {#881fc6c7-9c3b-48cc-87d2-85dd4137a595 .cell .markdown}
## 26. A Complete Example {#26-a-complete-example}

Let us imagine you found a typo in the original repository\'s README.

### Step 1: Fork on GitHub

You click **Fork**.

### Step 2: Clone your fork

``` bash
git clone git@github.com:YOUR_USERNAME/project.git
cd project
```

### Step 3: Add upstream

``` bash
git remote add upstream git@github.com:ORIGINAL_OWNER/project.git
```

### Step 4: Create branch

``` bash
git switch -c fix-readme-typo
```

### Step 5: Edit file

You fix the typo in `README.md`.

### Step 6: Commit

``` bash
git add README.md
git commit -m "Fix typo in README"
```

### Step 7: Push

``` bash
git push origin fix-readme-typo
```

### Step 8: Open PR

On GitHub, you create a Pull Request from:

-   `YOUR_USERNAME/fix-readme-typo`
-   into `ORIGINAL_OWNER/main`

If maintainers approve and merge it, your change becomes part of the
real project.
:::

::: {#04d5c448-0032-4b4d-b9f5-5ed0e5aa7b76 .cell .code}
``` python
git clone git@github.com:YOUR_USERNAME/project.git
cd project
git remote add upstream git@github.com:ORIGINAL_OWNER/project.git
git switch -c fix-readme-typo
git add README.md
git commit -m "Fix typo in README"
git push origin fix-readme-typo
```
:::

::: {#384051c6-31a1-401c-8e6f-bc76f860af2d .cell .markdown}
## 27. Quick Reference Commands {#27-quick-reference-commands}

### Clone your fork

``` bash
git clone git@github.com:YOUR_USERNAME/PROJECT.git
cd PROJECT
```

### Add upstream

``` bash
git remote add upstream git@github.com:ORIGINAL_OWNER/PROJECT.git
git remote -v
```

### Create and switch to a branch

``` bash
git switch -c my-feature
```

### Stage and commit

``` bash
git add .
git commit -m "Describe your change"
```

### Push branch

``` bash
git push origin my-feature
```

### Sync your fork

``` bash
git fetch upstream
git switch main
git merge upstream/main
git push origin main
```
:::

::: {#d9ec8491-2ddb-42d5-8400-2064f3ebdef3 .cell .markdown}
## 28. Final Summary {#28-final-summary}

A **fork** is your own GitHub-hosted copy of someone else\'s repository.

You typically use a fork when:

-   you do not have write access
-   you want to contribute safely
-   you want to work independently before proposing changes

The professional workflow is:

``` text
Fork → Clone → Add upstream → Create branch → Edit → Commit → Push → Pull Request
```

Key ideas to remember:

-   a fork gives you a personal copy, not contributor status
-   contribution usually becomes official when your PR is merged
    upstream
-   `git branch` and `git switch` are essential for clean PR workflows
-   branches should be focused and separate
-   Pull Requests are both technical and communicative
-   syncing with `upstream` keeps your fork healthy and current
:::

::: {#cb547470-913e-4ff2-b61d-31f3b8d47ae5 .cell .markdown}
## 29. Suggested Practice Exercises {#29-suggested-practice-exercises}

1.  Fork a small public repository.
2.  Clone your fork locally.
3.  Add the original repository as `upstream`.
4.  Create a branch named `docs-example-change`.
5.  Edit a documentation file.
6.  Commit the change with a clear message.
7.  Push the branch.
8.  Draft a Pull Request description, even if you do not submit it.

This practical repetition is the fastest way to internalize the
workflow.
:::

::: {#35487c18-dae4-4ee9-b565-34e6bd6cb82a .cell .markdown}
# Professional Guide to Merge, Rebase, and Conflict Resolution

This section adresses three critical Git topics:

-   **merge**
-   **rebase**
-   **conflict resolution**

These concepts are essential after learning branching, forks, and Pull
Requests, because real collaboration almost always involves integrating
changes from multiple branches.

------------------------------------------------------------------------

## Learning Goals {#learning-goals}

By the end of this section, you should be able to:

-   explain what **merge** does
-   explain what **rebase** does
-   distinguish the workflows and history produced by each
-   understand when a team may prefer **merge** or **rebase**
-   detect and resolve **merge conflicts**
-   continue or abort a merge or rebase safely
-   understand why conflicts happen
-   use practical commands in real project scenarios

## 1. Why These Topics Matter {#1-why-these-topics-matter}

As soon as more than one branch exists, integration becomes necessary.

Examples:

-   you created a feature branch and now want to bring it into `main`
-   the upstream repository moved forward while you were working
-   your Pull Request is out of date
-   two developers changed the same file in incompatible ways

At that point, Git must combine histories and file changes.

That is where **merge**, **rebase**, and **conflict resolution** become
central.

## 2. The Big Picture {#2-the-big-picture}

There are two common ways to integrate branch histories:

1.  **Merge**
2.  **Rebase**

Both can bring one line of work together with another, but they do it
differently.

### High-Level Difference

-   **merge** preserves the branching history and adds a merge commit
-   **rebase** rewrites the branch so it appears to start from a new
    base

Neither is universally "better."\
The right choice depends on workflow, team preference, and whether the
branch has already been shared.

## 3. What Is a Merge? {#3-what-is-a-merge}

A **merge** combines the histories of two branches.

Suppose you have:

-   `main`
-   `feature/login`

You worked on `feature/login`, and now you want those changes in `main`.

A common workflow is:

``` bash
git switch main
git merge feature/login
```

Git then attempts to combine the histories.

If it succeeds without conflict:

-   your work is integrated into `main`
-   Git may create a **merge commit**
-   the branch structure remains visible in history
:::

::: {#8f2b1296-cb7f-4c09-a342-54c368734871 .cell .code}
``` python
git switch main
git merge feature/login
```
:::

::: {#c0f9029d-a277-4447-931c-35ad435318a8 .cell .markdown}
## 4. What Does Merge Preserve? {#4-what-does-merge-preserve}

Merge is often appreciated because it preserves the historical fact
that:

-   work diverged
-   work happened on a side branch
-   that branch was later combined

So the history may show a branch shape instead of a perfectly straight
line.

This can be useful because:

-   it reflects real development flow
-   it preserves context
-   it avoids rewriting existing commits

## 5. What Is a Rebase? {#5-what-is-a-rebase}

A **rebase** moves a branch so that it is replayed on top of another
base.

Suppose this happened:

-   `main` moved ahead
-   your `feature/login` branch was created earlier
-   you now want your branch to sit on top of the latest `main`

You might run:

``` bash
git switch feature/login
git fetch origin
git rebase origin/main
```

Git then takes the commits from `feature/login` and **reapplies** them
one by one on top of the newer base.
:::

::: {#dd79ef60-f88a-40e1-a4df-036366264669 .cell .code}
``` python
git switch feature/login
git fetch origin
git rebase origin/main
```
:::

::: {#d7d1ddcc-032a-4622-b702-9c35d7f6022a .cell .markdown}
## 6. What Does Rebase Change? {#6-what-does-rebase-change}

Rebase changes commit history.

That is because Git is not merely "attaching" the old commits to a new
base.\
It is usually **re-creating** them.

So after a rebase:

-   commit IDs change
-   the branch history becomes more linear
-   the development path may look cleaner
-   but the original branch topology is no longer preserved in the same
    way

## 7. Merge vs Rebase: Conceptual Comparison {#7-merge-vs-rebase-conceptual-comparison}

### Merge

-   combines branches
-   usually creates a merge commit
-   preserves branch structure
-   does not rewrite existing commits

### Rebase

-   reapplies commits on a new base
-   usually creates a linear history
-   rewrites commit history
-   changes commit hashes

### One Useful Mental Model

-   **merge** = "bring histories together"
-   **rebase** = "move my branch so it looks like it started later"

## 8. Example History Shapes {#8-example-history-shapes}

Imagine this simplified history.

### Before Integration

``` text
A---B---C   main
     \
      D---E   feature
```

### After Merge

``` text
A---B---C-------M   main
     \         /
      D---E----/    feature
```

`M` is a merge commit.

### After Rebase

``` text
A---B---C---D'---E'   feature
```

The rebased commits `D'` and `E'` are new versions of the original
commits `D` and `E`.

## 9. Why Teams Choose Merge {#9-why-teams-choose-merge}

A team may prefer **merge** because:

-   it is safer for shared history
-   it does not rewrite commits already pushed
-   it preserves the true branch story
-   it is easier to reason about for many teams
-   it works well in collaborative environments where branches are
    already public

Merge is often the conservative and collaboration-friendly option.

## 10. Why Teams Choose Rebase {#10-why-teams-choose-rebase}

A team may prefer **rebase** because:

-   it produces a cleaner, linear history
-   it reduces noisy merge commits
-   it can make `git log` easier to read
-   it keeps feature branches up to date before merging
-   some projects require a clean history for review

Rebase is often favored in teams that care deeply about history hygiene
and disciplined branch practices.

## 11. Golden Rule of Rebase {#11-golden-rule-of-rebase}

A very important rule:

> Do not casually rebase commits that other people may already be using.

Why?

Because rebase rewrites history.

If you rebase a shared public branch:

-   commit hashes change
-   others may still have the old commits
-   pulling and merging becomes confusing
-   collaboration can break or become messy

### Safe Rule

Rebase is safest when:

-   the branch is local to you
-   or the team explicitly agrees on the rebase workflow

## 12. Typical Merge Workflow {#12-typical-merge-workflow}

A common merge workflow after finishing a feature:

``` bash
git switch main
git pull
git merge feature/login
git push origin main
```

### What Happens?

1.  move to `main`
2.  update local `main`
3.  merge the feature branch into `main`
4.  push the integrated result
:::

::: {#580111bc-70d9-4bf9-891b-d958f6165364 .cell .code}
``` python
git switch main
git pull
git merge feature/login
git push origin main
```
:::

::: {#64428ae3-ae80-4887-9554-494ae7cf76d5 .cell .markdown}
## 13. Typical Rebase Workflow for a Feature Branch {#13-typical-rebase-workflow-for-a-feature-branch}

Suppose your branch is behind `main` and you want to update it before
opening or finalizing a PR.

``` bash
git fetch origin
git switch feature/login
git rebase origin/main
```

Then, if the branch was already pushed before, you may need:

``` bash
git push --force-with-lease origin feature/login
```

### Why `--force-with-lease`?

Because after rebase, the branch history changed.\
A normal push may be rejected.

`--force-with-lease` is safer than plain `--force` because it checks
that the remote state is what you expect before overwriting it.
:::

::: {#1a601ac5-c7a0-4593-a60d-aebadeccce0a .cell .code}
``` python
git fetch origin
git switch feature/login
git rebase origin/main
git push --force-with-lease origin feature/login
```
:::

::: {#dc2402f8-63a8-4605-8125-e6081d91c799 .cell .markdown}
## 14. What Is a Conflict? {#14-what-is-a-conflict}

A **conflict** happens when Git cannot automatically decide how to
combine changes.

This usually happens when:

-   two branches changed the same lines
-   one branch deleted a file that another branch edited
-   structural changes overlap in incompatible ways

Git is very good at automatic merging, but it cannot safely guess human
intent in every case.

## 15. Why Conflicts Happen {#15-why-conflicts-happen}

Conflicts are not signs that Git is broken.\
They are signs that Git found overlapping changes and needs human
judgment.

Typical reasons:

1.  two developers edited the same code block
2.  branch A renamed or deleted something that branch B still uses
3.  the code evolved in two directions simultaneously
4.  a long-lived branch fell far behind `main`

The longer a branch lives without syncing, the more likely conflicts
become.

## 16. Example of a Conflict {#16-example-of-a-conflict}

Imagine `main` has:

``` python
def greet():
    return "Hello"
```

And your feature branch changed it to:

``` python
def greet():
    return "Hello, user"
```

But meanwhile `main` changed it to:

``` python
def greet():
    return "Hi"
```

Git now sees that the same lines were changed differently.

It cannot confidently choose one, so it reports a conflict.

## 17. How Git Marks Conflicts in Files {#17-how-git-marks-conflicts-in-files}

When a conflict happens, Git inserts markers into the file, such as:

``` text
<<<<<<< HEAD
return "Hi"
=======
return "Hello, user"
>>>>>>> feature/login
```

### Meaning

-   `<<<<<<< HEAD`\
    the current branch\'s version

-   `=======`\
    separator

-   `>>>>>>> feature/login`\
    the incoming branch\'s version

You must edit the file manually and remove these markers.

## 18. Conflict During Merge {#18-conflict-during-merge}

If a conflict happens during merge:

``` bash
git switch main
git merge feature/login
```

Git may stop and tell you which files are conflicted.

Then the normal process is:

1.  open conflicted files
2.  decide the correct final content
3.  remove conflict markers
4.  stage the resolved files
5.  complete the merge
:::

::: {#a1c20e4f-fa0e-40fc-9777-94fa2cc1892d .cell .code}
``` python
git switch main
git merge feature/login
```
:::

::: {#567d5b74-903d-47f2-b9a3-7a348a628f53 .cell .markdown}
## 19. Conflict During Rebase {#19-conflict-during-rebase}

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
:::

::: {#f7e79763-3406-4304-855e-d6167ef6934d .cell .code}
``` python
git switch feature/login
git rebase origin/main
git add .
git rebase --continue
```
:::

::: {#e4278834-5dad-4489-b680-9df9fd839310 .cell .markdown}
## 20. Merge Conflict Resolution: Step-by-Step {#20-merge-conflict-resolution-step-by-step}

Here is the standard sequence.

### Step 1: Attempt the merge

``` bash
git switch main
git merge feature/login
```

### Step 2: Check status

``` bash
git status
```

Git shows which files are unmerged.

### Step 3: Open each conflicted file

Look for markers like:

``` text
<<<<<<<
=======
>>>>>>>
```

### Step 4: Edit to the correct final result

Choose:

-   your version
-   their version
-   or a combination

### Step 5: Stage resolved files

``` bash
git add path/to/file
```

### Step 6: Finish the merge

If needed:

``` bash
git commit
```

Sometimes Git prepares the merge commit automatically after staging,
depending on the conflict path and tooling.
:::

::: {#bfc9c84f-3523-4653-b3c1-c29ca4c26b01 .cell .code}
``` python
git switch main
git merge feature/login
git status
git add path/to/file
git commit
```
:::

::: {#9c24c740-79db-4054-bcd2-a41c29296002 .cell .markdown}
## 21. Rebase Conflict Resolution: Step-by-Step {#21-rebase-conflict-resolution-step-by-step}

### Step 1: Start the rebase

``` bash
git switch feature/login
git rebase origin/main
```

### Step 2: Git stops on a conflict

Check status:

``` bash
git status
```

### Step 3: Fix the file contents

Remove markers and keep the correct result.

### Step 4: Stage the resolved files

``` bash
git add path/to/file
```

### Step 5: Continue the rebase

``` bash
git rebase --continue
```

Git then moves to the next commit.\
If another conflict appears, repeat.

### Optional: Skip a problematic commit

``` bash
git rebase --skip
```

### Optional: Abort the whole rebase

``` bash
git rebase --abort
```
:::

::: {#e65ae029-9204-4953-b76f-cd45e9cbabc8 .cell .code}
``` python
git switch feature/login
git rebase origin/main
git status
git add path/to/file
git rebase --continue
```
:::

::: {#5f72d6fa-7403-47b3-b039-68e39133ae81 .cell .markdown}
## 22. Aborting Merge or Rebase {#22-aborting-merge-or-rebase}

Sometimes you decide the current integration attempt is not worth
continuing right now.

### Abort a Merge

``` bash
git merge --abort
```

This attempts to return the repository to the pre-merge state.

### Abort a Rebase

``` bash
git rebase --abort
```

This attempts to restore the branch to how it looked before the rebase
began.

These commands are very important safety tools.
:::

::: {#d9403db7-0fdc-4329-ae1f-91462ed51eeb .cell .code}
``` python
git merge --abort
git rebase --abort
```
:::

::: {#cd0c9fd9-e5b3-49a1-b03e-885f26eb92bf .cell .markdown}
## 23. Merge Conflict Example: Human Decision {#23-merge-conflict-example-human-decision}

Suppose a file contains:

``` python
def connect():
    timeout = 30
    return timeout
```

In `main`, someone changed it to:

``` python
def connect():
    timeout = 10
    return timeout
```

In your branch, you changed it to:

``` python
def connect():
    timeout = 60
    return timeout
```

Git cannot decide whether the timeout should be `10` or `60`.

A human must decide:

-   should the final value be `10`?
-   `60`?
-   another value entirely?
-   should logic be refactored?

This shows why conflict resolution is a semantic task, not just a
mechanical one.

## 24. Useful Commands During Conflict Resolution {#24-useful-commands-during-conflict-resolution}

### See current status

``` bash
git status
```

### See differences

``` bash
git diff
```

### See staged differences

``` bash
git diff --staged
```

### After resolving

``` bash
git add path/to/file
```

These are the core commands you will use repeatedly during conflict
handling.
:::

::: {#67a6ff5f-fa51-4bf2-8b12-5727442ad2c7 .cell .code}
``` python
git status
git diff
git diff --staged
```
:::

::: {#dd2b0445-b2b0-497c-88ab-cae4aca56a6a .cell .markdown}
## 25. Conflict Prevention Strategies {#25-conflict-prevention-strategies}

You cannot eliminate all conflicts, but you can reduce them.

### Good Practices

1.  **Keep branches short-lived**\
    Long-lived branches drift and conflict more.

2.  **Sync frequently with main or upstream**

    -   merge from main regularly
    -   or rebase regularly if that is the team workflow

3.  **Make smaller Pull Requests**\
    Small focused changes are easier to integrate.

4.  **Communicate with teammates**\
    If two people plan to edit the same subsystem, coordinate early.

5.  **Avoid giant unrelated commits**\
    Big mixed commits make conflict analysis much harder.

## 26. Merge or Rebase Before a Pull Request? {#26-merge-or-rebase-before-a-pull-request}

This depends on the team.

### Common Team Preferences

-   Some teams want you to **merge `main` into your feature branch**
-   Others want you to **rebase your feature branch onto `main`**
-   Some teams squash everything at merge time on GitHub

### Correct Practical Rule

Always check:

-   `CONTRIBUTING.md`
-   repository guidelines
-   maintainer expectations

There is no universal rule that fits every project.

## 27. What Is Squash Merge? {#27-what-is-squash-merge}

A related concept is **squash merge**.

Instead of preserving all commits from the feature branch, the branch is
merged as **one single commit**.

This is often offered in GitHub Pull Requests.

### Why Teams Use It

-   cleaner history on `main`
-   less noise from many small "work in progress" commits
-   easier project history reading

### Important Distinction

-   squash merge is not the same as rebase
-   rebase rewrites branch history before integration
-   squash merge compresses branch history at integration time

## 28. Practical Merge vs Rebase Guidance {#28-practical-merge-vs-rebase-guidance}

### Prefer Merge When

-   you want safety on shared branches
-   you do not want to rewrite history
-   the team values preserving the branch story
-   the branch is already public and collaborative

### Prefer Rebase When

-   the branch is primarily yours
-   you want a cleaner linear history
-   the project expects rebased branches
-   you understand the implications of history rewriting

### A Good Beginner Rule

-   learn **merge** first
-   then learn **rebase**
-   use **rebase carefully**

## 29. Realistic Scenario 1: Updating a Feature Branch with Merge {#29-realistic-scenario-1-updating-a-feature-branch-with-merge}

You started `feature/login` a week ago.\
Meanwhile, `main` received new commits.

You want to update your feature branch without rewriting history.

One approach:

``` bash
git fetch origin
git switch feature/login
git merge origin/main
```

Pros:

-   simple
-   history-safe
-   no rewrite

Cons:

-   branch history may become noisier
:::

::: {#374fee6d-dd48-4541-9440-ad0913aae866 .cell .code}
``` python
git fetch origin
git switch feature/login
git merge origin/main
```
:::

::: {#7a9662ea-8b64-407f-9be7-b0c8d841a3ae .cell .markdown}
## 30. Realistic Scenario 2: Updating a Feature Branch with Rebase {#30-realistic-scenario-2-updating-a-feature-branch-with-rebase}

Same situation, but you want a cleaner branch history.

``` bash
git fetch origin
git switch feature/login
git rebase origin/main
```

Pros:

-   linear history
-   cleaner PR branch

Cons:

-   rewrites commit history
-   may require force-push
-   must be used carefully if shared
:::

::: {#3e45953c-3929-4dfa-b85e-4e85ad8d7885 .cell .code}
``` python
git fetch origin
git switch feature/login
git rebase origin/main
git push --force-with-lease origin feature/login
```
:::

::: {#ba391743-aa15-468b-b16d-f3f4851df44a .cell .markdown}
## 31. Realistic Scenario 3: Conflict in a Pull Request {#31-realistic-scenario-3-conflict-in-a-pull-request}

Suppose GitHub says:

> This branch has conflicts that must be resolved.

That usually means your branch can no longer be cleanly merged into the
target branch.

A common local resolution workflow is:

``` bash
git fetch upstream
git switch my-feature
git rebase upstream/main
```

or, depending on team style:

``` bash
git fetch upstream
git switch my-feature
git merge upstream/main
```

Then:

-   resolve conflicts locally
-   run tests
-   commit or continue
-   push the updated branch
-   the PR updates automatically

## 32. Visual Thinking: Conflict Is About Overlapping Intent {#32-visual-thinking-conflict-is-about-overlapping-intent}

A conflict is not merely "same file changed."

Two branches can modify the same file without conflict if they touch
different areas.

Conflict is more about:

-   overlapping edits
-   incompatible structural changes
-   ambiguous final intent

So conflict resolution requires understanding the code or text, not just
knowing Git commands.

## 33. Common Mistakes {#33-common-mistakes}

### 1. Rebasing a shared branch without coordination {#1-rebasing-a-shared-branch-without-coordination}

This confuses collaborators.

### 2. Using `--force` carelessly {#2-using---force-carelessly}

Prefer:

``` bash
git push --force-with-lease
```

### 3. Resolving conflicts mechanically without understanding the code {#3-resolving-conflicts-mechanically-without-understanding-the-code}

This can introduce subtle bugs.

### 4. Ignoring tests after conflict resolution {#4-ignoring-tests-after-conflict-resolution}

Always verify behavior after integrating complex changes.

### 5. Letting branches become stale {#5-letting-branches-become-stale}

The longer you wait, the harder integration becomes.

## 34. Safe Recovery Mindset {#34-safe-recovery-mindset}

When Git reports a conflict, do not panic.

A professional recovery mindset is:

1.  read the status carefully
2.  identify which branches are involved
3.  inspect the conflicted files
4.  decide the correct final code
5.  stage only when truly resolved
6.  continue or abort as needed
7.  run tests after resolution

Git is usually waiting for a human decision, not punishing you.

## 35. Quick Reference Commands {#35-quick-reference-commands}

### Merge a branch into main

``` bash
git switch main
git merge feature-name
```

### Rebase a branch onto main

``` bash
git switch feature-name
git fetch origin
git rebase origin/main
```

### Continue rebase after resolving conflicts

``` bash
git add .
git rebase --continue
```

### Abort a rebase {#abort-a-rebase}

``` bash
git rebase --abort
```

### Abort a merge {#abort-a-merge}

``` bash
git merge --abort
```

### Safer force push after rebase

``` bash
git push --force-with-lease origin feature-name
```

## 36. Final Summary {#36-final-summary}

**Merge** and **rebase** are both integration tools, but they produce
different histories.

### Merge {#merge}

-   combines branches
-   preserves branch structure
-   does not rewrite history
-   often safer for shared collaboration

### Rebase {#rebase}

-   replays commits on a new base
-   creates linear history
-   rewrites commit IDs
-   useful when used intentionally and carefully

### Conflicts

-   happen when Git cannot choose between overlapping changes
-   must be resolved by understanding the intended final result
-   are normal in real teamwork

The deeper lesson is this:

> Git integration is not only about commands. It is about managing
> history, collaboration, and code intent.

## 37. Suggested Practice Exercises {#37-suggested-practice-exercises}

1.  Create a repository with a `main` branch and a `feature` branch.
2.  Modify the same line differently in both branches.
3.  try `git merge` and resolve the conflict manually.
4.  reset the repository and repeat using `git rebase`.
5.  practice:
    -   `git status`
    -   `git diff`
    -   `git merge --abort`
    -   `git rebase --abort`
    -   `git rebase --continue`
6.  compare the final history using:

``` bash
git log --oneline --graph --all
```

That comparison is one of the fastest ways to truly understand the
difference between merge and rebase.
:::

::: {#9347ce20-aa62-419c-92aa-299b35972e71 .cell .code}
``` python
git log --oneline --graph --all
```
:::

::: {#f01a61af-e613-4670-b33c-1bfc26e16c3e .cell .markdown}
# Professional Guide to the Contributor Workflow (Without Fork)

This section is for the case where you are already a **contributor** or
team member and have **write access** to the main repository. In this
situation, the workflow is usually different from the fork-based
open-source workflow.

Instead of:

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

------------------------------------------------------------------------

## Learning Goals {#learning-goals}

By the end of this section, you should be able to:

-   understand how contributor workflow differs from fork workflow
-   know when a fork is unnecessary
-   clone the main repository directly
-   create and manage contribution branches inside the main repository
-   push branches to the shared repository
-   open Pull Requests from a branch in the same repository
-   understand when direct pushes to `main` are discouraged
-   work safely in a collaborative team environment

## 1. What Changes When You Are a Contributor? {#1-what-changes-when-you-are-a-contributor}

When you are a **contributor with write access**, you no longer need
your own fork in order to propose changes.

That is the key difference.

### Fork-Based Workflow

Used when:

-   you do **not** have write access
-   you contribute from outside the core team
-   you submit changes through your own fork

### Contributor Workflow

Used when:

-   you **do** have write access
-   you are part of the organization or trusted team
-   you can push branches directly to the main repository

So if you are already a contributor, the repository itself can usually
act as the shared collaboration space.

## 2. Do Contributors Still Use Pull Requests? {#2-do-contributors-still-use-pull-requests}

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

### Why? {#why}

Because Pull Requests are not only about permissions.\
They are also about:

-   review quality
-   design discussion
-   CI checks
-   documentation of why a change happened
-   protecting the stability of `main`

## 3. When Is a Fork Usually Not Needed? {#3-when-is-a-fork-usually-not-needed}

A fork is usually unnecessary when:

-   you are already on the project team
-   you have write access to the main repository
-   your organization expects branch-based collaboration inside the same
    repo

In that case, the normal pattern is:

``` text
main repository → feature branch → PR into main
```

There is no extra GitHub copy under your personal account.

## 4. High-Level Contributor Workflow {#4-high-level-contributor-workflow}

A common contributor workflow looks like this:

``` text
Clone main repository → Create feature branch → Edit → Commit → Push branch → Open Pull Request → Review → Merge
```

This is similar to the fork workflow, but the difference is where the
branch lives:

-   in fork workflow, the branch lives in **your fork**
-   in contributor workflow, the branch lives in the **main shared
    repository**

## 5. Step 1: Clone the Main Repository {#5-step-1-clone-the-main-repository}

Since you already have access, you normally clone the main repository
directly.

``` bash
git clone git@github.com:ORGANIZATION/PROJECT.git
cd PROJECT
```

There is no need to fork first.

Your `origin` now points directly to the shared repository.
:::

::: {#19383747-f002-4938-84ba-4ea23c984094 .cell .code}
``` python
git clone git@github.com:ORGANIZATION/PROJECT.git
cd PROJECT
```
:::

::: {#3ad793e5-75e9-4f1e-8034-e6854133f6e5 .cell .markdown}
## 6. Understanding `origin` in Contributor Workflow {#6-understanding-origin-in-contributor-workflow}

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

In many cases, there is no need for an `upstream` remote at all, because
you are already working directly with the main repository.
:::

::: {#d4a3a98e-983e-48ee-8d1d-7fbef9bf7910 .cell .code}
``` python
git remote -v
```
:::

::: {#75e72c92-4242-4200-97fd-4cc3cb435cf1 .cell .markdown}
## 7. Step 2: Update Your Local Main Branch {#7-step-2-update-your-local-main-branch}

Before starting new work, make sure your local `main` is up to date.

``` bash
git switch main
git pull
```

This is important because you usually want to branch from the latest
project state.
:::

::: {#32a3631a-c6c9-4ed0-ab3a-0b772dd0a9d1 .cell .code}
``` python
git switch main
git pull
```
:::

::: {#eec89204-91f4-42d9-b0e8-5ddf511f946e .cell .markdown}
## 8. Step 3: Create a Feature Branch {#8-step-3-create-a-feature-branch}

Even when you are a contributor, it is usually best **not** to work
directly on `main`.

Instead, create a focused branch:

``` bash
git switch -c feature/improve-login-validation
```

or:

``` bash
git switch -c fix/readme-typo
```

### Why Branches Still Matter

Branches let you:

-   isolate one change
-   keep `main` stable
-   make review easier
-   avoid mixing unrelated work
-   simplify rollback and debugging
:::

::: {#a459bf3e-e074-4dff-a231-a9989cc3bcb0 .cell .code}
``` python
git switch -c feature/improve-login-validation
```
:::

::: {#cb528d4e-9cc2-4cd7-a5a8-5f071832d65d .cell .markdown}
## 9. Should Contributors Push Directly to `main`? {#9-should-contributors-push-directly-to-main}

Usually, no.

Even if technically allowed, many teams discourage or forbid direct
pushes to `main`.

### Why Direct Pushes Are Risky

They can:

-   bypass code review
-   skip discussion
-   introduce unstable code
-   make auditing harder
-   break CI expectations
-   surprise teammates

### Better Practice

Use:

-   short-lived feature branches
-   Pull Requests
-   review gates
-   protected branch rules

In modern team workflows, `main` is often protected specifically to
prevent accidental direct pushes.

## 10. Protected Branches {#10-protected-branches}

Many GitHub repositories use **protected branches**.

A protected branch may require:

-   Pull Request before merge
-   at least one approval
-   passing CI checks
-   resolved conversations
-   linear history
-   no force-pushes
-   no direct pushes

This means that even contributors with write access still follow a
formal workflow.

So contributor status gives you **access**, but not necessarily
unrestricted freedom.

## 11. Step 4: Make Changes and Commit {#11-step-4-make-changes-and-commit}

After creating your branch, edit the necessary files and commit your
work.

``` bash
git add .
git commit -m "Improve login validation for empty email input"
```

### Good Commit Message Advice {#good-commit-message-advice}

Strong commit messages are:

-   clear
-   focused
-   action-based
-   easy for reviewers to understand
:::

::: {#eb1c3da9-adc7-4614-849c-6830eb0643a0 .cell .code}
``` python
git add .
git commit -m "Improve login validation for empty email input"
```
:::

::: {#5b4971a1-afc0-443a-8969-da4335c5b77e .cell .markdown}
## 12. Step 5: Push Your Branch to the Shared Repository {#12-step-5-push-your-branch-to-the-shared-repository}

Now push your branch to the main shared repository:

``` bash
git push origin feature/improve-login-validation
```

This is a major difference from the fork workflow.

### In Fork Workflow

You push to:

``` text
your fork
```

### In Contributor Workflow

You push to:

``` text
the main repository itself
```
:::

::: {#a2603ed0-f70f-42fd-912f-4dd8957ef106 .cell .code}
``` python
git push origin feature/improve-login-validation
```
:::

::: {#cdfe6fbe-b285-44a9-9ec9-64437db4dd76 .cell .markdown}
## 13. Step 6: Open a Pull Request from the Same Repository {#13-step-6-open-a-pull-request-from-the-same-repository}

Now create a Pull Request.

In this case, both the source branch and target branch are usually in
the **same repository**.

Example direction:

``` text
ORGANIZATION/PROJECT:feature/improve-login-validation
    →
ORGANIZATION/PROJECT:main
```

This is different from the fork model, where the source branch lives in
your fork.

## 14. Why PRs Still Matter for Contributors {#14-why-prs-still-matter-for-contributors}

A Pull Request is useful even when both branches are in the same
repository.

PRs provide:

-   peer review
-   architectural feedback
-   CI validation
-   historical record of discussion
-   visibility for teammates
-   a checkpoint before code reaches `main`

In strong engineering teams, the Pull Request is a collaboration tool,
not just a permission workaround.

## 15. Same-Repository PR vs Fork PR {#15-same-repository-pr-vs-fork-pr}

### Fork PR

``` text
your-username/PROJECT:my-branch
    →
ORIGINAL_OWNER/PROJECT:main
```

### Contributor PR

``` text
ORGANIZATION/PROJECT:my-branch
    →
ORGANIZATION/PROJECT:main
```

### Key Difference

The branch source lives in a different place:

-   fork PR → branch in your fork
-   contributor PR → branch in the main repository

## 16. Team Naming Conventions for Branches {#16-team-naming-conventions-for-branches}

Professional teams often use naming conventions such as:

-   `feature/add-export-button`
-   `fix/login-timeout`
-   `docs/update-installation-guide`
-   `refactor/auth-service`
-   `test/add-user-service-tests`

These names help reviewers understand branch intent quickly.

## 17. Typical Contributor Workflow Example {#17-typical-contributor-workflow-example}

Let us imagine you are part of the team and want to fix a bug.

### Steps {#steps}

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
:::

::: {#7c98600a-e144-4cc7-a47c-f45214d87ed3 .cell .code}
``` python
git clone git@github.com:ORGANIZATION/PROJECT.git
cd PROJECT
git switch main
git pull
git switch -c fix/login-timeout
git add .
git commit -m "Fix login timeout handling"
git push origin fix/login-timeout
```
:::

::: {#63049cbf-4cfa-4f3f-931c-600578de8f75 .cell .markdown}
## 18. What If Reviewers Request Changes? {#18-what-if-reviewers-request-changes}

Just like in fork workflow, you usually stay on the same branch.

Example:

``` bash
git add .
git commit -m "Address review comments"
git push origin fix/login-timeout
```

The Pull Request updates automatically because it is tracking that same
branch.
:::

::: {#1bb50297-6971-4e04-a84a-07dc3fe6d01e .cell .code}
``` python
git add .
git commit -m "Address review comments"
git push origin fix/login-timeout
```
:::

::: {#e8996ac1-34e8-442a-af6e-358d2425ac8f .cell .markdown}
## 19. Syncing Your Branch with Main {#19-syncing-your-branch-with-main}

While your Pull Request is open, `main` may move forward.

You may need to update your branch.

### Option A: Merge `main` into your branch

``` bash
git switch fix/login-timeout
git fetch origin
git merge origin/main
```

### Option B: Rebase onto `main`

``` bash
git switch fix/login-timeout
git fetch origin
git rebase origin/main
```

Which option to use depends on team policy.
:::

::: {#80b45246-4d90-4a31-a2b2-f67b274b7f12 .cell .code}
``` python
git switch fix/login-timeout
git fetch origin
git merge origin/main
```
:::

::: {#d13c7c07-39fd-4072-b296-0da50ac94ad2 .cell .code}
``` python
git switch fix/login-timeout
git fetch origin
git rebase origin/main
```
:::

::: {#c12d3416-6389-4ae7-bc90-dcfb18e820cc .cell .markdown}
## 20. What If the Team Allows Direct Pushes? {#20-what-if-the-team-allows-direct-pushes}

Some small teams or personal team projects do allow direct pushes to
`main`.

That workflow might look like:

``` bash
git switch main
git pull
git add .
git commit -m "Small update"
git push origin main
```

### Important Warning

Even if allowed, direct pushes are usually best reserved for:

-   tiny low-risk changes
-   emergency hotfixes
-   trusted internal workflows
-   solo-maintainer repositories

For most collaborative work, feature branches and PRs are still safer.
:::

::: {#22892d50-f3f1-471a-a8eb-df6f7e62087c .cell .code}
``` python
git switch main
git pull
git add .
git commit -m "Small update"
git push origin main
```
:::

::: {#9f988b68-18fa-4fdb-a45a-07d808e61f5d .cell .markdown}
## 21. Contributor Workflow and Code Review Culture {#21-contributor-workflow-and-code-review-culture}

Being a contributor is not only a permission issue.\
It is also a professional responsibility.

A strong contributor usually:

-   keeps branches focused
-   writes understandable commit messages
-   respects review comments
-   runs tests before pushing
-   updates stale branches
-   avoids mixing unrelated changes
-   understands team standards and repository rules

So contributor access should be paired with disciplined workflow habits.

## 22. Protected Main + Contributor Branches = Common Professional Pattern {#22-protected-main--contributor-branches--common-professional-pattern}

A very common modern setup is:

-   contributors have write access
-   `main` is protected
-   contributors can create branches
-   contributors push those branches to the same repo
-   PR review is required
-   CI must pass before merge

This combination gives teams the best of both worlds:

-   easy collaboration
-   strong quality control

## 23. Contributor Workflow vs Fork Workflow: Side-by-Side {#23-contributor-workflow-vs-fork-workflow-side-by-side}

### Fork Workflow

Used when you do not have write access.

``` text
Fork → Clone fork → Create branch → Push to fork → PR to upstream
```

### Contributor Workflow {#contributor-workflow}

Used when you have write access.

``` text
Clone main repo → Create branch → Push to same repo → PR to main
```

### Main Conceptual Difference

The technical Git operations are similar.

The main difference is:

-   where `origin` points
-   where the branch is pushed
-   whether a fork exists at all

## 24. Common Mistakes Contributors Make {#24-common-mistakes-contributors-make}

### 1. Working directly on `main` {#1-working-directly-on-main}

This increases risk and reduces review quality.

### 2. Pushing half-finished work to shared branches without clarity {#2-pushing-half-finished-work-to-shared-branches-without-clarity}

Teammates may review unstable code too early.

### 3. Opening giant PRs {#3-opening-giant-prs}

Smaller changes are easier to review and merge.

### 4. Ignoring branch naming conventions {#4-ignoring-branch-naming-conventions}

This makes the shared repository harder to navigate.

### 5. Forgetting to pull before creating a branch {#5-forgetting-to-pull-before-creating-a-branch}

You may branch from outdated `main`.

### 6. Rebasing shared team branches carelessly {#6-rebasing-shared-team-branches-carelessly}

If others use the same branch, history rewriting becomes dangerous.

## 25. Suggested Best Practices for Contributors {#25-suggested-best-practices-for-contributors}

1.  always update `main` before branching\
2.  create one branch per topic or fix\
3.  use clear names for branches\
4.  commit logically, not randomly\
5.  open Pull Requests early enough for review\
6.  keep PRs focused and readable\
7.  merge or rebase from `main` regularly if the branch stays open\
8.  follow repository rules and templates\
9.  do not treat write access as permission to skip discipline\
10. protect `main` whenever possible

## 26. Complete Example: Contributor Fixing a Bug {#26-complete-example-contributor-fixing-a-bug}

Suppose you are on the core team of a project and want to fix a timeout
bug.

### Workflow

``` bash
git clone git@github.com:ORGANIZATION/PROJECT.git
cd PROJECT
git switch main
git pull
git switch -c fix/login-timeout
# edit files
git add .
git commit -m "Fix login timeout handling"
git push origin fix/login-timeout
```

Then on GitHub:

-   open a Pull Request from `fix/login-timeout` into `main`
-   wait for CI and review
-   make requested updates if needed
-   merge after approval
:::

::: {#010d8fd4-9011-4702-b355-9b19784807cb .cell .code}
``` python
git clone git@github.com:ORGANIZATION/PROJECT.git
cd PROJECT
git switch main
git pull
git switch -c fix/login-timeout
git add .
git commit -m "Fix login timeout handling"
git push origin fix/login-timeout
```
:::

::: {#c86e9002-cb16-4cf2-be42-e394e67e842c .cell .markdown}
## 27. Quick Reference Commands {#27-quick-reference-commands}

### Clone main repository

``` bash
git clone git@github.com:ORGANIZATION/PROJECT.git
cd PROJECT
```

### Update main

``` bash
git switch main
git pull
```

### Create branch

``` bash
git switch -c feature/my-change
```

### Commit changes

``` bash
git add .
git commit -m "Describe your change"
```

### Push branch {#push-branch}

``` bash
git push origin feature/my-change
```

### Update branch with main using merge

``` bash
git fetch origin
git switch feature/my-change
git merge origin/main
```

### Update branch with main using rebase

``` bash
git fetch origin
git switch feature/my-change
git rebase origin/main
```

## 28. Final Summary {#28-final-summary}

When you are already a **contributor with write access**, you usually do
**not** need a fork.

The standard workflow becomes:

``` text
Clone main repository → Create branch → Commit → Push branch → Pull Request
```

Key ideas:

-   write access removes the need for a fork
-   it does not remove the value of branches or Pull Requests
-   direct pushes to `main` are often discouraged
-   professional teams often combine contributor access with protected
    branches
-   contributor workflow is about collaboration discipline, not just
    permissions

## 29. Suggested Practice Exercises {#29-suggested-practice-exercises}

1.  clone a repository where you have write access
2.  update your local `main`
3.  create a branch called `docs/test-contributor-flow`
4.  make a small documentation change
5.  commit the change
6.  push the branch to the shared repository
7.  draft a Pull Request from that branch into `main`
8.  optionally simulate a review update by making one more commit and
    pushing again

This practice will make the contributor workflow feel natural and
distinct from the fork workflow.
:::

::: {#0db059fd-5112-4b32-a413-a1d8bd3bc80c .cell .markdown}
# Professional Guide to Reset, Revert, Restore, Amend, and Cherry-Pick

This section is on five highly practical Git topics:

-   **reset**
-   **revert**
-   **restore**
-   **amend**
-   **cherry-pick**

These commands are essential once a learner understands commits,
branches, merges, rebases, and Pull Requests.

They help answer real-world questions such as:

-   How do I undo something safely?
-   How do I fix the last commit?
-   How do I restore a file?
-   How do I move one specific commit to another branch?
-   When should I rewrite history, and when should I preserve it?

------------------------------------------------------------------------

## Learning Goals {#learning-goals}

By the end of this section, you should be able to:

-   explain the difference between **reset** and **revert**
-   understand the purpose of **restore**
-   amend the most recent commit safely
-   move a specific commit with **cherry-pick**
-   choose the correct tool depending on whether history should be
    rewritten or preserved
-   avoid common mistakes when undoing or reusing work

## 1. Why These Commands Matter {#1-why-these-commands-matter}

In real Git usage, people often need to correct mistakes.

Examples:

-   you committed too early
-   you forgot to include one file in the last commit
-   you want to undo a bad commit without deleting history
-   you accidentally staged the wrong file
-   you want to bring one bug fix from one branch into another
-   you want to discard local edits and go back to the committed state

These are not rare situations. They are part of normal Git work.

That is why `reset`, `revert`, `restore`, `amend`, and `cherry-pick` are
core professional tools.

## 2. A High-Level Map {#2-a-high-level-map}

A useful mental model is this:

-   **reset** → move branch pointers and optionally unstage or discard
    changes
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
