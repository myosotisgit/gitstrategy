# Myosotis Git Strategy

Git strategy used by Myosotis ICT B.v.
A summary of the current git strategy used at Myosotis for its software projects. Based upon the GitFlow and Stable Mainline (Marcus Geelnard) methods. http://www.bitsnbites.eu/a-stable-mainline-branching-model-for-git/

## Structure

This models has 3 branches. 

* Master Branch
* Feature Branches
* Release Branches

For most purposes the 'master' branch is considered stable. In other words, if you check out the master branch you can expect that it builds correctly, passes all tests and you should be able to work with the end application.

### Feature branches
All development is done in dedicated (relatively short lived) feature branches. A feature branch branches off from master, and once development is finished and all the integration criteria have been met, it is merged back to the master branch.

A feature branch is typically called feature/ticket_description, where ticket is a reference to the corresponding ticket in the project issue tracker (if applicable), and description is a very short description (up to five words or so) of the purpose of the branch.

Example: feature/triage-filter-block

### Release branches
