# Best Practices for Working with Git

## Setting Up Git with SSH

To authenticate with GitHub securely, we use SSH keys instead of passwords.

### **1. Generate an SSH Key**

Run the following command to generate a new SSH key:

```sh
ssh-keygen -t ed25519 -C "your-email@example.com"
```

This creates a key pair, typically stored at `~/.ssh/id_ed25519` (private key) and `~/.ssh/id_ed25519.pub` (public key).

### **2. Add the SSH Key to GitHub**

Copy the public key and add it to GitHub:

```sh
cat ~/.ssh/id_ed25519.pub
```

Go to **GitHub → Settings → SSH and GPG keys → New SSH Key**, paste the key, and save it.

### **3. Test the SSH Connection**

To confirm that GitHub recognizes the key, run:

```sh
ssh -T git@github.com
```

If successful, GitHub will return a message like:

```sh
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## What to Check Before Running `git add`

Before staging files with `git add`, always:
1. **Check which files have changed**:
   ```sh
   git status
   ```
2. **Review changes before staging**:
   ```sh
   git diff file.txt
   ```
3. **Add only necessary files individually**:
   ```sh
   git add file.txt
   ```
4. **Never use `git add .`**, as it can accidentally stage unwanted changes.

---

## Working with Git Branches

### 1. **Use a Clear Branching Strategy**

We should follow a branching strategy such as Git Flow or GitHub Flow:

- `main` → Stable production-ready branch.
- `develop` → Ongoing development work.
- Feature branches (e.g., `feature/xyz`) for new functionality.
- Bug fix branches (e.g., `fix/xyz`) for patches.

### 2. **Create a New Branch**

```sh
git switch -c feature/new-feature
```

### 3. **Switch Between Branches**

To switch branches, use:

```sh
git switch main
```

### 4. **Merge Changes Using GitHub**

Instead of merging locally, we use GitHub’s pull request (merge request) feature:

1. Push your feature branch to GitHub:
   ```sh
   git push origin feature/new-feature
   ```
2. Go to the GitHub repository and create a **Pull Request**.
3. Request reviews and address any comments.
4. Once approved, click **Merge**.
5. Delete the feature branch both on GitHub and locally:
   ```sh
   git branch -d feature/new-feature
   git push origin --delete feature/new-feature
   ```

---

## Understanding `git push`

### **What `git push` Does**

`git push` uploads committed changes to the remote repository (e.g., GitHub). Before pushing, you need to commit the changes:

```sh
git commit
```

After writing a commit message in the editor that opens, save and exit, then push:

```sh
git push origin main
```

This sends the changes in the `main` branch to GitHub.

---

## Writing Good Commit Messages

A good commit message should be **clear, concise, and descriptive**. Follow this format:

```sh
<short summary>

<optional longer description>
```

When making a commit, Git will open a text editor for you to enter the message. Make sure to write meaningful descriptions.

Avoid vague commit messages like:
❌ `"Fixed stuff"`  
❌ `"Updated code"`  
❌ `"Bug fixes"`  

---

## Avoiding Large Files in Git

### Why Large Files Are a Problem

Adding large files to a Git repository can cause performance issues, slow down cloning, and increase repository size unnecessarily. Instead of committing large files directly, we should always put them on Git and avoid storing unnecessary data in the repository.

### Solutions for Handling Large Files

#### **Use `.gitignore` to Prevent Accidental Commits**

Before adding files, ensure that large files are ignored using a `.gitignore` file:

```sh
# Ignore large files
*.log
*.csv
*.txt
*.mp4
*.zip
*.tar.gz
# Ignore compiled files and cache
dist/
build/
*.o
*.so
*.exe
__pycache__/
*.pyc
*.pyo
```

Commit and push this change to prevent large files from being tracked:

```sh
git add .gitignore
git commit
```

After writing a commit message in the text editor that opens, save and exit. Then push:

```sh
git push origin main
```

---

## **Fixing a Repository That Has Large Files**

If we accidentally committed large files, we must **rewrite history**.

### **1. Find Large Files**

```sh
git rev-list --objects --all | grep -E '\.txt$|\.csv$'
```

### **2. Remove Large Files from History**

Using `git filter-repo` (Recommended):

```sh
git filter-repo --path path/to/largefile.csv --invert-paths --force
```

### **3. Force Push the Cleaned Repository**

```sh
git push origin --force --all
```

**Warning:** This rewrites history, requiring all collaborators to re-clone the repository.

---

## Summary of Best Practices

✅ Always put large files on Git and avoid unnecessary files in the repository.\
✅ Follow a structured Git branching strategy.\
✅ Use SSH keys for authentication, ensuring each user generates their own key.\
✅ Use `git switch` instead of `git checkout` for branch changes.\
✅ Use GitHub's Pull Request system for merging changes.\
✅ Write clear commit messages that describe what changed.\
✅ Always check `git status` and `git diff` before running `git add`.\
✅ Never use `git add .`, always stage files individually.\
✅ If large files were committed, clean the repository using `git filter-repo`.\

By following these best practices, we can ensure a clean, efficient, and well-managed Git workflow. 🚀

