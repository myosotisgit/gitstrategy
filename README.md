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
A release branch is essentially a fork of the master branch; it is never merged back to the master branch.

The main intent of a release branch is to freeze the code, and to have a point to fall back to if a hotfix is required in the future (e.g. if a customer needs a critical bugfix without consuming all the changes that have been made on master).

Given a regular major.minor.patch version numbering scheme (e.g. semantic versioning), a release branch should be named release/vX.Y, where X is the major version number and Y is the minor version number.

*Example: release/v1.3*
