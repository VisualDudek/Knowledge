# Git && GitHub

## Git

{% embed url="https://docs.gitbook.com/" %}

![](../.gitbook/assets/git.png)

{% embed url="https://ndpsoftware.com/git-cheatsheet.html" %}

### Edit last commit msg

`git commit --amend` add more changes to the last commit by staging before exec `--amend`

`-a` is not `--amend` first automatically "add" changes from all known files.

alias for zsh: `gc!` = `git commit -v --amend`

if you want to edit older commit msg:

```
git rebase -i HEAD~5
# (editor) replace pick with reword, save&quit
# for each reword new text editor will open
# if already pushed upstream, than U need to use force
git push --force <brand-name>
```

### Commit only part of code

`git add -p,--patch <filename>`

### Interactive Staging

`git add -i`

### Undo with git

{% embed url="https://docs.gitlab.com/ee/topics/git/numerous_undo_possibilities_in_git" %}

### Undo uncommitted changes

`git reset` this will unstage all files.

`git checkout [dir|file|.]` will revert local uncommitted changes, same as `git reset --hard HEAD`

### Undo the last commit

`git reset --soft HEAD~1`

`reset` copied the old head to `.git/ORIG_HEAD`

`commit` with `-c ORIG_HEAD` will open an editor, which initially contains the log message from the old commit and allows you to edit it.

```
$ git commit -m "Something terribly misguided" # (0: Your Accident)
$ git reset HEAD~                              # (1)
[ edit files as necessary ]                    # (2)
$ git add .                                    # (3)
$ git commit -c ORIG_HEAD                      # (4)
```

{% embed url="https://www.atlassian.com/git/tutorials/rewriting-history" %}

{% embed url="https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git" %}

### Git add and commit in one command

```
# use zsh alias gcam='git commit -a -m'

# use git alias
git config --global alias.add-commit '!git add -A && git commit'
#and use it with
git add-commit -m 'My commit message'
```

### Hooks

{% embed url="https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks" %}

### Merging vs. Rebase

{% embed url="https://www.atlassian.com/git/tutorials/merging-vs-rebasing" %}

### Merge specific file from another branch

```
git checkout src-branch path/to/file
```

### Renaming a file <a href="#renaming-a-file-using-the-command-line" id="renaming-a-file-using-the-command-line"></a>

```
git mv old_filename new_filename
git commit -m "Rename file"
git push origin your-branch
```

### Adding a local repository to GitHub with GitHub CLI <a href="#adding-a-local-repository-to-github-with-github-cli" id="adding-a-local-repository-to-github-with-github-cli"></a>

```
gh repo create
```

## GitHub

### Actions

YAML file `.github/workflows/*.yaml`

* U can re-run jobs

Links:

* Experimenting with Github Actions [link](https://seandavi.github.io/post/learning-github-actions/)
* Configuring a workflow [@GitHubDoc](https://help.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow#adding-a-workflow-status-badge-to-your-repository)

### Badge

Action dashboard -> choose workflow -> Create status badge paste it onto README.md

## gitlab/github/bitbucket

just repo in cloud

## Lab

1. Commit only part of code
2. Do staging interactive
