Table of Contents
=================

   * [1. Install git](#1-install-git)
   * [2. Clone a repository from github](#2-clone-a-repository-from-github)
   * [3. Basic usage of git](#3-basic-usage-of-git)
      * [3.1 Add changes and commit the changes](#31-add-changes-and-commit-the-changes)
      * [3.2 Version rollback](#32-version-rollback)
         * [Rollback before commit](#rollback-before-commit)
         * [Rollback after commit](#rollback-after-commit)
   * [4. Use branches to cooperate](#4-use-branches-to-cooperate)
      * [4.1 Create a new branch](#41-create-a-new-branch)
      * [4.2 Add new features with new branches](#42-add-new-features-with-new-branches)
      * [4.3 Local and remote branches](#43-local-and-remote-branches)
      * [4.4 Push and pull from remote repository](#44-push-and-pull-from-remote-repository)
   * [5. General work flow](#5-general-work-flow)
      * [5.1 Best practice about branches](#51-best-practice-about-branches)
      * [5.2 Working flow](#52-working-flow)


> **_Note_**: 
> 1. This guild assumes that you use linux (ubuntu especially) system, and you know the basics about linux command line
> 2. If you find some typos or you think you can improve some part, just modify this file and push to github
 

## 1. Install git 
```shell
sudo apt install git 
```

After the installation, you need to configure the git with your name and email 
```shell
git config --global user.name "YourName"
git config --global user.email "YourEmail"
```

Then you can check the configuration with 
```shell
git config --list
```

> **_NOTE:_**  This tutorial just demonstrates the best practice of version control using github. The basic and common usage of github will be introduced but it is not a in-depth tutorial for git and github. If you wanna to learn more about git and github, there are many good tutorial for how to use git and github online. Followings are some references:
* [中文教程](https://www.liaoxuefeng.com/wiki/896043488029600)
* [English tutorial](https://git-scm.com/docs/gittutorial)



## 2. Clone a repository from github
First, you'd better add your ssh key to github account for better user experience. 
* To generate a new ssh key, please check [this tutorial](https://docs.github.com/cn/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
* To add the ssh key to your github account, please check [this tutorial](https://docs.github.com/cn/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
* If you have several computers (including virtual machines), please copy the corresponding private ssh key file to ./ssh or generate/add a new key for each computer.

Please read though the [Official tutorial for repo cloning](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository-from-github/cloning-a-repository) for more details. It is better to clone with ssh, otherwise you will need to type your github account and password frequently, which is quite annoying. 

Two things you should be clear before next step:
1. How to generate and upload your ssh public key
2. How to get the copy link from github repository 
3. (optional) What is the difference between clone with https and clone with ssh 

Then, you can clone the test repo from github with ssh:
```shell
git clone git@github.com:CALAS-CityU/github_test.git 
```

After that you can see the repo in the current working directory. Entering the directory, you should see find just a single file named [README.md](https://docs.github.com/cn/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) in this directory
```shell
cd github_test
ls
```

## 3. Basic usage of git
### 3.1 Add changes and commit the changes
Currently there is on a README file in the repo, let's add a test.txt file 
```shell
echo "This is a test file" > test.txt
echo "Another test file" > another_test.txt
```

To make git know the changes you have made, you can use **git add** command. You can add with file name or let just add all the changes
```shell
git add .   # Add all the changes 
git add test.txt another_test.txt # Add with file name 
```

Then you can use **git commit** to save the current version. To save you time, you should add some meaningful message to this version. Otherwise, when you go back to your code 2 months later, it will make you painful.
```shell
git commit -m "add text files"   # provide meaningful message to this mini version 
```

> **_Note_**: To make git know the changes you have make, you need to
> 1. Add the change with git add
> 2  Commit the change with git commit

Each git commit you make form a mini version, git will store the snapshot of that version. You can check the version with 
```shell
git log 
```

### 3.2 Version rollback
A common situation of debugging is that you wanna to go back to a previous edition cause current code cannot execute totally. Git provides this features and help you to manage your work properly. 

#### Rollback before commit
To simulate this situation, let delete the test.txt file in the current git repo
```shell
rm test.txt another_test.txt
ls
```
Now you regret, you wanna your test files back. Then you can rollback to the previous version by
```shell
git checkout -- test.txt another_test.txt
ls
```
The test files will be restored. 

> **_Note_**: Two commands to rollback to previous commit
> * git checkout -- \<filename\> # rollback the corresponding files to the states that store in previous committed version. 
> * git reset --hard HEAD   # Rollback all the files to previous version

#### Rollback after commit
Support your situation are worse, you have committed the changes as following:
```shell
rm test.txt another_test.txt
git add .
git commit -m "remove test files"
``` 
Now you want you test fils back. If you try 
```shell
git checkout -- test.txt another_test.txt
```
A error message will print since the test files do not existing in the previous commit, it exists in the previous commit of previous commit, which is two versions back. However, we can still handle the case by git reset 
```shell
git reset --hard HEAD^
ls
``` 
Now, your test files are back

> **_Note_**: More about version control please check 
> [中文版](https://www.liaoxuefeng.com/wiki/896043488029600/896954074659008)
> [English](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)

## 4. Use branches to cooperate 
You should read through the following materials about git branch first:
* [中文版](https://www.liaoxuefeng.com/wiki/896043488029600/896954848507552)
* [English Version](https://www.atlassian.com/git/tutorials/using-branches)

> **_Note_:** Branch is the most importance concept in github. The rule of thumb that you should remember is that whenever you wanna make some changes to the existing files, create a new branch for the change instead of editing the file directly.


You can check the branches of the current git repository (repo for short) by 
```shell
git branch
``` 
You can see there is a master branch, which is the default branch for all git repository (repo for short), usually master branch is use to store the **stable** version of your project. Meanwhile, we usually use a **develop** branch to store the unstable development version of your project. 


### 4.1 Create a new branch
There is no develop branch in your repo, so you can just create a develop branch by
```shell
git branch develop
```
Then you can see the develop branch
```shell
git branch
```
The new branch will be created based on your current branch, all the file in the current branch will be copy to the new branch. This means that if your current branch is master, the new branch will have the same content as master. And if your current branch is develop, the new branch will have the same file as develop.

Now, let's switch to the develop branch and add new files.
```shell
git switch develop
echo "This is the develop branch" > develop.txt
ls
```
Please change **your name** in the develop file

Then we add and commit the changes
```shell
git add .
git commit -m "add develop file"
```

Commit belongs to branch, which means that the commit will only be save in the current branch. Therefore, if you switch back to master branch, there is no develop.txt file 
```shell
git switch master 
ls
```


### 4.2 Add new features with new branches
Now let's switch back to develop branch
```shell
git switch develop
```

Now you want to add your code to the repo, remember that each time you want to add code, create a new branch. So we create a new branch for your own code and switch to that branch.
```shell
git branch mycode  # You can use any branch name you want
git switch mycode  # Switch to the branch you just create
```
Now add some hello message to the develop.txt file, you should change \<YourName\> to your name
```
echo "Hello from <YourName>" >> develop.txt
cat develop.txt
This is the develop file
Hello from <YourName>
```

Then, remember to add and commit the change 
```shell
git add .
git commit -m "add hello message"
```

The develop.txt file has been changed and committed. You have test the file and it works well. Then you can add this change to the develop branch with **git merge** commands
```shell
git switch develop
git merge mycode
cat develop.txt
This is the develop file 
Hello from YourName
```
Now you can see that the develop.txt file under develop branch has been changed 

### 4.3 Local and remote branches 
But how to upload the changes to github repo and what is the relationship between the local branches and the repo stored in github? 
You can check the all the local and remote branches with
```shell
git branch -a
* develop
  master
  remotes/origin/HEAD -> origin/master
  remotes/origin/develop
  remotes/origin/master
``` 

>**_NOTE_:** HEAD is not a branch, if you want to know more about head please check [中文](https://juejin.cn/post/6844903493078089736) [English](https://stackoverflow.com/questions/2304087/what-is-head-in-git)

There are remote branches, the branches stored in github are origin/master and origin/develop. Each remote repo will have a host name and the default host name for github repo is origin. The branches in a remote repo will be marked as host_name/branch_name

Each local branch can have a corresponding remote branch as the **upstream branch**. Now let's check what is the upstream of our local master branch
```shell
git switch master
git status -sb 
## master...origin/master [ahead 1]
```
You can see that the upstream of local master branch is origin/master, which is the master branch stored in github

But if you check the upstream of the local develop branch, you should see the following output
```shell
git switch develop
git status -sb
## develop  
```
This means that there is no upstream for this local branch, you can set the upstream of this local develop branch to be origin/develop 
```shell
git branch --set-upstream-to=origin/develop develop
Branch 'develop' set up to track remote branch 'develop' from 'origin'.
```



### 4.4 Push and pull from remote repository
Now you think you might be able to push your changes at local develop branch to github
```shell
git push 
To github.com:CALAS-CityU/github_test.git
 ! [rejected]        develop -> develop (non-fast-forward)
error: failed to push some refs to 'git@github.com:CALAS-CityU/github_test.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
Try to push the local branch to github, you will get this error message. This message means that the remote branch have some changes that your local changes do not have. (If there is no error message, you might forget to change your name in the develop.txt file. Please check the previous session)

Therefore, you need to first get the newest version with git pull and fix the conflicts 
```shell
git pull
CONFLICT (add/add): Merge conflict in develop.txt
Auto-merging develop.txt
Automatic merge failed; fix conflicts and then commit the result.``
```
The message shows that there is a conflict between local file and remote file. And you need to resolve the conflict.
Let's check the file with conflict
```shell
cat develop.txt
```
The area enclosing by <<<<<<<<<<<<<<<<<<<<<<<<<<<< and >>>>>>>>>>>>>>>>>>>>>>>>>>>> is the conflict area. You should see other's hello message. Now, edit the develop.txt file with your favorite text editor. You should just append your hello message under other's hello message and save the file. 

Then commit the change and push 
```shell
git add .
git commit -m "add hello message of YourName"
git push
```

You can check the github page and you should see your hello message under the develop.txt file of develop branch

## 5. General work flow
### 5.1 Best practice about branches
* Use master branch for stable, well-tested and well-functional version 
  * Be careful when you decide to push your local repo to origin/master repo 

* Use develop branch for unstable, in-development features or code
  * Merge to develop branch first. After the code is well-tested, we can merge it to master branch 


### 5.2 Working flow
Whenever you want to make some change to the project
1. Pull the latest branch from github with git pull
   1. Otherwise you might have to solve many conflicts, not just a single file show in this guild 
2. Create a new local branch based on the develop branch 
3. Add your code and test it
4. Merge the new code to develop branch 
5. push the updated develop branch to github with git push
6. (optional) delete the local new branch 