# new-member-training: git merge conflicts

A merge conflict can come up when you're trying to merge two branches, but both edited the same file in the same place.  We'll walk through an example.

## Table of contents

#### [1. Setup](#setup)

#### [2. Your mission](#your-mission)

#### [3. Making a new branch](#making-a-new-branch)

#### [4. Committing our work](#committing-our-work)

#### [5. Simulating someone else's work](#simulating-someone-elses-work)

#### [6. Try to merge your branch into master](#try-to-merge-your-branch-into-master)

#### [7. Fixing the merge conflict](#what-the-merge-conflict-looks-like)

#### [8. Optional sections](#optional-sections)

##### [8a. Optional command line stuff](#optional-command-line-stuff)

##### [8b. Optional discussion about git workflow](#optional-discussion-about-git-workflow)


## Setup
Clone this GitHub repository.  This will download the files in this repository onto your local computer.
On the command line (i.e. Git shell):
```
$ git clone https://github.com/WHSRobotics/new-member-training.git
```

If you want to learn some useful stuff for the command line, click [here](#optional-command-line-stuff) for some tips.


## Your mission

For training purposes we want to make a merge conflict happen.  We'll set up this situation:

![Merge conflict image](/img/merge_conflict.png "Merge conflict")

If you look at the file `TrainingProgram.java` you'll notice that there are some things that need to be changed:
```java
public class TrainingProgram {
  public static void main(String[] args) {
    System.out.println("Robotics is fun!");
    System.out.println("My favorite robot is R2-D2.");
    System.out.println("Can't wait for the Velocity Vortex season!");
    System.out.println("Better get ready!");
  }
}
```

Our favorite robot is not R2-D2, and it is currently not the Velocity Vortex FTC season.
We'll fix these changes in our own branch called `text-fixes`.


## Making a new branch
To make a new branch called `text-fixes`, run this on the command line.
```
$ git checkout -b text-fixes
```
The `-b` means "make a new branch."

Edit lines 4-5 in `TrainingProgram.java` as such:
```java
    System.out.println("My favorite robot is BB-8.");
    System.out.println("Can't wait for the Relic Recovery season!");
```

What do we do next?  We can check the status of our git repository:
```
$ git status
On branch text-fixes
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   TrainingProgram.java

no changes added to commit (use "git add" and/or "git commit -a")
```

It shows that `TrainingProgram.java` needs to be staged.

If you want to brush up on your git, click [here](#optional-discussion-about-git-workflow) for an optional discussion about git workflow.


## Committing our work

Then let's stage and commit this to get ready to put it back onto the remote repository on GitHub.

To stage it:
```
$ git add TrainingProgram.java
```

To commit it:
```
$ git commit -m "Change favorite robot and season"
[text-fixes 6941ee1] Change favorite robot and season
 1 file changed, 2 insertions(+), 2 deletions(-)
```

Now we just have to go back to the master branch and merge this `text-fixes` branch into it.

This is how our repository looks like:

![Committed to text-fixes branch image](/img/merge_text-fixes.png "Committed to text-fixes branch")


## Simulating someone else's work

In the meantime while you were making that branch, Amar with his super speedy typing fingers already updated `TrainingProgram.java` with his favorite robot.  However he was not a cooperative teammate and he already pushed this change to master.  Let's simulate this by editing the `TrainingProgram.java` file on the master branch.

Switch to the master branch:
```
$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
```

Edit line 4 of `TrainingProgram.java` as follows (or at least have a favorite robot that's not BB-8):
```java
    System.out.println("My favorite robot is WALL-E.");
```

Note that Amar didn't pay attention to the FTC season and still left it at Velocity Vortex.

Stage and commit the file:
```
$ git add TrainingProgram.java
$ git commit -m "fav bot"
```

Now this is how our repository looks.  Thanks Amar.

![Merge conflict image](/img/merge_conflict.png "Merge conflict")


## Try to merge your branch into master

Now is your time to shine with your git wizardry.

Make sure you're on the master branch:
```
$ git checkout master
```

Then to merge your changes (favorite robot is BB-8, updated season to Relic Recovery) into the master branch:
```
$ git merge text-fixes
Auto-merging TrainingProgram.java
CONFLICT (content): Merge conflict in TrainingProgram.java
Automatic merge failed; fix conflicts and then commit the result.
```

## What the merge conflict looks like
Here is the infamous merge conflict in all of its glory.  Open up `TrainingProgram.java` and you'll see:
```java
public class TrainingProgram {
  public static void main(String[] args) {
    System.out.println("Robotics is fun!");
<<<<<<< HEAD
    System.out.println("My favorite robot is WALL-E.");
    System.out.println("Can't wait for the Velocity Vortex season!");
=======
    System.out.println("My favorite robot is BB-8.");
    System.out.println("Can't wait for the Relic Recovery season!");
>>>>>>> text-fixes
    System.out.println("Better get ready!");
  }
```

git tried to merge the files, saw that they were different in the same places, and is saying, "hey humans, please figure this out."
However it is nice and does show you both of the changes in the same file.

This part is from the master branch (Amar's edits)
```java
<<<<<<< HEAD
    System.out.println("My favorite robot is WALL-E.");
    System.out.println("Can't wait for the Velocity Vortex season!");
=======
```

This part is from the `text-fixes` branch (your edits)
```java
=======
    System.out.println("My favorite robot is BB-8.");
    System.out.println("Can't wait for the Relic Recovery season!");
>>>>>>> text-fixes
```

## Fixing the merge conflict

Talk with your teammate Amar and reach an agreement on what the favorite robot and the FTC season should be.
Then edit `TrainingProgram.java` accordingly.  Sample follows:
```java
public class TrainingProgram {
  public static void main(String[] args) {
    System.out.println("Robotics is fun!");
    System.out.println("My favorite robot is EVE.");
    System.out.println("Can't wait for the Relic Recovery season!");
    System.out.println("Better get ready!");
  }
}
```

Checking the status of our repo:
```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   TrainingProgram.java

no changes added to commit (use "git add" and/or "git commit -a")
```

So let's stage and commit our newly reconciled file and be friends.
```
$ git add TrainingProgram.java
$ git commit -m "Reconciled bot and season"
[master 4bc3aba] Reconciled bot and season
```

Checking our commit history we can see that all is well:
```
$ git log
commit 4bc3aba7a6f8b30075d59a605c955376ef182b86
Merge: 5150103 6941ee1
Author: Proud Heng <pheng@ucsd.edu>
Date:   Mon Sep 11 20:28:59 2017 -0700

    Reconciled bot and season

commit 515010394c8ec554a3943a8e87b035ebb174f1e9
Author: Proud Heng <pheng@ucsd.edu>
Date:   Mon Sep 11 19:29:46 2017 -0700

    fav bot

commit 6941ee1c65174e1d97f1615c66de0c279042035f
Author: Proud Heng <pheng@ucsd.edu>
Date:   Mon Sep 11 19:26:55 2017 -0700

    Change favorite robot and season

commit f7c96d85072446f196e200b08576ea72e7046d52
Author: Proud Heng <pheng@ucsd.edu>
Date:   Mon Sep 11 19:24:04 2017 -0700

    Init

commit 48ed824c48e67dda2165a496dd1c1e0920f23436
Author: proudh <pheng@ucsd.edu>
Date:   Mon Sep 11 19:22:43 2017 -0700

    Initial commit
```

The merge conflict is solved. Good job!

## Optional sections 

### Optional command line stuff

To go into the folder containing the git repository, type the following command ("cd" == "change directory"):
```
$ cd new-member-training
```

To list all files in this folder, type:
```
$ ls
README.md		TrainingProgram.java	img
```

To print out the contents of a file (for example `TrainingProgram.java`), type:
```
$ cat TrainingProgram.java 
public class TrainingProgram {
  public static void main(String[] args) {
    System.out.println("Robotics is fun!");
    System.out.println("My favorite robot is R2-D2.");
    System.out.println("Can't wait for the Velocity Vortex season!");
    System.out.println("Better get ready!");
  }
}
```

If you want more info on these commands, look them up in the manual using `man`:
```
$ man cat
```

To exit out of the manual, press `q` to quit.

Click [here](#your-mission) to get started on your mission.


### Optional discussion about git workflow

The files you see online on GitHub in a repository (i.e. at https://github.com/WHSRobotics/new-member-training) are on the **remote repository**.

When you clone the remote repository to your own local computer, you get a copy of the files there.  So the folder `new-member-training` that gets created that has all of the files in the remote repository is called the **local repository**.

The process of adding/removing/changing file(s) from your local repository to your remote repository goes:

1. **Add the changed file(s).**  This stages them, i.e. lets you select which files you want to add/remove.

2. **Commit the file(s).**  This neatly packages your changes with a commit message that lets everyone know why you made the changes, i.e. "Added driver control for drive train."

3. **Push the commit.**  This uploads your changes to the remote repository.

You can [do this directly on GitHub](https://help.github.com/articles/managing-files-on-github/), but in general people will either [use the command line](https://help.github.com/articles/managing-files-using-the-command-line/), or a client like [SourceTree](https://www.sourcetreeapp.com/) or [GitKraken](https://www.gitkraken.com/).

Let's say you want to add a file `HelloFellowRobots.java` to your remote repository `training-programs`.  Here are the steps you'd take to do that:

0a. On your computer, **make the file `HelloFellowRobots.java`**

0b. Find your remote repository `training-programs` on GitHub, and **clone it somewhere onto your computer**.  How to clone a repo: https://help.github.com/articles/cloning-a-repository/

0c. Let's say you cloned the repository to `~/whs-robotics/training-programs`. This is where your local repository is.  **Move `HelloFellowRobots.java` into the `training-programs` folder.**

Add, commit, and push the files following any of these (or your own favorite client):

- [directly on GitHub](https://help.github.com/articles/adding-a-file-to-a-repository/)

- [the command line](https://help.github.com/articles/adding-a-file-to-a-repository-using-the-command-line/)

- [SourceTree](https://confluence.atlassian.com/get-started-with-sourcetree/commit-and-push-a-change-git-847359114.html) 

- [GitKraken](https://support.gitkraken.com/getting-started/guide)
 
Now we're ready to commit our work!  Click [here](#committing-our-work) to go back.
