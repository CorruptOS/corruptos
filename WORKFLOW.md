# CorruptOS — Git Workflow

Every session, every person, no exceptions. The shape is always:
**Sync → Branch → Work → Check → Ship → Clean up.**

---

## 1. Sync
Get your local `main` matching what's actually on GitHub.

```
git checkout main
git pull origin main
```

Why: you want to build on top of the latest merged code, not something stale.

---

## 2. Branch
Make your own sandbox, named `yourname/what-youre-doing`.

```
git checkout -b yourname/what-youre-doing
```

Examples: `rehan/gdt-setup`, `vijay/paging`, `bihari/keyboard-driver`

Why: keeps your in-progress work isolated so `main` never breaks while you're mid-experiment.

---

## 3. Work
Write your code normally.

- Turn on **"insert final newline on save"** in your editor (VS Code: search settings for "insert final newline", check the box). Avoids weird git-blame/diff issues.
- If working in the same file as someone else, post in `#on-deck` on Discord what you're touching before you start.

---

## 4. Check
Before committing, look at exactly what you changed.

```
git status
git diff
```

Confirm you only see `+` additions where you meant to add, and no unexpected `-` deletions on lines you didn't mean to touch.

---

## 5. Ship
Save locally, then send to GitHub.

```
git add .
git commit -m "clear description of what you did"
git push origin yourname/what-youre-doing
```

Commit message should describe *what changed*, not "stuff" or "fix". Examples:
- `add GDT setup in boot.asm`
- `fix segment descriptor bug`

---

## 6. PR + Review + Merge (on GitHub)

1. Open the Pull Request (GitHub prompts you automatically after a push)
2. Post the PR link in `#pr-reviews` (or it'll auto-post via the GitHub bot in `#github-activity`)
3. Someone else reviews — reads the diff, approves or requests changes
4. **1 approval required** before the merge button unlocks (branch protection is on)
5. Merge the PR
6. Delete the branch (GitHub offers a button right after merging)

---

## 7. Loop back to step 1

```
git checkout main
git pull origin main
```

Now your local main has everyone's latest merged work. Ready for the next session.

---

## If you get a merge conflict

```
git checkout main
git pull origin main
git checkout yourname/your-branch
git merge main
```

Git will mark the conflicting spot like this:

```
<<<<<<< HEAD
your version
=======
their version
>>>>>>> main
```

Manually decide the final version, delete the `<<<<<<<`, `=======`, `>>>>>>>` markers, save.

```
git add .
git commit -m "resolve merge conflict"
git push origin yourname/your-branch
```

Then merge the PR normally.

---

## The one rule

**Pull before you start. Push after you finish.** That phrase alone prevents most of the mistakes that'll trip you up.
