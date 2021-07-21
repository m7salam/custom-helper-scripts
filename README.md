# custom-helper-scripts
helpful scripts I found that it helps my dev env and productivity


# git custom commands guide

must follow this steps before using the files

```bash
mkdir ~/bin
cd ~/bin
echo export PATH="$HOME/bin:$PATH" >> ~/.zshrc # use .bashrc if u are using bash
source ~/.zshrc

```

## download the file with this command
```bash

curl https://raw.githubusercontent.com/m7salam/custom-helper-scripts/master/git-custom-commands/git-add-safe -o git-add-safe

``` 

make sure its excutable by doing

```bash
sudo chmod u+x git-add-safe

```

same thing for the other commands
