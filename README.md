# custom-helper-scripts
helpful scripts I found that it helps my dev env and productivity


## Index

1. [Download commands](#download-files) - Download the files quickly using curl. 
2. [git-addsafe](#git-addsafe) - Add all commited files without any new migrations generated and deletes the untracked migrations (Django specific). 
3. [git-deletemerged](#git-deletemerged) - Deletes all your already merged local branches to clean ur environment. 
4. [git-refresh](#git-refresh) - Refresh current local branch with any remote branch easily. 
5. [git-pushremote](#git-pushremote) - Push local branch changes to remote branch after updating from it.
6. [git-switch](#git-switch) (Deprecated) - Switch branches easily. Takes care of stashing changes and creating a new branch if required.
7. [Setup Cutom Commands](#setup) - Get started in 5 minutes max.
8. [Creating Custom Git Commands](#writing-custom-git-commands) - Have your own ideas? Create custom scripts.



## download-files 
```bash

curl https://raw.githubusercontent.com/m7salam/custom-helper-scripts/master/git-custom-commands/{command-file-name} -o {command-file-name}

# example:
curl https://raw.githubusercontent.com/m7salam/custom-helper-scripts/master/git-custom-commands/git-refresh -o git-refresh


``` 

make sure its excutable by doing


```bash
sudo chmod u+x {command-file-name}

```

## git-addsafe
This enables you to stage your edited files safetly without staging any migrations files so migrations can be only run on your specified branch as it needs to be consistant which in this case solves the migrations conflicts that face django developers while working with big teams .

Instead of:
```bash
git add .
git reset -- **/migrations/*.py
git clean -f
```
you can just do:
```bash
git addsafe
```

<b>Usage</b>
```
git addsafe
```


## git-deletemerged
This enables you to delete all merged branches from your local git

it will exclude (dev,master,staging,hotfix) and the current branch u are on with the * .


Instead of:
```
git branch --merged| egrep -v "(^\*|dev|master|staging|hotfix)" | xargs git branch -d
```
you can just do:
```
git deletemerged
```

<b>Usage</b>
```
git deletemerged
```

## git-refresh
This enables you to pull changes from a different remote branch to your local branch with just one command. 
You can write your own custom git commands to do whatever repetitive actions you do multiple times a day. 
For, me I usually update my local branch with the remote branch from which I branched off initially. 
Doing this would usually take multiple commands.

Instead of:
```
git stash
git checkout remote-branch
git pull --rebase origin remote-branch
git checkout current-branch
git rebase remote-branch
git stash apply
```
you can just do:
```
git refresh remote-branch-name
```

<b>Usage</b>
```
git refresh remote-branch-name
```
`remote-branch-name` is the remote branch from which you want to pull changes. 

## git-pushremote

git-pushremote enables you to push your changes to remote origin to the same branch name as local. It also checks if the remote branch exists or not. If it exists, it updates your local branch with remote before pushesing the commits.

Instead of:
```
git stash
git pull --rebase origin current-branch
git push origin current-branch
git stash apply
```
you can just do:
```
git pushremote
```

<b>Usage</b>

While you are on your current local branch you want to push, do:
```
git pushremote
```

If you want to force push just use
```
git pushremote -f
```

## git-switch (Deprecated)

github has introduced a git switch command starting v2.23

you can read more about it here
https://bluecast.tech/blog/git-switch-branch/

so can simply use 

```
git switch {branch}

# or 

git switch -c {new_branch}

```

git-switch enables you to switch to a new or existing branch easliy. You don't need to worry about stashing changes or looking into if the branch already exist or not. It will create the branch if it does not exists, otherwise will just switch to the banch and apply the stashed changes.

Instead of:
```
git stash
git checkout old-branch (or git checkout -b new-branch)
git stash apply
```
you can just do:
```
git switch checkout-branch-name
```

<b>Usage</b>

Just provide the branch name to which you want to switch as follows:
```
git switch checkout-branch-name
```

## Setup

1. Put the `git-refresh` and `git-pushremote` files anywhere on your system in a folder. Lets say you've put in the folder named `bin` in your home directory, so the folder path is `/home/username/path/bin/`
2. Add the directory path to your environment `PATH`. For Linux/Mac, you can edit your `.zshrc` by doing `vim ~/.zshrc` # use .bashrc if u are using bash. Add following line in the file in the beginning:
   
   ```
   #!/bin/sh
   # For git commands
   export PATH=$PATH:/home/{your-username}/bin
   # Other existing export statements.
   # End of file
    ```
   Now after saving the file, do the following on the terminal:
   
   ```
   source ~/.zshrc
   ```
3. That's it. You are done. You should be able run the commands. 


## Writing custom git commands

Lets say you want to make a command `git awesome` which takes one parameter and then calls series of git commands. 

1. Create a file named `git-awesome` in a folder somewhere.
2. Add that folder path to your environment `PATH` as shown previously, if not already.
3. Have the following inside the file:

   ```
   #!/bin/sh

   # Check if params are sufficient enough to go ahead.
   # P.S: This command takes one parameter so check if you have the param.
   parameter=$1
   test -z $parameter && echo "ERROR: Please provide the param." 1>&2 && exit 1

   # Find which is your current branch
   if currentBranch=$(git symbolic-ref --short -q HEAD)
   then
     echo On branch $currentBranch
     echo Doing work using $parameter ...
     # Write your git commands here
     echo Success!
   else
     echo ERROR: Cannot find the current branch!
   fi
   ```

