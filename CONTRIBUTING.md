# Contributing to Konine

Go to https://github.com/hospicedev/konine.dev — in the top right there's a button that says Fork. Click there to clone the repo. That will copy the repo to your GitHub user, e.g., https://github.com/yourusername/konine.dev

Clone your project locally:
```
git clone https://github.com/yourusername/konine.dev .
cd konine.dev
```

Ready!

## How to commit
If you have made modifications to the code:

```
git status # to see what's going on
git commit -a -m 'message here, this will commit the changes on the tracked files'
git push origin master # will "upload" the changes to your repo
```

Tricks:
```
git add . # will add all the files, even new ones
git add -u # will add all the tracked files even the deleted ones
git commit -a -m 'working closed etc  #725' # this will commit and mention an issue in the repo
```

## Pull Requests
Now you have new code at your fork, e.g., https://github.com/yourusername/konine.dev.
To move them to the original https://github.com/hospicedev/konine.dev repo you need to go to
https://github.com/yourusername/konine.dev, and click on Pull Request (next to compare). This will create a pull request to the original code and the responsible will decide to merge it or not.

Notes:
- Try to submit pull requests against master branch
- Try not to pollute your pull request with unintended changes — keep them simple and small

## Keep sync with original repo
First time, add a remote with the upstream:
```
git remote add upstream https://github.com/hospicedev/konine.dev.git
```

Every time you want to sync:
```
git fetch upstream
git merge upstream/master
```
