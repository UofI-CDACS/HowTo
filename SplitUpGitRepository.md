# Splitting up a Git Repository

Over time repositories tend to grow. At some point imposing a better organization of the repository is necessary. Doing this is GitHub should be easy, doing this in Git should be easy. Figuring out how to do this proved to be more difficult than necessary.

This document documents one way of doing this that worked for me. It appears there may be several methods to do this.

My requirements were simple.

 1. Split certain files into a separate repository
 2. Keep the history of those files in the new repository

Conceptually the steps are as follows:

 1. Create a mirror of the starting repository on your local machine

``` bash
git clone --mirror <original github repository>
```

 2. Create an empty GitHub repository using the GitHub Interface.
 3. Push the mirrored repository to the freshly created GitHub repository. The following severs any link to the old repository while keeping the history. It should now be an independent repository.

 ```bash
     git push --set-upstream <new github repository> master
 ```

 At this point you should have a new repository with all of the files and history from your original repository in GitHub. Repeat the previous steps for each new repository that you want to extract from the original.

 Now you can clone your new repositories and remove the files you don't want in each new repository. Commit and push changes as you would normally.

 At this point you should have the new repositories along with the original. What you do with the original is up to you. I would recommend keeping it around until you are sure you no longer need it.
