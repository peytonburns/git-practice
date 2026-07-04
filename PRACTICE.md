# Git Sandbox — Practice Exercises

Work in `/Users/burnsp1/git-sandbox`. The "remote" at
`/Users/burnsp1/git-sandbox-remote.git` behaves like GitHub on your machine.

## Setup (already done)

```bash
cd /Users/burnsp1/git-sandbox
git remote -v
# origin → ../git-sandbox-remote.git
```

---

## Exercise 1: The commit loop

**Goal:** Save a snapshot of your work locally.

```bash
cd /Users/burnsp1/git-sandbox
echo "Line 3: added in exercise 1" >> notes.txt
git status          # see unstaged changes
git diff            # see what changed
git add notes.txt   # stage the change
git status          # see staged changes
git commit -m "Add line 3 to notes"
git log --oneline   # see your history
```

---

## Exercise 2: Branches

**Goal:** Work on a side experiment without touching `main`.

```bash
git branch                    # list branches (* = current)
git switch -c feature/experiment
echo "experimental idea" >> notes.txt
git add notes.txt
git commit -m "Try an experiment on a branch"
git log --oneline --graph --all

git switch main               # back to main — notes.txt unchanged here
git log --oneline --graph --all

git switch feature/experiment
git log --oneline -3
```

Merge when happy:

```bash
git switch main
git merge feature/experiment -m "Merge experiment into main"
git branch -d feature/experiment
```

---

## Exercise 3: Push and pull (local remote)

**Goal:** Sync your local repo with the simulated remote.

```bash
# From main, after new commits:
git push -u origin main

# Simulate a teammate (or second machine):
cd /tmp
git clone /Users/burnsp1/git-sandbox-remote.git git-sandbox-clone
cd git-sandbox-clone
echo "change from clone" >> notes.txt
git add notes.txt && git commit -m "Change from cloned repo"
git push origin main

# Back in your sandbox — get their work:
cd /Users/burnsp1/git-sandbox
git pull origin main
```

---

## Exercise 4: Pull with local changes (common gotcha)

```bash
echo "my local edit" >> notes.txt
# Don't commit yet — try pulling. Git may block you.
git pull origin main
# Fix: commit or stash first, then pull
git stash
git pull origin main
git stash pop
```

---

## Exercise 5: Undo safely (sandbox only!)

```bash
# Undo unstaged edits to one file:
git restore notes.txt

# Undo last commit, keep changes staged:
git reset --soft HEAD~1

# Undo last commit, discard changes (destructive):
# git reset --hard HEAD~1   # only in sandbox!
```

---

## Optional: Connect to real GitHub later

When ready, create an empty repo on GitHub, then:

```bash
git remote rename origin sandbox-remote
git remote add origin git@github.com:YOUR_USER/git-sandbox.git
git push -u origin main
```

Keep `sandbox-remote` if you still want the local simulator.

---

## Cheat sheet

| Command | Mental model |
|---------|----------------|
| `git status` | What's changed? |
| `git add` | Choose what goes in the next snapshot |
| `git commit` | Save snapshot with a message |
| `git branch` | List/create alternate timelines |
| `git switch` | Move to another timeline |
| `git merge` | Combine timelines |
| `git push` | Upload commits to remote |
| `git pull` | Download + merge remote commits |
| `git log` | Browse snapshot history |
