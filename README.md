# What is github

GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.  Also, it lets you know when and who made any change in the code and revert back if it's necessary.


## Table of Contents

1. [Concepts of Git](#Concepts-of-Git)
2. [Create Git](#Create-Git)
3. [Branches](#Branches)
4. [Branching Strategies](#Branching-Strategies)
5. [Conflicts](#Conflicts)
5. [Alias](#Alias)
5. [Pull Request (PR)](#PR)
5. [Tags](#Tags)
5. [Stash](#Stash)

### Concepts of git <a name="Concepts-of-Git"></a>

- Keeps track of code history
- Takes 'snapshots' of any file
- The 'snapshots' are triggered by commits
- It can stage files before committing

<img align="center" src="https://miro.medium.com/max/686/1*diRLm1S5hkVoh5qeArND0Q@2x.png" alt="drawing" width="600"/>


Previous image show us that github has 3 status:

- **1.** Working directory, which is where files that are not handled by git. These files are also referred to as "untracked files".
- **2.** Staging area, which are files that are going to be a part of the next commit, which lets git know what changes in the file are going to occur for the next commit.
- **3.** The repository which contains all of a project's commits.  



## Create git <a name="Create-Git"></a>

### Basic commands <a name="Basic-Commands"></a>

- ```git init```: This command turns a directory into an empty Git repository. This is the first step in creating a repository. After running git init, adding and committing files/directories is possible.

    ```
    # change directory to codebase
    $ cd /Users/computer-name/Documents/website
    
    # make directory a git repository
    $ git init
    Initialized empty Git repository in /Users/computer-name/Documents/website/.git/
    ```   

- ```git add files```: Adds files in the to the staging area for Git. Before a file is available to commit to a repository, the file needs to be added to the Git index (staging area). There are a few different ways to use git add, by adding entire directories, specific files, or all unstaged files ```git add .```.

    ```
    $ git add <file or directory name>
    ```

- ```git status```: This command returns the current state of the repository.

    git status will return the current working branch. If a file is in the staging area, but not committed, it shows with git status. Or, if there are no changes it’ll return nothing to commit, working directory clean.
    
    ```
    $ git status
    ```
    
- ```git commit```: Record the changes made to the files to a local repository. For easy reference, each commit has a unique ID.

    It’s best practice to include a message with each commit explaining the changes made in a commit. Adding a commit message helps to find a particular change or understanding the changes.
    
    ```
    # Adding a commit with message
    $ git commit -m "Commit message in quotes"
    ```
    
    **Notes:**
    - In order to append something to the last commit you have made, you can use ```git commit --amend``` **Warning**: this just works with the last commit you have made
    - to delete a commit you have made, you can access to the commit code typing ```git log --oneline --decorate```; after that type ```git reset <commit code>``` and later ```git checkout -- <file>```. **Warning**: Be careful with the reset of commit because working with shared repositories may create conflicts. In conclusion, *this is not the best practice when you are working with other developers on the same project*. ```git reset --hard <commit code>``` delete the commit from every area of the project (repo, staging and working)
    
    
- ```git pull```: To get the latest version of a repository run git pull. This pulls the changes from the remote repository to the local computer.

    ```
    $ git pull <branch_name> <remote_URL/remote_name>
    ```    
    
- ```git push```:Sends local commits to the remote repository. git push requires two parameters: the remote repository and the branch that the push is for.

    ```
    $ git push <remote_URL/remote_name> <branch>

    # Push all local branches to remote repository
    $ git push —all
    ```

- ```git clone```: To create a local working copy of an existing remote repository, use git clone to copy and download the repository to a computer. Cloning is the equivalent of git init when working with a remote repository. Git will create a directory locally with all files and repository history.

    ```
    $ git clone <remote_URL>
    ```
    
- ```git branch```: To determine what branch the local repository is on, add a new branch, or delete a branch.

    ```
    # Create a new branch
    $ git branch <branch_name>

    # List all remote or local branches
    $ git branch -a
    
    # Rename a branch
    $ git branch -m <oldname> <newname>
    
    # Delete a branch
    $ git branch -d <branch_name>
    ```
    
- ```git checkout```: To start working in a different branch, use git checkout to switch branches.

    ```
    # Checkout an existing branch
    $ git checkout <branch_name>

    # Checkout and create a new branch with that name
    $ git checkout -b <new_branch>
    ```
    
    In order to discard a change you have made, you can use ```git checkout -- <file>```. It turns back to the version allowed on the repository **Warning**: this just works if the file you had modified is in the working area but not in the staging area, it means the user have **NOT** made a ```git add```. If ```git add``` have been clicked, you can go to the original version typing ```git reset HEAD <file>``` and later ```git checkout -- <file>```
    
    
- ```git merge```: Integrate branches together. git merge combines the changes from one branch to another branch. For example, merge the changes made in a staging branch into the stable branch.

    ```
    # Merge changes into current branch
    $ git merge <branch_name>
    ```

    If Git has to do a recursive merge, it is necessary to append a new commit wich let us to know what chnages are included in the merge. Also, when thi skind of merge happens, Git try to look for the common root and start to do the recursive merge from it. If this is not possible, this is what is know as a "Conflict"

- ```git revert```: command helps you undo an existing commit. This command is not as agressive as ```git reset``` and is most cmmonly used.

    ```
    git revert --no-commit <commit code or HEAD~1 (being 1 the number of commit you want to revert)>. 
    ```
    
    To confirm the revert just type ```git revert --continue```
    

- ```git rebase```: If <branch> is specified, git rebase will perform an automatic git switch <branch> before doing anything else. Otherwise it remains on the current branch.
    
    ```
    git rebase <receptor> <rebased>
    ```

    Some examples example of a rebase:
    
    1. 
        Before rebase:
        
                      A---B---C topic
                     /
                D---E---F---G master

        ```git rebase master topic```

        After rebase:
        
                            A'--B'--C' topic
                           /
              D---E---F---G master
        
    2.
        Before rebase:

            o---o---o---o---o  master
                 \
                  o---o---o---o---o  next
                                   \
                                    o---o---o  topic
        
        ```git rebase --onto master next topic```

        After rebase:
        
            o---o---o---o---o  master
                |            \
                |             o'--o'--o'  topic
                 \
                  o---o---o---o---o  next
    


### Branches <a name="Branches"></a>

Branch in Git is similar to the branch of a tree. Analogically, a tree branch is attached to the central part of the tree called the trunk. While branches can generate and fall off, the trunk remains compact and is the only part by which you can say the tree is alive and standing. Similarly, a branch in Git is a way to keep developing and coding a new feature or modification to the software and still not affecting the main part of the project. You can also say that branches create another line of development in the project. The primary or default branch in Git is the master branch (similar to a trunk of the tree). As soon as the repository creates, so does the main branch (or the default branch).

<img align="center" src="https://wac-cdn.atlassian.com/dam/jcr:746be214-eb99-462c-9319-04a4d2eeebfa/01.svg?cdnVersion=1583" alt="drawing" width="600"/>

### Branching Strategies <a name="Branching-Strategies"></a>

There are some branching strategies useful when you are working in your project:

1. **Git Flow Branch Strategy**The main idea behind the Git flow branching strategy is to isolate your work into different types of branches. There are five different branch types in total:

    - Main
    - Develop
    - Feature
    - Release
    - Hotfix

    The two primary branches in Git flow are main and develop. There are three types of supporting branches with different intended purposes: feature, release, and hotfix.
    
    **Pros and Cons**:
    
    1.1. Pros
        1.1.1. The various types of branches make it easy and intuitive to organize your work.
        1.1.2. The systematic development process allows for efficient testing.
        1.1.3. The use of release branches allows you to easily and continuously support multiple versions of production code.
    
    1.2. Cons
        1.2.1. Depending on the complexity of the product, the Git flow model could overcomplicate and slow the development process and release cycle.
        1.2.2. Because of the long development cycle, Git flow is historically not able to support Continuous Delivery or Continuous Integration.


2. **The GitHub flow branching strategy** is a relatively simple workflow that allows smaller teams, or web applications/products that don’t require supporting multiple versions, to expedite their work.

    In GitHub flow, the main branch contains your production-ready code.

    The other branches, feature branches, should contain work on new features and bug fixes and will be merged back into the main branch when the work is finished and properly reviewed.
    
    **Considerations**: 
    1. Any code in the main branch should be deployable.
    2. Create new descriptively-named branches off the main branch for new work, such as feature/add-new-payment-types.
    3. Commit new work to your local branches and regularly push work to the remote.
    4. To request feedback or help, or when you think your work is ready to merge into the main branch, open a pull request.
    5. After your work or feature has been reviewed and approved, it can be merged into the main branch.
    6. Once your work has been merged into the main branch, it should be deployed immediately.

    **Pros and Cons**:

     2.1. Pros
         2.1.1. GitHub flow is the most simple.
         2.1.2. Because of the simplicity of the workflow, this Git branching strategy allows for Continuous Delivery and Continuous Integration.
         2.1.3. This Git branch strategy works great for small teams and web applications.

     2.2. Cons
         2.2.1. This Git branch strategy is unable to support multiple versions of code in production at the same time.
         2.2.2. The lack of dedicated development branches makes GitHub flow more susceptible to bugs in production.

    <img align="center" src="https://www.gitkraken.com/img/landing-page/git-flow.svg" alt="drawing" width="300"/>


3. **GitLab Flow Branch Strategy**, at its core, the GitLab flow branching strategy is a clearly-defined workflow. While similar to the GitHub flow branch strategy, the main differentiator is the addition of environment branches—ie production and pre-production—or release branches, depending on the situation.

    Just as in the other two Git branch strategies, GitLab flow has a main branch that contains code that is ready to be deployed. However, this code is not the source of truth for releases.

    In GitLab flow, the feature branch contains work for new features and bug fixes which will be merged back into the main branch when they’re finished, reviewed, and approved.

    **Pros and Cons**:
    
    3.1. Pros
        3.1.1. When compared to the Git flow branch strategy, GitLab flow is more simple.
        3.1.2. GitLab flow is more organized and structured than the GitHub flow branch strategy.
        3.1.3. After slight modification, GitLab flow can allow for Continuous Delivery and versioned releases.
    
    3.2. Challenges of GitLab Flow
        3.2.1. GitLab flow is not the simplest Git branch strategy.
        3.2.2. GitLab flow is not the most structured Git branching strategy which can lead to messy collaboration.

    **Considerations**: 
    1. The GitLab flow branching strategy works with two different types of release cycles:
    2. Versioned Release: each release has an associated release branch that is based off the main branch. Bug fixes should be merged into the main branch first, before being cherry-picked into the release branch.
    
    <img align="center" src="https://www.gitkraken.com/img/landing-page/git-flow-2.svg" alt="drawing" width="300"/>

### Conflicts <a name="Conflicts"></a>

A merge conflict is an event that takes place when Git is unable to automatically resolve differences in code between two commits. Git can merge the changes automatically only if the commits are on different lines or branches.

<img align="center" src="https://www.simplilearn.com/ice9/free_resources_article_thumb/pull-push.JPG" alt="drawing" width="600"/>

To solve this issues git include on the code both versions with the code from the merged branch and the code from the receptor of the merge with '<<<<<<<<<<'code A'========='code B'>>>>>>>>>>>>>>' You have to look for the best solutions and when you have it, ```git add <file>```, later ```git commit -m 'Insert the text of commit```.


### Alias <a name="Alias"></a>

If you go on to use Git with any regularity, aliases are something you should know about. Git doesn’t automatically infer your command if you type it in partially. If you don’t want to type the entire text of each of the Git commands, you can easily set up an alias for each command using git config. Here are a couple of examples you may want to set up:

Some examples are

```
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
$ git config --global alias.lod 'log --online --decorate'
$ git config --global alias.lodag 'log --online --decorate --all --graph'
$ git config --global alias.unstage 'reset HEAD--'
```

**Notes:**

- In order to look for the alias you have created, you should try this ```git config --global --get-regexp alias```
- To delete an alias you have created, try this ```git config --global --unset alias.<name of the alias>``` 

### Pull Request (PR) <a name="PR"></a>

After forking the repository on the remote hosting service and cloning it to your local machine, you can start working on making changes to the code. fter you’re happy with the changes, you can push the work up to your fork and open a pull request that will signal the main repo. This pull request asks the maintainer(s) to review your work, provide comments, request edits, etc.

If your pull request is approved, the maintainer will merge your changes into the main repo.


### Tags <a name="Tags"></a>

Git has the ability to tag specific points in a repository’s history as being important. Typically, people use this functionality to mark release points (v1.0, v2.0 and so on). To asign a tag you can try this:

```
$ git tag V1.1
```

The code shown before will be assigned to the HEAD of the project, but you can asiggn a tag to an oldest version, just doing:

```
$ git tag V1.0 <commit_code>
```

**Notes:**
- In order to look for the tags you have created, you should try this ```git tag```
- You can create annotate tags wich let you to write a longer comment of the tag you are creating. Try this ```git tag -a <tag_name> <commit_code>```. So you can see the complet text of the tag typing ```git show <tag_name>```


### Stash <a name="Stash"></a>

Use git stash when you want to record the current state of the working directory and the index, but want to go back to a clean working directory. The command saves your local modifications away and reverts the working directory to match the HEAD commit.

```
git stash save "Optional Text"
```

After typing the code shown before, your wirking directory will be cleaned and your 'temporary' code is staged on the stash. You can look for the code hosted on the stash with ```git stash list```. To go back to your stashed code use ```git stash apply <stash (Optional)>``` on the branch when you were working before. 

## **Practical hint** : 

If you don't know what kind of command you should type, with ```-h``` or ```--help``` git would give you a list of possible commands you have
