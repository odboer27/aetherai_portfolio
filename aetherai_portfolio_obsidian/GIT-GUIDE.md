# Working on this site with Git & GitHub

Run these commands in a terminal opened in the project folder.

## The big picture

```
 your PC (local repo)                      GitHub (remote "origin")
 ─────────────────────                     ─────────────────────────
 edit files
   │  git add      → stage changes
   │  git commit   → save a snapshot locally
   │  git push     ───────────────────────►  branch updated on GitHub
   │                                          │
   │                                          │  Pull Request: feature → main
   │                                          │  review → Merge
   │  git pull     ◄───────────────────────  main now has your change
```

- **Repository (repo):** the project folder plus its full history.
- **Commit:** a saved snapshot with a message describing what changed.
- **Branch:** a separate line of work. `main` is the "official" version; you do new work on other branches so `main` always stays working.
- **Remote / origin:** the copy of the repo on GitHub.
- **Pull request (PR):** a request on GitHub to merge one branch into another, with a place to review the changes first.

---

## 1. Create the repository on GitHub (once)

1. Go to https://github.com/new
2. Repository name: `aetherai` (it must match the remote already set in this folder)
3. Choose Public (needed for free GitHub Pages) or Private
4. Do **not** tick "Add a README", ".gitignore" or "license" — this folder already has them
5. Click **Create repository**

Check that this folder points at it:

```bash
git remote -v
```

You should see `origin  https://github.com/odboer27/aetherai.git`.
(If a folder has no remote yet, you'd add it with `git remote add origin <url>`.)

## 2. First commit and push to `main` (once)

```bash
git add .
```
```bash
git commit -m "Initial portfolio page"
```
```bash
git push -u origin main
```

`-u` links your local `main` to GitHub's `main`, so later a plain `git push` / `git pull` is enough.
The first push opens a browser window to sign in to GitHub (Git Credential Manager).

## 3. Branches — work without touching `main`

Create a branch and switch to it:

```bash
git switch -c feature/real-content
```

Useful commands:

```bash
git branch
```
(lists branches; `*` marks the one you're on)

```bash
git switch main
```
(go back to main — your files change to match that branch)

Publish the branch to GitHub:

```bash
git push -u origin feature/real-content
```

Naming tip: `feature/...` for new things, `fix/...` for bug fixes, `content/...` for text updates.

## 4. Copying (cloning) the repository to a computer

On a new PC, or to start from scratch:

```bash
git clone https://github.com/odboer27/aetherai.git
```
```bash
cd aetherai
```

You get `main`. To work on an existing branch from GitHub:

```bash
git switch feature/real-content
```

(Git automatically creates a local branch tracking the remote one.)

## 5. The everyday loop: change → commit → push

```bash
git pull
```
(start by fetching the latest changes)

Edit `index.html` / `style.css`, then:

```bash
git status
```
(which files changed?)

```bash
git diff
```
(what exactly changed?)

```bash
git add index.html style.css
```
(stage the files you want in this commit; `git add .` stages everything)

```bash
git commit -m "Add real project descriptions"
```

```bash
git push
```

Good commit messages say *what* and ideally *why*, in the present tense: "Add contact links", "Fix timeline on phones".
Commit small and often — each commit is a point you can return to.

## 6. Pull requests — merging your branch into `main`

1. After `git push` on your branch, open https://github.com/odboer27/aetherai — a yellow banner offers **Compare & pull request**. (Or: **Pull requests** tab → **New pull request**.)
2. Set **base: `main`** ← **compare: `feature/real-content`**.
3. Write a title and a short description of what changed and why.
4. Click **Create pull request**.
5. Review it:
   - **Files changed** tab shows the diff line by line; you (or a collaborator) can comment on specific lines.
   - Need changes? Just commit and push to the same branch — the PR updates automatically.
   - If GitHub Pages / checks are set up, their status shows on the PR.
6. Click **Merge pull request** → **Confirm merge**, then **Delete branch** (on GitHub).
7. Bring your PC up to date and clean up:

```bash
git switch main
```
```bash
git pull
```
```bash
git branch -d feature/real-content
```

**Merge conflicts:** if `main` changed the same lines as your branch, GitHub says the branch has conflicts. Run `git switch feature/real-content`, then `git merge main`, open the files with `<<<<<<<` / `>>>>>>>` markers, keep the right text, then `git add` + `git commit` + `git push`.

## 7. Bonus — publish the site with GitHub Pages

1. Repo on GitHub → **Settings** → **Pages**
2. Source: **Deploy from a branch** → Branch: `main`, folder: `/ (root)` → **Save**
3. After a minute the site is live at https://odboer27.github.io/aetherai/

From then on every PR you merge into `main` updates the live site automatically —
which is exactly why you do your work on branches: `main` = what visitors see.

## Cheat sheet

| Goal | Command |
|---|---|
| What's changed? | `git status` / `git diff` |
| Save a snapshot | `git add <files>` then `git commit -m "msg"` |
| Upload commits | `git push` |
| Download changes | `git pull` |
| New branch | `git switch -c <name>` |
| Change branch | `git switch <name>` |
| History | `git log --oneline` |
| Undo unstaged edits to a file | `git restore <file>` |
