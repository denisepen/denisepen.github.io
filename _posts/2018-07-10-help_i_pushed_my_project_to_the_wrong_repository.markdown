---
layout: post
title:      "HELP! I pushed my project to the wrong repository!!!"
date:       2018-07-10 19:28:12 +0000
permalink:  help_i_pushed_my_project_to_the_wrong_repository
---



So I was making great progress on my rails project when I had the bright idea to make a few changes. I thought I would add a new model which would then change the way the other models interacted. Since this new model could potentially lead to a completely different level of functionality and thus a completely different user experience I decided to make this an entirely new project.  Rather than create a new branch from the old project I would simply create a copy of the old project and give it it's own repository.  Simple enough right?  

![](https://media1.tenor.com/images/e83efff147c45e884da3e73f867be4a8/tenor.gif?itemid=9450407)

That's git and Githib laughing in the backround at my naivete.  

You see I forgot about a little thing called the .git folder.  The parent project that I was copying had already been saved to a Github repository and because of this it already had a .git folder.  The .git folder has all the information that identifies the file as being a git project and  also contains information about its remote - it's Github repository.  The .git file is created whenever you initialize a new git file - when you type: 

`git init` 

Read more about the .git folder here: [What is a .git folder?](https://www.quora.com/What-is-the-git-folder)

Immediately upon typing `git init` a .git file has been created and added to your project and has been tagged with identifying information about its remote location.  Any duplicate of this project will also include this .git folder which means it also now has been tagged with the **EXACT SAME REMOTE LOCATION AS YOUR PARENT PROJECT!!!** 

That means there are now two projects being pushed to the exact same repository - Not what I wanted at all.  When I typed: `git remote add origin git@github.com:denisepen/new_app.git` I received the following message: 
```
fatal: remote origin already exists.
```


But I ignored this because I had followed all the instructions and there was no reason for there to be an error.  This was a new project right? (WRONG!)  I continued to push this new project to it's repository. And I thought everything was fine until I realized that I couldn't view my files on github.  Even after pushing this new project to Github I still only saw this page: 

![](https://i.imgur.com/d7i5xMu.png)


**When I expected to see something like this: **


![](https://i.imgur.com/fTVpaiS.png)


So I panicked because I knew that I had pushed this new project  - somewhere - I just didn't know where.  Until, I went back to the parent project (the one that I had copied from), made some changes to it, and tried to commit and push the changes to the master. The push was rejected and I received the same error I had received before: 
```
fatal: remote origin already exists.
```

And that's when I started to panic. When I checked Github I realized I had pushed the new project to the repository of the old project.  **HOW DID THS HAPPEN?**

It was the .git folder.  When I duplicated the parent project I also copied the .git file which automatically set the location of the remote - which was the repository of the parent.  

So, two lessons here:

> 1.  If you receive an error with the word "fatal" in the message stop what you're doing, review what  you've done up to that point, and do not, under any circumstances, proceed on your current path because it will be "FATAL".  (I'm forgiving myself for this one because I am relatively new to programming and didn't realize how bad fatal can be.  For the record even in programming, "fatal" is not on a spectrum, it is an absolute. Oh well, you live and you learn).
> 
2. If you are copying a project with a .git file immediately delete the .git folder on the copy.  This can be accomplished with  `rm -rf .git`  Once you you initialize the git process by typing `git init` a new .git folder will be generated. And your new project will be routed to the appropriate repository.  


Luckily,  both files were exactly the same so it wasn't too much of a problem to fix but it was still a headache.  I had to roll back the parent's location to where it was before my mistake:  

Using `git log` will give you a list of all the projects commits.  Find the ID of the commit you want to go back to and type `git reset --hard <Your-Commit-ID>` .  At this point your parent is fixed.
	
	
This was a great learning experience but it just added to my paranoia about using git.  What mistakes have you made while using git? Do you also look like this before preparing to do something new with git?

![](data:image/jpeg;base64,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)







