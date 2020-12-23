# Git and associated commands

## Couple of beginner Git commands

git clone "url"
...Bring a somewhere else hosted repo  into a folder on local machine.

git status
...See what the status is (untracked files etc.).

git add (.)/(file)
...Track files and changes in Git.

git commit (-S: sign) (-am: add and give title, works only if modified not for new files) "title" -m "description"
...Save files in Git.

commit -a
...Combines git add + git commit.

git push (-u) origin "branch"
...Upload Git commits to remote repo (origin = location; -u = is shorthand for --set-upstream to set the default remote repo and branch, after this it suffices to use "git push").

git init
...Initialize a git repo. After this the remote repo has to be instantiated, easiest way is via github, then take https/ssh address and use "git remote...".

git remote add origin "https/ssh"
...Sets where to push the repo to.

git remote -v
...Prints connected remote repo's to the local repo.

git branch
...Prints the branches and marks active/current branch

git checkout "branch"
...Switches branch to "branch"

git checkout -b "branch"
...Create a new branch

git diff (file/branch)
...Show differences of current branch with branch

git merge
...Merges branches

git pull (origin) "branch"
...Download changes from remote repo to local machine, opposite of push.

git branch -d "branch"
...Deletes "branch"

git restore --staged "file"
...Unstages the file after add command.

git reset HEAD~1
...Go back one further to undo the commit and staging.

git log
...Brings up the commit log

git reset "commit hash"
...Unstaging till the commit with "commit hash"

git reset -hard "commit hash"
...Undoing all the changes until "commit hash" (files are changed!)

git ls-tree "branch"
...Prints files of branch

## Git config stuff

git --version
...Prints Git version

git config --global user.email
...Prints email

git config --global user.email "email"
...Sets email

git config --global init.defaultBranch main
...

git config --list
...

git config --get remote.origin.url
...

## Workflows

Github: Write Code -> Commit Changes (optional: make pull request)

Git: Write Code -> Save Changes (git add) -> Commit Changes (git commit) -> Push Changes (git push) -> Make Pull Request

## Branches

main, feature-#-issue, bug-fix, hot-fix

## GPG commands

gpg --armor --export "key"

git config --global user.signingkey "key"
...Sets GPG key

git config --global user.signingkey
...Prints key

gpg --list-secret-keys --keyid-format LONG

gpg --edit-key "key"

