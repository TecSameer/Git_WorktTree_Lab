# Git WorkTree Lab

Hands-on lab session for git worktree. By the end, you will fully understand git worktree, why it exists, when to use it, and how to safely run parallel work (features + hotfix) without committing or pushing your current work.

## 🧠 What problem does git worktree solve?
### Real‑world pain without worktree

You are:
* Working on Branch 1 (feature-A)
* Have half-done changes
* Suddenly:
    * 🔥 Production hotfix needed
    * ✨ New feature requested

Normally you’d:
* Stash changes ❌
* Commit dirty WIP ❌
* Or clone repo again ❌

✅ git worktree gives you:

* Multiple working directories
* Same repository
* Different branches
* No stashing, no committing, no recloning

## 🧱 Repository Setup (Starting Point)
### 1️⃣ Clone repository

```bash
git clone https://github.com/TecSameer/Git_WorkTree_Lab.git
cd .\Git_WorkTree_Lab\
```

### 2️⃣ Check current branch
```bash
git branch
```

## 🌿 Step 1: Create Branch and Push to GitHub
### 3️⃣ Create new branch and make some changes
```bash
echo "Creating new file app.txt" > app.txt
git add .\app.txt
git checkout -b features/feature-1
git commit -m "Added app.txt file in features/feature-1 branch"
```
#### Explanation:
* *echo* → creates new file & writes content.
* *git add* → stages new file.
* *got checkout* → creates new branch named feature-1 in features folder.
* *git commit* → commits the changes to git

### 4️⃣ Push branch to GitHub
```bash
git push -u origin features/feature-1
```
Once changes are pushed to branch, create a PR in github and merge it with main branch.

## 🛠 Step 2: Start Working parallelly on second branch (NO COMMIT)
### 5️⃣ Create second branch and make some changes
```bash
git checkout -b features/feature-2
echo "Adding new file for features/feature-2 branch" > feature-2.txt
echo "Adding new file for features/feature-2 branch. Work in progress" > feature-2.1.txt
git add .\feature-2.txt
```
#### 🚨 Important
* Changes are uncommitted
* You must NOT commit or push

## 🌳 Step 3: Create Worktree for Parallel Feature
Now we need **another branch**, but:
* Don’t disturb current work
* Keep dirty changes intact

### 6️⃣ Create new worktree for parallel feature
```bash
git worktree add -b features/feature-3 ../worktrees/features
```
#### Explanation:
* *git worktree add* → Creates new working directory.
* *-b features/feature-3* → Branch name
* *../worktrees/features* → Path to new folder.

✅ Now you have:
* Git_WorkTree_Lab/ → features/feature-2 (dirty)
* worktrees/features/ → features/feature-3 (clean)

### 7️⃣ Enter parallel feature directory
```bash
cd ..\worktrees\features\
```
#### 🔍 Verify isolation
```bash
git branch
```
#### And:
```bash
git status
```
#### Output:
```bash
nothing to commit, working tree clean
````

#### 🎯 Your feature‑branch‑1 changes are completely untouched
### 8️⃣ Do work in parallel feature
```bash
echo "Adding new file for features/feature-3 branch" > feature-3.txt
git add .\feature-3.txt
git commit -m "Added feature-3.txt file in features/feature-3 branch"
git push -u origin features/feature-3
```
Create a PR in github for the commit and merge it with main branch.

Now, you've 2 commits in main branch. One is of feature-1 and second is of feature-3. 

## 🚑 Step 4: Create Production Hotfix Worktree

### 9️⃣ Create hotfix worktree
```bash
git worktree add -b hotfixes/hotfix-1 ../worktrees/hotfix main
```
#### Explanation:
* *git worktree add* → Creates new working directory.
* *-b hotfixes/hotfix-1* → Branch name.
* *../worktrees/hotfix* → Path to new folder.
* *main* → Base branch

✅ Now you have:
* Git_WorkTree_Lab/ → features/feature-2 (dirty)
* worktrees/features/ → features/feature-3 (clean)
* worktrees/hotfix/ → hotfixes/hotfix-1 (clean)

### 1️⃣0️⃣ Enter hotfix directory
```bash
cd ..\worktrees\hotfix\
```

### 1️⃣1️⃣ Apply hotfix
```bash
echo "Adding new file for hotfixes/hotfix-1 branch" > hotfix-1.txt
git add .\hotfix-1.txt
git commit -m "Adding new file in hotfix worktree"
```

### 1️⃣2️⃣ Push hotfix to GitHub
```bash
git push -u origin hotfixes/hotfix-1
```

Create a PR in github for the commit and merge it with main branch.

🚀 Production fire resolved

✅ Still no commit on branch feature-2

## 🔙 Step 5: Return to feature branch 2 (Dirty State Preserved)
```bash
cd ..\..\Git_WorktTree_Lab\
git status
```
#### Output:
```bash
modified: feature-2.txt
```
✅ Your uncommitted work remains exactly as it was

## 📋 Step 6: Manage Worktrees
### 1️⃣3️⃣ List all worktrees
```bash
git worktree list
```
#### Output:
* Git_WorkTree_Lab/ → features/feature-2 (dirty)
* worktrees/features/ → features/feature-3 (clean)
* worktrees/hotfix/ → hotfixes/hotfix-1 (clean)
#### Why this matters
* Shows which branches are **locked**
* Prevents accidental deletion

## 🧹 Step 7: Remove Worktree (When Done)
### 1️⃣4️⃣ Delete parallel feature worktree
```bash
git worktree remove ../worktrees/hotfix
git worktree remove ../worktrees/features
```
#### Rules
* Must have:
    * No uncommitted changes
    * Branch can still exist

### 1️⃣5️⃣ Prune stale metadata
```bash
git worktree prune
```
#### Explanation:
* Cleans references of deleted folders
* Always safe to run
---

## 🚨 Important Rules of git worktree
```text
✅ One branch → one worktree
❌ Same branch cannot be checked out in two directories
✅ You can delete folders only via command
❌ Never manually delete worktree directory
✅ Dirty state is isolated per worktree
```

## 🏆 When to Use git worktree (Real Usage)
```text
✅ Parallel features
✅ Production hotfix
✅ Code reviews
✅ Release prep
✅ QA validations
✅ Large repos (saves disk space)
```

## 🧠 Mental Model (Remember This)
```tree
1 repository (.git)
│
├── worktree A → feature-branch-1 (dirty)
├── worktree B → parallel-feature (clean)
└── worktree C → hotfix/prod-issue (clean)
```

## ✅ You now know COMPLETE git worktree
**You learned:**
* ✅ Creating worktrees
* ✅ Working without committing
* ✅ Parallel branches
* ✅ Hotfix from main
* ✅ Cleanup & safety rules
* ✅ Real-world workflow