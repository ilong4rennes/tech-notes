![[Screen Shot 2024-01-17 at 00.06.52.png]]![[Screen Shot 2024-01-17 at 00.07.36.png]] 

Don't add untested features to main branch!

![[Pasted image 20240117082454.png]]
1. send files from our **working directory** to a **staging level**. 
2. once we have a collection of related files in staging we want to save to the repo, we 'commit' those files along with a useful message explaining what was committed. 
3. if we ever want a file or set of files back from the repository, we can do so using the 'checkout' process.

## Commands
- `git init` = create a new repo
- `touch {file}` = creates an empty file if a file does not exist. 
- `git add {file}` = tells git to stage the file or directory for a commit, if it exists.
- `git status` = displays the state of the working directory and the staging area
- `git commit -m "Message"` = Commit the files in staging to the repository; include a description message using the –m flag
- `git rm {file}` = remove file from the working directory
- `git checkout HEAD {file}` = get the deleted file back; `HEAD` is a pointer to your most recent commit. This could also be the SHA value of any previous commit.
- `git diff` = see changes
- `git diff --cached {file}` = see the difference between the staged version and the repository version, specify the --cached flag
- `git branch` = list all current branches; current branch marked with *
- `git branch {branchName}` = create new branch
- `git checkout {branchName}` = switch branch
- `git checkout -b {bName}` = created the branch and switched to it in one command by specifying the -b flag to git checkout.
- `git merge {bName}` = merge the branch to current branch
- `git branch -d {bName}` = delete the branch
- `git push -f`: Force a push that would otherwise be blocked, usually because it will delete or overwrite existing commits _(Use with caution!)_
- `git push -u origin [branch]`: Useful when pushing a new branch, this creates an upstream tracking branch with a lasting relationship to your local branch
- `git push --all`: Push all branches
- `git push --tags`: Publish tags that aren't yet in the remote repository