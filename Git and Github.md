**By Kunal Kushwaha yt**

# IMPORTANT
```
git init
git add .
git commit -m "message"
git status

git remote add origin <copied github repo link>
git remote -v

git push origin main 

git branch feature // create a new branch 
git checkout feature

git checkout main 
git merge feature

git log 
```

### Basic directory commands : 
```
Show contents of current folder : 
> ls

Make directory :
> mkdir dir_name
mkdir project01

Change directory : 
> cd dir_name
Ex :
cd D:
cd project01

Command to create a file :
> touch fileName.extension
Ex :
touch names.txt

Command to Delete a file :
> rm -rf fileName
Ex :
rm -rf names.txt

View the contents of the file
> cat fileName.exe
Ex :
> cat names.txt

To Open the file in a text editor :
> vi fileName (for vim)
> subl fileName (for Sublime Text)
```

`git init`
- Initializes an empty git repository (.git) in the folder which tracks the changes made in the folder.
- .git is generally a hidden file
- ls won't show hidden folders
- ls -a : to show the hidden files and folders

To see what is inside the .git folder : 
`ls .git`

To see if the changes made to the git repo are saved or not : 
`git status`

Putting untracked files into the staging area : 
`git add .`
For specific files in the current directory : 
`git add names.txt`

To commit :
`git commit -m "message"`
-m : message to be displayed

To remove files from the staged area :
`gitrestore --staged names.txt`

To see the whole history of the project i.e. all commits made to the project :
`git log`

Note :
- each commit is built on top of the previous commit 
- So you cannot delete any commit from the middle
- You can unstage  commits for a checkpoint or a particular commit

To remove some commits : 
- copy the id of the commit just below it 
`git reset <commit it>`

Code Backup : 
If you want to try something on a clean codebase
Like if you want to save your work somewhere without making a commit and try something else, but when you want to get the original code back you can get that back
`git stash`

To get back the staging area : (rewind the git stash)
`git stash pop`

Delete changes that were not commited : 
`git stash clear`


To link github repository to git :
`git remote add origin <copied github repository link>`
origin -> name of the url that you added 
By convention all repos and folders in your personal account have a name called origin

Show all urls attached to the folder :
`git remote -v`

To view our changes in the github we push : 
`git push <url name> <branch name>`
ex : 
`git push origin main`
`git push origin feature`


## Branching
A branch represents an independent line of development. 
Branches serve as an abstraction for the edit/stage/commit process.

![[example-diagram.png]]

- In branches never commit on the main/master branch
- Branches are used for new feature implementation or fixing bugs.
- Our code which is not finalized yet might contain some errors
- So all our code should go on a separate branch so that the users and other devs are not affected.

Creating a new Branch
`git branch <branch name>`
Ex : 
`git branch feature`

To checkout or point to a new branch :
`git checkout feature`

WEBSITE TO VISUALIZE AND LEARN BRANCHING :
https://learngitbranching.js.org/

- Every time you create a new branch it will be created from the head.

To return to the main branch :
`git checkout main`

![[git picture 2.png]]

To make the branch (feature or bug fix) a part of the main branch : 
`git commit <branch name>`



## For Contributing to Open Source Projects : 
1. Open the project you want to work with on github
2. fork
3. Select your own account
4. Now copy link of the repo
5. `git clone <link>`
Ex : `git clone https://github.com/example-user/my-project.git`

- Your own account url is known as origin
- Similarly the account from which you have forked the project is known as upstream url (by convention)

#### Pull Request
- When you make any changes in your own copy of the project and want it to be visible in the main project 
- You request in via a PULL REQUEST
- Then people would review your code
- Might suggest some changes. 
- Then when it is merged, the code changes you made in your own account (forked) in any branch;
- That will be visible in the main project's main branch

Note : One branch can only open one request
If you want  to open different pul requests for different features you are working on, you will and should create different branches.

When the online repository contains a commit that your repository does not, you might have to force push it
`git push origin kunal -f`


#### Fetching
- Fetching refers to downloading the latest changes from a remote repository without merging them into your local branch.
Command : 
`git fetch `
Ex : 
`git fetch origin`

- You can fetch upstream - fetch and merge, in your origin repo to update your forked project according to the original project
OR
`git checkout main`
`git fetch --all --prune`
prune -> those which were deleted will also be fetched
Now we have to reset the main branch of the origin to the main branch of upstream
`git reset --hard upstream/main`
then 
`push origin main`
or
`git pull upstream main`

Let's say you have a lot of commits you are working on and you want to merge that into a single commit

Trick to squash last few commits :
Reset the commit before which you want to group
Then commit again

OR 

`git rebase -i <commit id just before which you want to group>`
i -> interactive environment

Now all the commits above it you can pick and squash
Change 'pick' into 's' for the commits you want to combine

Whichever is labeled as s merge it into the previous commit labelled pick

Ex : 
Pick qwerty 1
S tyuiiop 2
S vjjhvok 3
Pick fdashf 4

This will basically merge 2 and 3 into 1

### Merge Conflicts 
Multiple people making changes to the same line/code
Git will ask you to resolve changes manually
Then mark as resolve
Commit merge


# ERRORS :
1. "main and master are entirely different commit histories"
Solution : 
```
git checkout master
git branch main master -f
git checkout main
git push origin main -f
```


Reverting local changes back to the last commit : 
```
git restore .
git reset
git clean -f
git clean -fd
```