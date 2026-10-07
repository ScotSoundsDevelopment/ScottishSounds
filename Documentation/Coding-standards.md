This document will show how the team works on a shared repository, and it is meant to be a guideline when wondering about how to name things, structure the file directory and follow GitHub workflows to safely and confidently contribute to the codebase.

### The Code
Based on the stack decided we follow the standards indicated in the official documentation when it comes to casing conventions and nomenclature for variables, methods, components, classes, file names, etc.

#### Variables
to be determined

#### Functions
to be determined

#### Classes
to be determines

#### File Names
to be determined

#### Self Documenting Code
Write code that is self explanatory, don't leave any room for ambiguity. If you are creating a variable that contains the age of a cat, call it ``catAge``. If you are writing a method that calculates the water temperature in a tank call it ``CalculateTankWaterTemperature()``. This allows us to keep comments to a few lines, commenting your code is good practice but creating a wall of text for others to read is not efficient. More on self documenting code can be read in this post: https://critter.blog/2020/09/15/dont-comment-your-code-refactor-it/ 

### Git & GitHub
Git and GitHub are powerful tools that allow us to safely and securely work on a shared repository. When working on the same codebase with multiple people, following processes makes the difference between knowing exactly what is going on opposed to feeling clueless when approaching a ticket. 

#### Git
Git is a distributed version control system that allows you to apply changes on a file structure when working on a "copy", this allows you to try and test things without irreversibly breaking the original code, it is very important though that you understand why and how. Git needs to be installed on your machine to work locally, more on Git in the official website: https://git-scm.com/
Use `git --help` when unsure which command to use.

##### Basic commands 
`git init`: This initialises a git repository within the folder it was called from. It creates the .git folder that allows git to work within that environment
`git status`: Allows you to check on the status of the files contained in the folder, it will show if there is any unmarked (new) file, any staged (added) file, any deleted file or any modified file that needs to be committed.
`git branch`: Shows all the local branches available, when adding a word after 'branch' it creates a new branch with that word as a name
`git checkout branch`: It switches to the branch named 'branch'
`git add .`: Stages all file changes
`git add ./folder/specificFile.txt`: Stages the file specified in the path, good practice when you want to write a personalised commit message to each change
`git commit -m 'message'`: Commits all the staged files, this is like a save on the branch it is called from, adding a message is mandatory, it will help understand what changes have been made and it will appear as the commit title
`git stash`: Have you added changes to the wrong branch? No worries! You can "stash" any staged change away, this command will allow you to virtually undo your changes without deleting them
`git stash pop`: Once you have switched to the correct branch, this command will "release" the changes into the new branch, no work gets lost!

#### GitHub
GitHub allows you to host a repo in the cloud and share it with other developers. It has become industry standards not only because it allows to work on a repo from multiple endpoints but also because it makes the repository always available.
There are specific workflows to follow when working on a hosted repository, this is to protect the codebase from braking changes, as well as making sure your code can be reviewed by your peers.

##### How To Name Branches
Branches should be named using "-" instead of spaces, so a viable branch name would be: `branch-name`. We use the ticket number as a start reference to allow people to understand what the branch was created for, and a very brief feature description. If you are working on ticket #66 adding authentication you could name your branch: `t66-authentication`.

##### Dev
Dev is our default branch, it is the branch we create new branches from, and the branch we commit and merge into when changes are approved.

##### Main
Main is the "client's branch", when we hit a deployable milestone, we merge the current dev into main.

##### Publish a local branch
When you create a new branch in your local copy of the repository, you will have to publish it in the hosted one. use `git push origin -u branch-name` where branch-name is the name of the branch you want to publish

##### Push Commits
Once the branch is published you can now integrate pushes to the remote as part of your workflow, once a a commit as been made, you can update your hosted branch by calling `git push` 

##### Pull Requests
Once you are done working on a feature and you want people to review your code and merge it into dev, you can go over the GitHub web interface (or GitHub desktop) and open a Pull Request towards dev. Add the people you want to review it as reviewers and wait for their acceptance or request for changes. Request for changes can be argued if you think your choices are correct, if a resolution is not possible via GitHub comments we can discuss it in a stand-up or dedicated meeting.

##### Merging
Once the PR is approved by all the reviewers, you will be in charge of the merge. This is to make sure that you know when the ticket can be placed into the "done" column. When reviewing others' Pull Requests never perform the merge for them unless there's a good reason for it.

##### Code Reviews
These happen when you are added as a reviewer in a Pull Request. Every person should be able to review each other's code, and be able to understand what the code is for and why choices have been made, what specific functions are for and why they have been added. When writing code keep in mind others will have to understand it as well.

