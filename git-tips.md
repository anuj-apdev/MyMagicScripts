# Git Tips and tricks

## Seeing changes dif/show/log commands

### Help
- git help log -> to get help about log command.

### Variants of git log
- git log
- git log <filename/directoryName> -> Only chnages on that file or dir.
- git log <branchName> -> Shows log for that branch.
- git log -n 10 -> show only 10 recent commits
- got log -10 -> Same as above
- git log --since=2019-01-01
- git log --until=2019-01-01
- git log --until="3 days ago"
- git log --after="2.weeks" --before="3.days"
- git log --author="Anuj"
- git log --grep="Init" -> or use BugFix, ghi, Jira ID etc
- git log --oneline -> Shows details in single line for each commit.
- git log <Commit ID>..HEAD -> Everything after thet commit, untill head.
- git log SHA.1...SHA.2
- git log branch1..branch2
- git log mybranch..origin/mybranch --> To See changes between local and remote branch. 
  - This has to be done between fetch and merge. Otherwise pull is better option.
- git log -p -> See changes/patch as well.
- git log --stat -> Statisctics, how many changes in each file, per commit.
- git log --format=[oneline, short. medium, full, fuller, email, raw] -> different formating and details.
- git log --oneline -> this one even shortens hash.
- git log --graph -> Shows branch info as well.
- git log --graph --all --oneline --decorate -> Here all means show all branches.


## git show
- git show HEAD -> most recent commit. i.e. changes between commited version, with its older one.
- git show -p <SHA> -> will show patched version, i.e. changes as well.
- git diff -> unstaged changes i.e. difference between commited version v/s local changes.
- git diff --cached(or staged) -> diff between staged and commited version
- diff works with tree-ish objects, like files, dir with file and tree, commits and brnches.
- git diff <OldCommitSHA>...<Newer commit sha> --color-words
- git diff <OldCommitSHA>...HEAD --color-words
- git diff master..feature/1234 -> Shows diff between master branch and feature/1234 branch.

### Staging area -> best use of staging areas is to help make atomic commit
- Always make atomic commit. 
 - Only add Atomic changes to staging area once, 
 - and then commit those changes, 
 - and then come back and add more files for second commit.
 - If commits are atomic, it also helps as Reveting One commit at a time, easier.

## Undoing changes

### Undo changes in unstaged file (or untracked changes in tracked file) -- checkout
- git checkout -- <filepath> -> This `--` means don't go to different branch, do the checkout from current active branch. 
  - Checkout this file from current branch.( in other worlds; Undo my unstaged changes)

### Unstage any staged file
- git reset HEAD <FilePath> -> This one will unstage the staged changes, but you will need to do checkout to undo them in files.
- it will Also remove newly added files from git history. and make it untracked.
- Can be used to make commits atomic. If mistakenly you have added all files in staging are usimng `git add .`, you can unstage, those files which makes sence for different commit, for a while.

### Undo commited changes
- git doesn't allow to delete intermedite commits. Only last Commit can be amended.
- `git commit --ammend -m "Ammended message"`
 - Old commit will be gone (with its SHA), HEAD will get reset, and message will also get updated. New SHA will get generated.
 - Its similar to ammending, redoing last commit.

- `git checkout <commit-sha> -- <filename>`
 - it leaves the old commit version of file in staging area. You can merge that version or change it further.
 - Usually you my want to see changes of one commit before the commit you want to undo/see.
 - In either case, you will have to create a new commit.

- `git checkout -- <filename>` to move it back to original (last commited version) file.

## Revert commit
git revert <commit SHA> 
- - equivalent to commit

## Clean
- git clean -n -> Just tells what it would do. Doesn't really removes it. 
- git rm --cached <filename> -> This will delete/remove a tracked file from being tracked further. Without deletingn it from CWD.
 - - add to gitignore as well, to make this not sppering as untracked file.
 - - file will still exist in remote repo, but future changes will not be tracked.

### File Listing and tree
- `git ls-tree HEAD`
- `git le-tree HEAD <filename/directoryname>` -> A filter to only show the file/dir.
- `git ls-tree HEAD <directory>/` -> Show contentx insode the dir.(/ is used for that)
- .gitignore -> to untrack a file.
- .gitkeep -> to trck empty directory

---

## Tree-ish
A directory or something that points to a directory like
- A commit
- A refference/tags
- A branch

Or in other words 
- SHA1 Hash
- HEAD Pointer reference
- Branch reference
- Tag reference
- Ancestory

### Getting Ancestor commits
- `git show HEAD`
- `git show HEAD^ or HEAD^^`
- `git show <Commit SHA>^`
- master^, HEAD~, HEAD~3

# Branches

- Commit created after merge of a branch to master(or some other target branch) is called merge commit.

## Creation of branch and switcing between
- `git branch `-> LIsts all the branches.
- `git branch -r` -> LIst remote branches.
- `git branch feature_1234` -> creates. branch but doesn't switch to it.
- `git checkout branchname` -> Swithces to given branch
- `git checkout -b branchname` -> Creates and Swithces to given branch
- `git checkout -b branchname source_branch` -> if you dont want source to be your current branch
- `git checkout -b branch_name SHAID` -> To start a branch from given commit ID. New branch will not have later commits.
- `git diff master..feature/1234` -> dif between any 2 branches.
- `git branch --merged` -> Shows all the branches whose all commits are there in current branch
  - helps while deleting branches. Use current branch by default. And HEAD to start with.
  - Compare between develop and master, whether all changes of develop available in master.
  - Lists Branches whose tip are reachable from the specified commit (HEAD if not specified)
  - Branch tip is in the history of the specified commit.
- `git branch --no-merged` -> Opposite of bove. Showing branches which are not fully included in current branch.
- `git branch -r merged` -> r stands here for remote branch.
- `git branch --merged HEAD`
- `git branch --merged july_release`
 - Will start from the TIP of july_release, and work backwards down the history to find which of the branches have their TIP reachable in the hostory.

### Identity Merged branches

- `git branch --merged origin/july_release`
- `git branch --merged b26758d`  -> Start with this commit.
- `git branch -d branch_to_delete` -> Must switch to different branch first.
- `git branch -D branch_to_delete` -> Force delete, If branch has some unmerged commits. i.e. Branch to delete has some changes/commits that current active brnch doesn't have.
- `git push origin :new_feature` -> depeted remote branch
- `git push --delete origin new_feature` -> git 1.7>, same as above
- `git push -d origin new_feature` -> git 2.8>, same as above
- Deleting remote brnches deletes it's tracking branch for the user who initiated the delete.
- Other collaborators need to `prune`. `git fetch` will not prune automatically.

### Reset commits

#### --soft -> Resets only HEAD, not staging index or working tree.
 - `git reset --soft HEAD^` -> to undo lat commit
 - Can be done iteratively.
 - Can be rolled back by -> `git reset --soft <Old Head commit SHA>`
 - If a newe change is made, then we can't go back to old HEAD. That will be abandoned.
 - Changes will be thrown back to staging area.
 - Mostly used To Squash or skip commits 
 - Squash example -- `git reset --soft HEAD^^; git commit` 
 - This will move back 2 commits, and then create a new commmit with changes of both. Effecting a squasha. commit.

#### --medium -> Default option
 - It also removes changes from Staging area as well.
 - previous commits will be discarded, SO others cant see it.
 - Can go backward and forward. If you recall the older commit sha, and no commits in between rollback.
 - Use case -> **Split the commit into 2 commits.**
 - Also we can make changes to older commits. and then merge it back, or ignore it completely, or some files.

#### --hard -> Changes everything including working tree
 - Every changes will be gone.
 - SHort term rollback is possible. So if we have copied the SHA of older commit, we can go back to it in some short time.
 - In Long time, it will be garbage colleced, and cnnot be rolled back
 - Use Case -> **Used to reset to different branch**

### Merging Branches
- `git merge feature?1234` -- run this cmd from master to merge in master
- fast forward vs real merge
- `git merge-base master branchnme` -> Tells when these 2 branches diverge, or common meeting point.

#### stash and its usage 
- By default it works only on tracked files.
- `git stash save "Save or commit message"` -> To save changes temporarily while switching branches.
- `git stash list` -> to see stashed changes.
- `git stash show stash@{0}` -> to show a paeticular stash.
- `git stash -p stash@{0}` -> to show patch as well, i.e. changes in each file.
- Above commands will work across the branch. stash is not tied to any particular branch.
- `git stash pop` -> un-stashes latest stash, i.e. applies changes back to files.
- `git stash pop stash@{0}` -> Applies changes from given stash, and removes the stash.
- `git stash apply stash@{0}`-> Applies changes from given stash, and also keeps the stash, so that it could be applied to another branches as well.

- `git stash drop stash@{0}` -> removes stash permanently, without saving changes anywhere.
- `git stash clear` -> Clears entire stash


# Remote Repositories

- `git remote` -> Shows list of remotes
- `git remote add origin <URL>`
- `git remote -v` -> shows origins with fetch and push remotes.
- `git remote rm origin`
- `git remote set-url <URL of remote>`

## Remote Branches
- Original; branch on the remote repository. `bugfix-1234`
- Local Snapshot of remote branch, also called remote tracking branch `origin/bugfix-1234`
	- Helps to work offline, has all the metadata of remote.
	- Can be periodically uopdated using fetch.
	- Internet is only needed for push or pull operations.
- Local Branch, which is tracking the remote branch `bugfix-1234`

## Push and Pull -- Remote commands
- `git push -u origin master` -> origin is alias of remote, master is target branch to be pushed. -u is to set tracking branch.
- If tracking branch is set, No need to further specify remote branch name while push or pull, of any branch.
- `git branch -r` -> will show remote brnches
- `git branch -a` shows both, local and remote branches.
- `--set-upstream` and -u options are same, and they start tracking remote branch.
- `git branch -u <remote>/branch-name branch-name`
- `git branch -u origin/feature/1234 feature/1234`
- `git branch --unset-upstream feature/1234` stops tracking.
- `git diff origin/master..master` will show changes which are nit yet pushed. + not yet pulled.
- `git push origin master` -> Pushing everything from master(local) to origin i.e. alias for remote.

- `git fetch origin` or just `git fetch`
- `git log --oneline -5 origin/master` -> to show recent commits on remote, till the time of running fetch, and current state/commit and HEAD of existing branch.
- `git merge origin/master` -- bring those changes in to current branch.
- `git pull` = `git fetch` + `git merge`
- git pull --rebase (or -r) -> Rebase with pull, not merge.
- git pull --rebase=preserve
- git pull --rebase=interactive

If you have done changes and dont want to mess with changes/conflicts on remote branch, use fetch, and later use merge to deal with conflict. If you want to dela with everything right sway, or you dont have any local changes, then use git pull.

So far we have dealt with master branch. Lets play with other branches as well
- `git branch feature/1234 origin/feature/1234` -> Create a new branch from remote given branch, and start trcking.
- `git checkout -b feature/1234 origin/feature/1234` -> Same as above, also change to this branch.

if push fails, then fetch + merge + push. to get changes from remote. And then push on top of it. 

### Delete a remote branch
- `git push origin feature/1234` == `git push origin feaure/1234:feature/1234`
- `git push origin` :feature/1234
- `git push origin` -> delete feature/1234

### Force Push -> Destroy the remote commits/version and put my version
- Remote is gone horribly wrong and needs repair.
- Local version is preferable
- Versions have diverged and merging is undesirable
- `git push -f` or `git push --force`
- Commits on remote branch will disappear. 
- `git show origin/master` -> Will show diff between current branch and origin/master i.e. tracking branch for remote.
- `git reset --hard origin/master` -> All collaborators need to do this to get the change locally, git pull will not make the faulty commit disappear in local branch.
- It is a big impact on collaborators. Only Maintainer should do such changes.

### Pruning stale branches 
- Delete all remote tracking branches.
- Remote-tracking branches, not remote branches.
- git remote prune origin
- git remote prune origin --dry-run

### Tags

#### Generic tag commands
- Lighweight tag
 - git tag newfeature.v1 SHA12345
- Annotated tag
 - git tag -a "V1.1" -m "Some message" dd87659sfd
- Uses current HEAD of SHA not provided.
- git tag or git tag --list or git tag -l
- git tag -l "v2*" -> Wild Card Search
- git tag -l -n -> get first line of annotations as well.
- git show tagv1 or git diff tag1..tag2 -> Just like we use SHA
- git tag -d v0.1 or git tag --delete v0.1

#### Tags with remote
- git fetch fetches tags aas well
- git push doesn't push tags by default.
- git push origin v1.1 -> Pushes this tag
- git push origin --tags -> Pushes all tags
- git fetch --tags -> only fetches tags
- git push origin :v1.1 -> delete a tag. just like we delete a branch.
- --delete and -d will also work.

#### Checkout Tags
-  git checkout -b new-branch v1.0 -> right way to work with tags.
-  git checkout v1.1 -> Dangerous option, Will move HEAD backwards.
 - ~May end up with detached HEAD state, if you keen on making commits from there.~
 - Its better to checkout master again.
 - Detached HEAD is like being on a unnamed branch.
 - New commits will not belong to any branch, and will be Garbage Collected (in almost 2 weeks)
 - To save work in such case, we can tag those commit as temp. git tag temp. (HEAD detached) 
 - Or Create a branch git branch temp_branch (HEAD still be detached). Once you checkout master or some other branch, then HEAD will be moved to normnal state.
 - git chekout -b new_branch -> reattached HEAD.

## Interactive staging
- `git add -i` or `git add --interactive`
- patch mode -- Hunk
- Other commands that work with Patch mode
 - `git add -p or git add --patch`
 - `git stash -p`
 - `git reset -p`
 - `git checkout -p`
 - `git commit -p`
- Hunk can be splitted, and also edited.

### Cherry Picking commits
- git cherry-pick sha1
- git cherry-pick sha1..sha2
- Cannot cherry-pick merge commit
- --edit or -e to edit commit message
- Can result in conflict, which must be resolved
- use git cherry-pick --continue instead of git commit

### Patches -- Create sharable patches

#### Diff Patches
- git diff from-commit output-commit > output.diff
- Make sue to start with one commit before, before adding changes of a commit. So from commit should be one commit before.
- Has no commit history, SHA, or commit messages. 
- So it can only apply changes, not commit those changes, due to lack of metadata.
- To apply diff, `git apply ~/Desktop/Patch_to_diff_file`
- Changes are nethier staged, nor commited.
- Wont apply entire commit, in case of conflicts. 
 - Files/Working tree should look like how it was supposed to look like before the commit/dif.

#### Formatted patches
- includes commit messages and metadata of commits
- git format-patch sha1..sha2
- Creates one file for each commit. So if there are 5 commits between SHA1 and SHA2, it will create 5 files
- git format-patch master -> Diff between current and master. All commits in between TIPS.
- git format-patch -1 655da
- git format-patch master -o feature
- git format-patch 2e33d..655da --stdout > feture.patch -> in single file
- same as cherry-picking, same chnages, different sha
- git am feature/file1.patch
- git am feature/*.patch
- It also does changes and commits.

## Log options

### log
- git log -p (or --patch)
- git log -L 10,15 my.txt

### clame
- git blame <fileName> 
- -w to ignore whitespaces
- -L 100,150 to show specific lines
- -L 100,+5 to show 5 lines from line number 100
- git blame SHA filenae.txt -> pretends as checkout file version of that given commit.
- anotate is similar to blame

### Bisect
- git bisect start
- git bisect bad <treeish> -> start as given commit to be good
- git bisect good <treeish> -> start as given commit as bad
- git bisect good/bad -> current is good or bad
- git bisect reset

## Rebase
Cleaner and more Linear project history
Owner of the branch ensures topic branch commit applies cleanly.
All commit SHA changes of current branch
git stores the commits at temporary storage, moves the HEAD to source branch TIP, and then replays those commits.
git rebase master -> replay current branch at the tip of master branch. Uses current branch by default
git rebase master new_feature -> rebasing of new_feature, automatically checkout new_feature.
git merge-base master new_feature -> Tells where new_feature diverged from master
merge can be undone, not rebase.
no additional merge commit, but destructive. History changed completely.
Thou shalt nor rebase public branch -- as it abandons existing shared commits and creates a new commit.
Collaborators would se history vanished.

### When to rebase or merge
Merge to allow commits to be stand out or to be clearly grouped.
Merge to bring back topic branches back into master
Rebase to add minor commits in master to a topic branch.
Rebase to move commits from one branch to another
Merge anytime the topic branch is public and is being used by others.
git rebase --onto newbase upstream branch -> To rebase across the branch.
 - onto is the branch on which we are going to rebase our target branch
 - upstream is the branch on which our current branch(or the one we want to move) is based upon
 - final input branch is the branch which has to be moved. i.e. all commits which are made on target branch will be moved/rebased to --onto newbase_branch, but without taking the commits of upstream branch.
 - Can be undone, just by changing first 2 arguments of command, So target_branch will be moved back from onto newBase branch to original upstream branch.

#### Undo a rebase
Undoing complex rebase may lose data.
- git reset --hard ORIG_HEAD
 - ORIG_HEAD is a temporary HEAD, which points to the HEAD before the 
 - Can be changed, if another rebase would have happened.
 - git rebase --onto SHA<MERGE_BASE> master new_feature

#### Interactive Rebase
- Chance to modify or undo commit as they are being replayed.
- Can edit commit contents
- Can reorder or skip commits.
- pick, drop, reword, edit, squas, fixup, exec
- git rebase -i (--interactive)
- git rebase -i HEAD~3 -> To edit history and commits, on current branch. Not recomended in Public repo as, commit SHAID will be lost.
- 

### Squash commits
- squash: combine change set, concatenate commit messages.
- fixup: combine change set, throw out the messages.
- Uses first Author in the commit series.
- git rebase -i HEAD~3 -> Gives an opportunity to do these squash etc.
- 
