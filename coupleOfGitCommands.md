# Git and associated commands

## Couple of beginner Git commands

1. Bring a somewhere else hosted repo into a folder on local machine:
    
        $ git clone "url"

2. See what the status is (untracked files etc.):

        $ git status

3. Track files and changes in Git:

        $ git add (.)/(file)

4. Save files in Git:

        $ git commit (-S: sign) (-am: add and give title, works only if modified not for new files) "title" -m "description"

5. Combines git add + git commit:

        $ commit -a

6. Upload Git commits to remote repo (origin = location; -u = is shorthand for --set-upstream to set the default remote repo and branch, after this it suffices to use "git push"):

        $ git push (-u) origin "branch"

7. Initialize a git repo. After this the remote repo has to be instantiated, easiest way is via github, then take https/ssh address and use "git remote...":

        $ git init

8. Sets where to push the repo to:

        $ git remote add origin "https/ssh"

9. Prints connected remote repo's to the local repo:

        $ git remote -v

10. Prints the branches and marks active/current branch:

        $ git branch

11. Switches branch to "branch":

        $ git checkout "branch"

12. Create a new branch:

        $ git checkout -b "branch"

13. Show differences of current branch with branch:

        $ git diff (file/branch)

13. Merges branches:

        $ git merge

14. Download changes from remote repo to local machine, opposite of push:

        $ git pull (origin) "branch"

15. Deletes "branch":

        $ git branch -d "branch"

16. Unstages the file after add command:

        $ git restore --staged "file"

17. Go back one further to undo the commit and staging:

        $ git reset HEAD~1

18. Brings up the commit log:

        $ git log

19. Unstaging till the commit with "commit hash":

        $ git reset "commit hash"

20. Undoing all the changes until "commit hash" (files are changed!):

        $ git reset -hard "commit hash"

21. Prints files of branch:

        $ git ls-tree "branch"

22. Making a PR from CLI:

        $ git request-pull [-p] <start> <url> [<end>]
    
    this is issued from within the branch that should be pulled into starting point/branch.

23. Compare the current active (not master) branch to the masin:

        $ git request-pull main ./

## Git config stuff

1. Prints Git version:

        $ git --version

2. Prints email:

        $ git config --global user.email

3. Sets email:

        git config --global user.email "email"

4. Sets default branch to main within Git (for creation):

        $ git config --global init.defaultBranch main

5. Show config list:

        $ git config --list

6. Show url of origin:

        $ git config --get remote.origin.url

## Workflows

* Github:
    
    Write Code -> Commit Changes (optional: make pull request)

* Git:

    Write Code -> Save Changes (git add) -> Commit Changes (git commit) -> Push Changes (git push) -> Make Pull Request

## Common branches

* main
* feature-#-issue
* bug-fix
* hot-fix

## GPG commands

1. This is to sign commits so that they appear as "verified" when pushed.

        $ gpg --armor --export "key"

2. Sets GPG key:

        $ git config --global user.signingkey "key"

3. Prints key:

        $ git config --global user.signingkey

4. Prints key fill info:

        $ gpg --list-secret-keys --keyid-format LONG

5. To edit the key:

        $ gpg --edit-key "key"
## SSH on win 10

### Checking for existing SSH keys

1. Open Git Bash and enter:
        
        $ ls -la ~/.ssh
    
    to list the files in .ssh dir
    
2. check if public (*.pub) exist, if not proceed to next item to generate a key pair

### Generating a new SSH key

1. Generate the keys with email used to sign in Github:

        $ ssh-keygen -t ed25519 -C "your_email@example.com"

2. When prompted hit "Enter" to accept default or give "path+filename":

        $ Enter a file in which to save the key (/c/Users/*you*/.ssh/id_ed25519): /c/Users/*you*/.ssh/filename

3. Then enter password and confirm when prompted.

### Add SSH key to the ssh-agent

1. Ensure that ssh-agent is running:

        $ eval `ssh-agent -s`

2. Add SSH private key to ssh-agent:

        $ ssh-add ~/.ssh/*your_private_key*

    This is for current session only.

### Add SSH key to Github account

1. Copy SSH public key to clipboard:

        $ clip ~/.ssh/*your_public_key*

2. Navigate to Github and enter it under settings.


### Auto-launching ssh-agent on Git for Windows

1. Check if there's a .bashrc file:
    
        $ ls -la ~/.bashrc

2. If not, then create a .bashrc file:

        $ cp > ~/.bashrc

3. Add some aliases:

        $ notepad ~/.bashrc

4. In notepad paste the following:

        # Aliases
        # alias alias_name="command_to_run"
        # Long format list
        alias ll="ls -la"
        # Print my public IP
        alias myip='curl ipinfo.io/ip'

5. Check if the addition works:

        $ source ~/.bashrc
        $ ll

6. Now add the auto-launching part to .bashrc:

        # Auto-launching ssh-agent
        env=~/.ssh/agent.env

        agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

        agent_start () {
            (umask 077; ssh-agent >| "$env")
            . "$env" >| /dev/null ; }

        agent_load_env

        # agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running
        agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

        if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
            agent_start
            ssh-add
        elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
            ssh-add
        fi

        unset env

7. Then reload:

        $ source ~/.bashrc
### Configuring the ssh-agent

1. Create config file if not exists:

        $ touch ~/.ssh/config

2. Edit the config file:

        $ notepad ~/.ssh/config

3. Add following to config file:

        IdentityFile ~/.ssh/*your private key*
        AddKeysToAgent yes

        Add SSH private key to ssh-agent (this should be done only once):
        ...$ ssh-add ~/.ssh/*your_private_key*