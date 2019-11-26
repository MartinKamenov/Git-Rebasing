# How to git rebase?

## Commonly asked questions
### What is git rebase and why do we need it?
- Git rebase command changes the staring point(rebases) the place that our branch starts. This means that if master branch has changed we can stack our commits which are in our branch on top of the current last commit of the master branch.

### How do we benefit from this?
- By rebasing we create copy of each commit that we have made in our branch. This means we can make small and big fixes to our commits, like squashing two or more commits in one, split multiple commit to multiple more descriptive commits, change commits names and so on.
<img alt="git rebase" src="https://cdn-media-1.freecodecamp.org/images/aEjZMJ6s4rDVqzXveqgLrwkQ0RJEvOTjAIUc" width="400">

## Usefull commands
### Update master branch
1. `git checkout master` - First we should go to master branch
2. `git fetch --prune` - Update all branches and remove the once that are removed
3. `git pull --rebase` - Pull latest changes on master
4. `git checkout -` or `git checkout {our branch name}` - Return on our current branch

### Simple rebase with master branch
- <img alt="Rebase info" src="https://cdn-images-1.medium.com/max/2404/1*p0EGOtTUhzpUnF5p2c2UAw.png" width="400">
1. [Update master branch](https://github.com/MartinKamenov/Git-Rebasing/edit/rebase/read-me/README.md?pr=%2FMartinKamenov%2FGit-Rebasing%2Fpull%2F1)
2. `git rebase -i master`
3. When we submit this command a text editor will open showing us the commits that we have currently on our branch, which should look like this:
  ```
  pick 495b784 Update read-me content
  pick 48as867 Some other commit

  # Rebase cad7910..495b784 onto cad7910 (1 command)
  #
  # Commands:
  # p, pick = use commit
  # r, reword = use commit, but edit the commit message
  # e, edit = use commit, but stop for amending
  # s, squash = use commit, but meld into previous commit
  # f, fixup = like "squash", but discard this commit's log message
  # x, exec = run command (the rest of the line) using shell
  # d, drop = remove commit
  ```
  For simple rebase without changing the content and commit names we leave the code editor leaving it as it is.
  If you are using VIM the save the content as it is use:
  - `ctr + c` - for escaping
  - `x` - for saving the content

  After we escape the editor we should get that the rebasing is complete.
4. `git push --force-with-lease` for updating the branch history on the git server(remotely).

### Rename commit message
- <img alt="rename commit" src="https://linuxize.com/post/change-git-commit-message/featured.jpg" width="400">
1. [Update master branch](https://github.com/MartinKamenov/Git-Rebasing/edit/rebase/read-me/README.md?pr=%2FMartinKamenov%2FGit-Rebasing%2Fpull%2F1)
2. `git rebase -i master`
3. When we submit this command a text editor will open showing us the commits that we have currently on our branch, which should look like this:
  ```
  pick 495b784 Update read-me content
  pick 48as867 Some other commit
  ...
  ```
  we change it to this
   ```
  reword 495b784 Update read-me content
  pick 48as867 Some other commit
  ...
  ```
  We can change the commit name by changing the word `pick` before the commit name with `r` or `rename`.
  After we save the content that we have just changed, for each commit that we want to rename the text editor will show us current commit message and we will be able to change it.
  ```
   Update read-me content (Update the commit name by changing "Update read-me content" with our new commit message)
  # Please enter the commit message for your changes. Lines starting
  # with '#' will be ignored, and an empty message aborts the commit.
  #
  # Date:      Tue Nov 26 14:11:10 2019 +0200
  #
  # interactive rebase in progress; onto cad7910
  ```
  
4. `git push --force-with-lease`
  
### Squash multiple commits into one
  - <img alt="git squash" src="https://backlog.com/app/themes/backlog-child/assets/img/guides/git/basics/rewriting_history_005.png" width="400">
1. [Update master branch](https://github.com/MartinKamenov/Git-Rebasing/edit/rebase/read-me/README.md?pr=%2FMartinKamenov%2FGit-Rebasing%2Fpull%2F1)
2. `git rebase -i master`
3. When we submit this command a text editor will open showing us the commits that we have currently on our branch, which should look like this:
  ```
  pick 495b784 Update read-me content
  pick 48as867 Some other commit
  ...
  ```
  we change it to this
   ```
  pick 495b784 Update read-me content
  squash 48as867 Some other commit
  ...
  ```
  We can squash the one commit to another by changing the word `pick` before the commit name with `s` or `squash`.
  By changing the `48as867` commit command to `squash` we are squashing this commit to the previous one(`495b784`)
  After we save the content the text editor will open indicating that we want to squash multiple commits into one:
  ```
  # This is a combination of 2 commits.
  # This is the 1st commit message:

  Update read-me content

  # This is the commit message #2:

  Some other commit
  ```
  We save it as it is and the rebasing is complete

4. `git push --force-with-lease`

### Create *fixup to already pushed commit - *fixup is like a commit that only fixes small portion of code and is not tracked in the history of the branch
1. [Update master branch](https://github.com/MartinKamenov/Git-Rebasing/edit/rebase/read-me/README.md?pr=%2FMartinKamenov%2FGit-Rebasing%2Fpull%2F1)
2. After we have created the changes locally add them by using `git add .`
3. Get the commit hash to which you wish to add the fixup by using `git log --oneline`
```
d733b66 Update read-me content
cad7910 Initial commit
```

In this case the hash would be `d733b66`
- <img alt="git hash" src="https://victoria.dev/blog/git-commit-practices-your-future-self-will-thank-you-for/cover_git-commit-art.png" width="400">

4. `git commit --fixup=d733b66` - we commit the changes directly to the specific commit.
5. `git rebase -i --autosquash d733b66~1` - we autosquash the fixup to the specific commit.
6. `git push --force-with-lease` - Push updated commit to the server

### Swap commit places in branch history
- <img alt="Swap icon" src="https://static.thenounproject.com/png/340572-200.png" width="400">
1. [Update master branch](https://github.com/MartinKamenov/Git-Rebasing/edit/rebase/read-me/README.md?pr=%2FMartinKamenov%2FGit-Rebasing%2Fpull%2F1)
2. After we have created the changes locally add them by using `git add .`
3. Get the commit hash to which you wish to add the fixup by using `git log --oneline`
 ```
  pick 495b784 Update read-me content
  pick 48as867 Some other commit
  ...
  ```
  we change it to this
   ```
  pick 48as867 Some other commit
  pick 495b784 Update read-me content
  ...
  ```
  We simply rearange commit places in the editor and save the changes in the text editor

4. `git push --force-with-lease` - Push updated commit to the server

### Fetch changes that were made on branch with rebase
- Let's say we have checkout a collaborator's branch and then he/she has used `git rebase` and `git pull` and `git pull --rebase` show that we need to merge conflicts on our files
1. `git status` - fetch how many commits our current branch is ahead and behind the branch we wish to pull locally
2. `git reset --hard HEAD~{number of commits the branch is ahead}` - We undo the commits that are on the branch and set the HEAD to be the base commit
3. `git pull` - Then we pull the changes after the rebasing is done and we are on to date with the branch



## Contributors
| Team member         | Email                       | Username          |    Tasks                                              |
| ------------        | -------                     | :------:          | -------------------------                             |
| Martin Kamenov      | martinkamenov10@gmail.com   | MartinKamenov     | Content provider                                                |

## Additional sources
- [Git Rebase Interactive](https://thoughtbot.com/blog/git-interactive-rebase-squash-amend-rewriting-history)
