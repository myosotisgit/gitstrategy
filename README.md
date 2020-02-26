# Myosotis Git Strategy

Git strategy used by Myosotis ICT B.v.
A summary of the current git strategy used at Myosotis for its software projects. Based upon the GitFlow and Stable Mainline (Marcus Geelnard) methods. http://www.bitsnbites.eu/a-stable-mainline-branching-model-for-git/

## Structure

This models has 3 branches. 

* Master Branch
* Feature Branches
* Release Branches

For most purposes the 'master' branch is considered stable. In other words, if you check out the master branch you can expect that it builds correctly, passes all tests and you should be able to work with the end application.

![stable mainline branching](/images/stable-mainline-branching-model.png)

### Feature branches
All development is done in dedicated (relatively short lived) feature branches. No development is done directly on master!. A feature branch branches off from master, and once development is finished and all the integration criteria have been met, it is merged back to the master branch.

A feature branch is typically called **feature/ticket_description**, where ticket is a reference to the corresponding ticket in the project issue tracker (if applicable), and description is a very short description (up to five words or so) of the purpose of the branch.

**Example: feature/triage-filter-block**

### Release branches
A release branch is essentially a fork of the master branch; it is never merged back to the master branch.

The main intent of a release branch is to freeze the code, and to have a point to fall back to if a hotfix is required in the future (e.g. if a customer needs a critical bugfix without consuming all the changes that have been made on master).

Given a regular **major.minor.patch** version numbering scheme (e.g. semantic versioning), a release branch should be named **release/vX.Y**, where X is the major version number and Y is the minor version number.

**Example: release/v1.3**

### Release tags
In addition to release branches, release tags are created for each actual release (this may include release candidates that are intended for QA or beta testing, as well as public or customer releases). The release tags are made in the corresponding release branch.

The commit that represents a specific release is tagged with a tag named **vX.Y.Z**, optionally suffixed with a textual identifier, such as **-rc1, -beta** or **-some-customer**.

**Example: v1.3.1-rc2**

## Creating a release
There are essentially two kinds of releases:

* Standard releases, that include new features from master.
* Patch releases, that fix bugs or problems in earlier releases.

The main difference between the two is that standard releases are based on master, while patch releases are based on previous release branches. Otherwise the processes for the two are very similar.

### Standard Release
The initial purpose of the release branch is to feature-freeze the release, while keeping the master branch open for continued development.

#### Fixing bugs on the release branch
Any bugs that are found in a release candidate can be fixed in one of two ways.

##### Proper fix on master
The preferred way to fix a release bug is to implement the fix on master, using the conventional feature development process, and then cherry-pick the bug fix commit(s) to the release branch.

The advantages are that the fix will automatically be included in future releases, and it will be subject to the standard integration process (i.e. all the regular quality measures apply).

##### Quick-fix on the release branch
If the fix is too complex to implement properly in time for the release (e.g. if the bug is a symptom of a bigger architectural problem), or if the corresponding fix on master would be incompatible with the release branch, one option is to make a quick-fix on the release branch.

When doing so, it is important to create a ticket for fixing the bug properly on master, and preferably start working on it right away.

### Patch release
A patch release is created from an existing release branch. For instance, if v1.2.0 needs a critical patch, the patch is applied on top of the release/v1.2 branch, and then the regular release stabilization process is performed (except that tags now use the name of the new version number, for instance v1.2.1-rc1)...

# Git Cheat Sheet

* Markdown: https://guides.github.com/features/mastering-markdown/

# Setting up Clone / Mirror repos
* Github duplicating repos: https://help.github.com/en/articles/duplicating-a-repository

As with a bare clone, a mirrored clone includes all remote branches and tags, but all local references will be overwritten each time you fetch, so it will always be the same as the original repository.

## 1. Create a bare mirrored clone of the repository.

`git clone --mirror <url to git repo.git>`

This will clone an existing git repo into a **bare** local repository that is identical to the upstream repo.
--> On servers behind a firewall you need to use the https link to the repository!

## 2. Set the push location to your mirror.

`cd repository-to-mirror.git`

`$ git remote set-url --push origin https://github.com/exampleuser/mirrored`

Setting the URL for pushes simplifies pushing to your mirror; to update your mirror, fetch updates and push. Only needed for development environments.
 
## 3. Get latest changes from origin (remote repo)

`git fetch -p origin`

`git push --mirror`*

> * *This will push to ALL remote branches. So WATCH OUT when you have production servers defined!*

## 3.1 Update existing root folder

`git --work-tree=/var/www/webrootfolder/html --git-dir=<repo folder>/repo.git checkout -f`

# Preview a previous commit
If, for some reason, a recent commit breaks something you can pinpoint the exact commit where it happened by running `git commit xxxxx`.
Git will undo all the previous commits UPTO the commit specified (iow, HEAD will point to the commit specified). Once you have located the bug, checkout master again and fix it, or "revert" the commit(s).

`git checkout <commit hash>`

To view all commit hashes use `git log --oneline`. Dont forget to `git checkout master` when you are done!

# Revert a Commit

`git revert <commit hash>`

And afterwards switch back to the working branch: `git checkout <working branch`

# Deleting Branches
## Summary - Deleting Branches (local/remote) 
Remote: `git push --delete <remote_name> <branch_name>`

Local: `git branch -d <branch_name>`

Note that in most cases the remote name is origin.

## Delete Local Branch
To delete the local branch use one of the following:

`git branch -d branch_name`

`git branch -D branch_name`

Note: The -d option is an alias for --delete, only deletes the branch if fully merged. Use -D, which is an alias for --delete --force, which deletes the branch "irrespective of its merged status."

## Delete Remote Branch
As of Git v1.7.0, you can delete a remote branch using

`git push <remote_name> --delete <branch_name>`

which might be easier to remember than

`git push <remote_name> :<branch_name>`

which was added in Git v1.5.0 "to delete a remote branch or a tag."

Starting on Git v2.8.0 you can also use git push with the -d option as an alias for --delete.
Therefore, the version of Git you have installed will dictate whether you need to use the easier or harder syntax.

## Remove "ghost" branches (cached locally)
When a remote branch is removed from origin it might still exists on local machines. Use --prune to remove these 'ghosts' branches.

`git fetch --all --prune`





# Sync to other repositories (Staging to client/live system)
* https://www.digitalocean.com/community/tutorials/how-to-use-git-hooks-to-automate-development-and-deployment-tasks#using-git-hooks-to-deploy-to-a-separate-production-server

To easily 'update' existing software trees (on client servers, live systems) you can use a cloned bare repository to push changes to these remote servers. For example:

GitHub | Staging server | client server | webroot
------------ | ------------- | ------------- | -------------
[GitHub repo] --> | [Bare mirror clone] --> | [Bare clone] --> | [software tree]

The [Bare Mirror clone] is updated with `git fetch`. The [Bare clone] is updated with `git push [Bare clone]`. And the software tree is updated automatically using the `hooks/post-receive` file (created inside the bare repo).


## Create local bare repo (on client/live system)
Create a folder for the git repo in /home/user/repos/repo.git and initialize the repo.

`mkdir -p /home/user/repos/repo.git`

`cd repo.git`

`git init --bare`


## post-receive (on client/live system)
To automate the synchronize we use a “hook” that comes with git. This hook will “fire” when it received a remote push from the development server. This hook will only synchronize the master branch and not other branches!


`cd bare-clone`
`vi hooks/post-receive`

Add this code to post-receive. Make sure to change the paths to your situation!!
This bash code will check if the remote push was for the “master” branch and only then will it synchronize to the htdocs folder. And the text between "echo" will be shown in the git output during the push command on the pushing server.

```php
#!/bin/bash
while read oldrev newrev ref
do
    if [[ $ref =~ .*/master$ ]];
    then
        echo "Master branch update request received from upstream "
        echo "Deploying master to local folder"
        git --work-tree=/var/www/pathtowebroot/html --git-dir=/home/user/repos/repo.git checkout -f
        echo "Finished receiving Master branch."
    else
        echo "Update Request: $ref successfully received.  Doing nothing: only the master branch may be deployed on this server."
    fi   
done
```

`chmod 755 post-receive`


## Add remote and initialize (on development server) 
The last step is to add the production git to development repo as a “remote server”. This can easily be done by using the built-in git command:

`git remote add production ssh://demo@server_domain_or_IP:PORT/home/git.repo`

For example:

`git remote add **clientrepo** ssh://user@client.systen.com:22/home/clientaccount/repos/repo.git`

To initialize and copy the master branch to the new bare repo we push the branch

`git push --set-upstream **clientrepo** master`

After the initialize we can, from the development server, push the master like this.

`git push **clientrepo** master`

This will only work when you are in the original repo (/home/user/repos/repo.git)

### Ref
* Git remove branches: https://stackoverflow.com/a/2003515

