# GIT BASIC COMMANDS 

## Initial setup

• **git init** — Creates a brand new, empty local Git repository in the current folder.

• **git clone <url>** — Downloads an existing remote repository (from GitHub/GitLab) along with its entire version history to your machine.

• **git config** --global user.name "Your Name" — Sets the name tied to your commit signatures.

• **git config** --global user.email "you@example.com" — Sets the email address tied to your commit signatures.

## Track changes, save milestones, and view repository state.

• **git status** —Shows exactly which files are modified, untracked, or staged for the next commit.

• **git add** <file> — Moves changes from the working directory to the staging area (preparing them to be saved).

• **git add .** — Stages all modified and new files in the current directory.

• **git commit -m** "your message" — Permanently records a snapshot of your staged changes in the local history. Keep messages descriptive.

• **git log --oneline**— Displays a condensed, clean history of all past commits in chronological order.


### BRANCHING 
• **git branch** — Lists all local branches in the current repository. The active branch is highlighted with an asterisk (*).

• **git checkout -b <branch-name>** — Creates a brand new branch and instantly switches you over to it.

• **git switch <branch-name>**— Moves you from your current branch to an existing one.

• **git merge <branch-name>**— Combines the history of the specified branch into your currently active branch (e.g., run this on main to pull in a completed feature branch).

• **git branch -d <branch-name>** — Safely deletes a branch that has already been merged.


### Interacting with Remotes

• **git remote add origin <url>** — Links your local repository to a remote server path for the first time.

• **git fetch** — Pulls down the latest updates, branches, and tags from the remote repo without modifying your local working files.

• **git pull** — Fetches updates from the remote branch and instantly tries to merge them into your local branch. (Equivalent to git fetch followed by git merge).

• **git push origin <branch-name>** — Uploads your local commits up to the remote repository.
