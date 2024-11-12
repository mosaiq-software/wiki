# Using Git Version Control

Git is an extremely important part of day to day operations as a developer, and we must be diligent to use it properly.

## Big Idea

The main idea behind using version control is that it makes everything we do traceable, restorable, and portable. When - not if - something goes wrong we need to be able to be able to track what happened and how to fix it. This is the power of git.

## How

While there are many different ways to use git, here is how we use it at Mosaiq:

All git related actions, aside from commit, should be done via the git CLI and not an IDE. It may be difficult to remember all the commands initally, but it is worth knowing.

### Creating a Repo
To start a new project, create a new **private** repository on Github. If the project it already active, skip to step 2. We stick to using lower-kebab-case for all repository naming to ensure consistency across projects.  
Clone the repository by grabbing the link ending in `.git` from the clone section of github, and run the command
```
git clone https://github.com/mosaiq-software/<my-project-name>.git
```
Its a good practice to make a new folder on your machine just for Mosaiq repositories, run the clone command in there to generate a new folder within it.

Once you have the repo, its time to get to work. Every card on Trello will be assigned a "card code", "story number", or "work item": all just fancy names for the XXX-123 number that corresponds to the projects code (such as NSM for node-server-manager) and a number.
  These card codes help keep track of what was completed when and give a simple way to relate code and Trello cards. Pick an item that was assigned to you (or choose one from the backlog) and you're ready to go.

### Branches
In (most) of our projects we use a standard convention to organize branches
`main` is the most protected branch. This will be where we deploy code from and should be as bug-free as possible. The only way code is added to `main` is by the projects lead engineer making a PR to `main` from `develop`
`develop` is the master working branch of all our changes. When a work item is complete and ready to get merged in, this is the branch it goes to.
`XX-00` is the naming convention to use for your branches. Name your branch after whichever card number it is you're currently working on. Make sure to only make changes relative to that card in its own branch.

To get started with development, make sure to pull the latest version of the `develop` branch using:
```
git checkout origin develop
```
to checkout the develop branch, then pull in changes using
```
git pull origin develop
```
to update it. It should either say that its up to date, or show +/- for what changes were made.

Once you've checked out develop, you can use
```
git checkout -b XXX-1223
```
To checkout a branch for you to work on, replacing `XXX-123` with your card's number. The `-b` flag on `checkout` tells git to try and create a new branch


### Development
The development process is very straightforwards. Make frequent commits to your branch using
```
git commit -a -m "enter a commit message in this string"
```
or use your IDE's built in source controller for this.

If you accidentally make a commit that you want to undo, this can be done using
```
git reset --soft HEAD^
```
and it will undo the last commit, but preserve your work. Look into hard resets if you want to delete the work.

If you need to switch branches while working, you can commit the code to your current branch then checkout another. Its normal to have multiple active branches at once.

If you accidentally make changes on the wrong branch, not to worry, get all your changes into the stage (ready to commit but not commited yet) (use reset as shown above if needed), then checkout the branch you'd like to push the changes to. Commit them there.

Once you're done work on this branch, make sure you commit all changes.
Then run
```
git fetch origin develop && git merge origin develop
```
to merge any changes that may have been made since you first checked out this branch. If there are merge conflicts here, merge them in. For generated files such as `package-lock.json` or `yarn.lock` always accept the incoming changes, then regenerate the files if they have issues.

Once you've got your branch ready to merge into `develop`, run 
```
git push
```
to push the branch to Github.


### Pull Requests (PR)
After you've pushed a branch to Github, its time to merge it into develop. Unfortunately, we are human, and tend to make changes that arent always 100% right the first time. This is why we have pull requests!
In Github, open a new PR to merge your branch into develop, denoted as `develop <-- XXX-123`. When you create this a webhook should fire to let members in the discord know that your code is ready for review.

We don't have Github Pro, so we can't enable rules on our private repositories. Thus, we have to be diligent in our process to make sure we always get *at least* 1 reviewer on our code before we merge it.

You can now move the card on Trello to the 'In PR' list.

When you see a message that someone elses code is in PR, it is beneficial to the entire team that you take a look at it. The quicker we can get code reviewed, the faster we can make progress.

Don't be afraid to be overly confident and knit-picky in this process! We all know how to program, if you see something thats not right, or just have a genuine question about what it does, ask! Put a comment in the section of code and request it to be reviewed. Even the best developers make mistakes, we need to hold eachother to the highest standard when it comes to this.

Once a PR is reviewed, approved, and ready to merge, the creator of the PR (unless someone else has permission to) should merge it in.


### Hooks & Scripts
We have a few simple hooks and scripts set up on our projects. These should be added into every new project to uphold code quality standards and increase automation of workflows.

Husky pre-push will run a set of commands after you run `git push` and before git pushes, allowing code to be tested before being sent to the remote. All proejcts should have the prepush hook run unit testing with coverage, linting, and the before-push script.

The before-push script checks that the current branch is not named `develop` nor `main`, and throws an error if it is.

Mosaiq's Github org is set up with a global webhook that listens for when PRs are created and relays it to Tilebot. This should not be enabled on an individual repo to avoid duplicate messages.


### Tips
You can chain together commands using `&&` in the command line such as:
```
git checkout origin develop && git pull origin develop
```
to do multiple actions back to back.


When you create a branch locally, it is not automatically made on the remote side as well. You can enable this by running
```
git config --global push.autoSetupRemote true
```
Now your remote branch is made and tracked whenever you run `git push`




