# Git Materials (Final Corrected Version)

## Pull Command

The **pull** command fetches changes from a remote repository and then integrates them into your current branch (usually via merge or rebase).

```bash
git pull
```

---

## Reset Warning Update

`git reset --hard` can permanently destroy local work. Treat this command as destructive and use it only when you are certain.

---

## Amend Clarification

Amend is safest when:
- the commit is still local
- you have not pushed it yet

Additionally, amending a pushed commit rewrites history and usually requires a force-push, which can disrupt collaborators.

---

## Cherry-Pick Clarification

Cherry-pick copies the effect of a commit—it does not move the original commit. This can create duplicate history if used carelessly.

---

## Annotate vs Blame

`git annotate` is closely related to `git blame`, which is more commonly used in modern workflows.

---

## Remote URL Consistency Note

Use consistent remote formats (either HTTPS or SSH) throughout your workflow to avoid confusion.

Example (HTTPS):

```bash
git remote add origin https://github.com/username/repo.git
```

Example (SSH):

```bash
git remote add origin git@github.com:username/repo.git
```
