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

## SSH on win 10

### Checking for existing SSH keys

Open Git Bash

Enter:
...ls -la ~/.ssh
...\# lists the files in .ssh dir
...check if public (*.pub) exist, if not proceed to next item

### Generating a new SSH key

Generate the keys with email used to sign in Github:
...$ ssh-keygen -t ed25519 -C "your_email@example.com"

When prompted hit "Enter" to accept default or give "path+filename":
...$ Enter a file in which to save the key (/c/Users/*you*/.ssh/id_ed25519): /c/Users/*you*/.ssh/filename

Then enter password and confirm when prompted.

### Add SSH key to the ssh-agent

Ensure that ssh-agent is running:
...$ eval `ssh-agent -s`

Add SSH private key to ssh-agent:
...$ ssh-add ~/.ssh/*your_private_key*

### Add SSH key to Github account

Copy SSH public key to clipboard:
...$ clip ~/.ssh/*your_public_key*

Navigate to Github and enter it under settings.


### Auto-launching ssh-agent on Git for Windows

Check if there's a .baschrc file:
...$ ls -la ~/.bashrc

If not, then create a .bashrc file:
...$ cp > ~/.bashrc

Add some aliases:
...$ notepad ~/.bashrc

In notepad paste the following:
...\# Aliases
...\# alias alias_name="command_to_run"

...\# Long format list
...alias ll="ls -la"

...\# Print my public IP
...alias myip='curl ipinfo.io/ip'

Check if the addition works:
...$ source ~/.bashrc
...$ ll

Now add the auto-launching part:
...env=~/.ssh/agent.env

...agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

...agent_start () {
...    (umask 077; ssh-agent >| "$env")
...   . "$env" >| /dev/null ; }

...agent_load_env

...\# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running
...agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

...if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
...    agent_start
...    ssh-add
...elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
...    ssh-add
...fi

...unset env