---
layout: post
title:      "First time collaborating with git and first hackathon!🥳"
date:       2020-11-13 16:20:20 +0000
permalink:  first_time_collaborating_with_git_and_first_hackathon
---


This week I was able to join a team for my first time collaborating with git but also my first hackathon and the experience so far has been very intense and great! I entered the team a little bit late because they needed extra help after they started, but the tight deadline got me into a hackathon "rush" mode and had a couple of days of intense coding. 

I usually don't code much on the weekends but with this "rush" found myself coding all the weekend from early in the morning (9-10am) until 1-2am, which was probably not the healthiest approach (still made sure I took a long 4-5 hour lunch break). But for the next time I will make sure I pace myself and not lose extreme amounts of sleep over it.

We built this project with the full-stack react-typescript framework blitz which I never worked with before, excluding React, so it was a big learning experience!

But, I think the main lesson I am taking away is learning to collaborate with git, as all software development work relies on version control to manage complex situations where everything needs to be in sync like a symphony or a complex machine.

## Collaborating with git

Some of the git commands that I got more familiar with working in this project were `git checkout -b`, `git checkout` and `git merge`. These commands allow you to create new branches or travel around existing branches in the project to make sure different branches are more compatible.

### `git checkout -b <name-of-branch>`

Whenever we create or clone a repository we usually arrive to the `main` -or before the "master" branch-. But git allows to create branches or copies of the code (that will only exist in the 'git alternate universe' so you wont literally see that your project folder has been duplicated on your computer, but 
 the new branch exists in git). Each branch in git has its own commits and modifications. So `git checkout -b new-branch` will create a new copy of your project called `new-branch` where you can make changes and make sure that the original is not affected.

### `git checkout <name-of-branch>`

This command without the `-b` argument allows you to travel around the different branches/versions of the project as long as your current branch does not have any changes pending to be committed. If your branch has changes you can push them or [stash](https://git-scm.com/docs/git-stash#:~:text=DESCRIPTION,to%20match%20the%20HEAD%20commit.) them. [(Link to a great video about stash)](https://youtu.be/KLEDKgMmbBI?t=525)

### `git merge <name-of-branch>` 

This command will allow you to overwrite the branch you are currently on with another one. So the normal scenario could be that you want to have the new features that the team made. You can transfer the new features to your branch by doing `git merge <new-features-branch>` and the branch `new-features-branch` would overwrite yours.

However, if you made some changes in the same files as the `new-features-branch` the merge conflict would have to be resolved manually [(Great video on this)](https://www.youtube.com/watch?v=__cR7uPBOIk).

### Some scenarios:

*How can I add the latest of project features to my branch?*

It could be by first 'traveling' to the branch that you want to start from. Maybe you were working away from the project for a couple of days and some new things were added, and you would like to have them in your branch and build your new additions on-top of them. 

First, if you want to take a look at what the changes specifically look like, you can travel to the newest branch by doing `git checkout <main>` e.g. or <any branch name>, and launch for server/app from there. Then if you want to make this changes available in your branch you can travel back to your branch (`git checkout <my-branch>`) and from there do `git merge <main>` and overwrite `my-branch` with `main`.

*What if I have some changes in my branch?*

Also if you have a merge conflict but still want to tinker around with the `main` branch you can just 'travel' to the other branch e.g. (`git checkout <main>`) and then from there create a new branch by doing a `git checkout -b <new-branch>` and the git command with the `-b` attribute will allow you to create a brand new branch that is a copy of the `main` without changing your original branch `my-branch` where you had the changes you wanted to keep.

### Final thoughts:

If there was in fact a conflict of branches where the same files where modified this would have to be resolved 'by hand', which would just be a bit more tedious but not a problem impossible to solve.

Hopefully this blog was helpful to get started with collaborating in the git universe, I know for me its been a confusing topic but learning these tools really helps to understand the software development process.

Feel free to reach out for any conversations:

[LinkedIn](https://www.linkedin.com/in/santiago-salazar-pavajeau/)

[Twitter](https://twitter.com/santispavajeau)



