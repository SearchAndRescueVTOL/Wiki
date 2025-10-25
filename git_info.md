## Best Practices for Working with Git

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
ssh -T [git@github.com](mailto:git@github.com)
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

add in this tutorial how to do pull requests in github

Here’s the updated Git best practices tutorial including a detailed explanation of **how to create a pull request (PR) on GitHub**, integrated seamlessly into your workflow.

***

# Best Practices for Working with Git

## Setting Up Git with SSH

To authenticate with GitHub securely, we use SSH keys instead of passwords.

### 1. Generate an SSH Key

```sh
ssh-keygen -t ed25519 -C "your-email@example.com"
```

This creates a key pair, typically stored at `~/.ssh/id_ed25519` (private key) and `~/.ssh/id_ed25519.pub` (public key).[^1]

### 2. Add the SSH Key to GitHub

Copy the public key and add it to GitHub:

```sh
cat ~/.ssh/id_ed25519.pub
```

Go to **GitHub → Settings → SSH and GPG keys → New SSH Key**, paste the key, and save it.[^1]

### 3. Test the SSH Connection

```sh
ssh -T git@github.com
```

If successful, GitHub will respond with:

```
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```


***

## What to Check Before Running `git add`

Before staging files:

1. View changes:

```sh
git status
```

2. Review differences:

```sh
git diff file.txt
```

3. Stage only needed files:

```sh
git add file.txt
```

4. Avoid `git add .` to prevent staging unwanted files.[^2]

***

## Working with Git Branches

### 1. Use a Clear Branching Strategy

- `main` → production-ready branch
- `develop` → ongoing development
- `feature/xyz` → new features
- `fix/xyz` → bug fixes[^2]


### 2. Create a New Branch

```sh
git switch -c feature/new-feature
```


### 3. Switch Branches

```sh
git switch main
```


***

## Making Pull Requests on GitHub

A **pull request (PR)** lets you propose and collaborate on changes to a remote repository before merging them into a base branch like `main`.[^3][^4]

### 1. Push Your Branch to GitHub

After committing locally:

```sh
git push origin feature/new-feature
```


### 2. Create a Pull Request

1. Go to your repository on **GitHub.com**.
2. You’ll see a **Compare \& pull request** banner at the top. Click it.
Alternatively, go to the **Pull Requests** tab and click **New Pull Request**.
3. Ensure:
    - The **base branch** is set to `main` (or `develop`).
    - The **compare branch** is your feature branch (`feature/new-feature`).
4. Add a clear **title** and detailed **description** for your pull request.
    - Explain what the change does.
    - Mention any related issues (e.g., *Fixes \#42*).
5. Click **Create Pull Request** or optionally **Create Draft Pull Request** if it’s not ready for review.[^4][^1]

### 3. Request and Manage Reviews

After creating the PR:

- Assign **reviewers** using the right sidebar.
- Reviewers can **comment**, **approve**, or **request changes**.[^5][^4]
- You can also discuss changes directly in the **Files Changed** tab.


### 4. Merge the Pull Request

Once the PR is approved:

1. Click **Merge Pull Request** and confirm.
2. Delete the branch:

```sh
git branch -d feature/new-feature
git push origin --delete feature/new-feature
```

This keeps your branch list clean.[^3][^1]

***

## Understanding `git push`

`git push` uploads commits to the remote branch. Before pushing:

```sh
git commit
git push origin main
```

This syncs local changes with GitHub.[^1]

***

## Writing Good Commit Messages

Write meaningful, structured commit messages:

```sh
<short summary>

<optional detailed explanation>
```

Avoid vague ones like “fixed stuff” or “update code”.[^2]

***

## Avoiding Large Files in Git

Large files make repositories slow. Use `.gitignore`:

```sh
*.log
*.csv
*.mp4
*.zip
dist/
build/
__pycache__/
```

Commit and push the `.gitignore` file:

```sh
git add .gitignore
git commit -m "Add .gitignore"
git push origin main
```


***

## Fixing a Repository with Large Files

To clean large files from history:

1. Identify large files:

```sh
git rev-list --objects --all | grep -E '\.csv$|\.mp4$'
```

2. Remove them:

```sh
git filter-repo --path path/to/largefile.csv --invert-paths --force
```

3. Force push clean history:

```sh
git push origin --force --all
```


***

## Summary of Best Practices

✅ Use SSH for secure Git authentication.
✅ Always check `git status` and `git diff` before staging.
✅ Avoid `git add .` — stage files individually.
✅ Use `git switch` for branches.
✅ Create pull requests in GitHub for proper code review.
✅ Write meaningful commit messages.
✅ Avoid large files; add them to `.gitignore`.
✅ Use `git filter-repo` to clean unwanted history.
