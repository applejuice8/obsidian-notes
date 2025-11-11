![Git](https://cdn.svglogos.dev/logos/git-icon.svg "Git")

---

[![Git, GitHub, & Workflow Fundamentals - DEV Community](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fvpxeexqyfvf4hw3zxtbn.png)

---

# git config
```bash
git config --global user.name Jason
git config --global user.email jason123@gmail.com
git config --global init.defaultBranch main
```

---

# git init
```bash
cd myFolder
git init
```

---

# git status
```bash
git status
git status -s

'''
M  file1.js  # Left M (All changes in staging area)
MM file1.js  # Right M (Changes not in staging area)

?? file2.js  # File not in staging area
A  file2.js  # Added new file
'''
```

---

# git add
- Add file to staging area (Ask git to track changes in these files)
- After every change, need to add again
- Even after remove file, need to add
```bash
git add file1.txt file2.txt
git add *.txt
git add .  # Add all
```

---

# git commit
- Commit files in staging area
```bash
git commit -m 'Initial commit'

git commit  # Opens default editor to type longer messages

git commit -am 'Fixed bugs'  # Skip staging area, add and commit all files
```

---

# git log
- View commit history
```bash
git log
# Space (Next page)
# q (Quit)
```
[![Git Log Cheatsheet For a Productive 2024 - Hatica](https://images.prismic.io/hatica/27290fcd-a24b-445e-b639-627031a9bcb7_log+1.png?auto=compress,format&rect=0,0,1312,1008&w=1200&h=922)

```bash
git log --oneline  # Short summary (1 line per commit)
git log --oneline --reverse  # Oldest to newest
```

---

# git checkout
- Move HEAD pointer to past commit (Read only)
- Switch branch
```bash
# Move HEAD pointer to past commit (Read only)
git checkout f284e0f5b7941443f98cc0608e791a6be4c973aa
git checkout main  # Go back
```

```bash
# Switch branch
git checkout main
git checkout feature/login
```

---

# git remote add
- Go to GitHub repo, click Code, copy link
- If project folder in local
```bash
git remote add origin https://github.com/applejuice8/portfolio.git
```

---

# git clone
- If project folder not in local
```bash
git clone https://github.com/applejuice8/portfolio.git
```

---

# git push
- Add files to GitHub
- Origin refers to the link added with `git remote add`
- Each branch need once `-u` tell Git local branch `main` to track remote `origin/main`
```bash
git push -u origin main
git push -u origin feature/login  # Push branch

git push  # Subsequent push
```

---

# git pull
- Fetch latest code on remote repository
```bash
git pull origin main

git pull  # Subsequent pull
```

---

# .gitignore
- Files that will not get push
```bash
code .gitignore
# or
echo "logs/" > .gitignore
```

```
logs/
.env
.DB_STORE
```

---

# git branch
- Branch will inherit code depending on where you are when you run the command
```bash
git branch feature/login  # Create branch
git checkout feature/login  # Move to branch
git checkout -b feature/login  # 2 in 1

git branch  # See at which branch

git branch new-branch source-branch  # Explicitly name source branch

git branch -M main  # Name current branch as main

git branch -d feature/login  # Delete branch
```

---

# Merge Branch
## 1. Pull Request
- Merge on remote repository (GitHub)
- After create new branch, can go GitHub click `Create pull request`
- Team lead can review and merge pull request
- If want to view the merged code, need `git pull`

## 2. git merge
```bash
git checkout main
git merge feature/login
```

---

# Resolve Merge Conflicts
```bash
git checkout main
git pull
```

---

# git ls-files
```bash
# List files in staging area
.gitignore
file1.js
```

---

# git rm

```bash
rm  # Remove from directory but still in staging area

git rm file1.txt file2.txt  # Remove from directory and staging area

git rm -cached file1.txt file2.txt  # Only remove from staging area
git rm -cached -r myFolder  # Recursive for folders
```

---

# git mv
```bash
git mv file1.txt main.js  # Rename file1.txt to main.js

# If normal Linux command
mv file1.txt main.js
git add file1.txt main.js
```
