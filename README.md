## Err for theme/PaperMod

Use this [link](https://confluence.atlassian.com/bbkb/fix-error-fatal-no-url-found-for-submodule-path-in-gitmodules-1188400518.html). Or this text:

# Fix error "fatal: No url found for submodule path in .gitmodules"

## Purpose

The purpose of this KB article is to provide context as to why this issue may occur and how to address this.

## Diagnosis

- A user updates their **.gitmodules** file to point to a new path for their submodule in another repository

- The user then attempts to update the git submodules in the remote repository (either using GIT or in Bitbucket Cloud Pipelines) by using some variation of the following command:
  
  git submodule update --init --recursive
  

- They are then met with the following error message - which points to the old path that was previously referenced in their .gitmodules file:
  
  fatal: No url found for submodule path 'x' in .gitmodules
  

## Procedure

Essentially, the root cause of the issue is that either the old submodule path remains cached (and must be be removed) or a path does not exist (and must be created) before the submodules may be updated.

**To check this, you will need to perform the following on your local repository:**

1. Check the currently configured git submodules by executing the following to verify the submodule path status:
  
  **git submodule status**
  

**<u>If the old path is present:</u>  
 **You will need to execute the following command to remove this from the cache:

**git rm --cached old/path**

**<u>If no path is present:</u>**  
 To create the mapping reference, enter the following into your .gitmodules file in the root of your repository:

```
[submodule "path_to_submodule"] 
  path = path_to_submodule 
  url = git://url-of-source/
```

**`path_to_submodule:`** is the actual path within your repository (relative to the root) where the submodule will be used**`url-of-source:`** is the URL of the original repository that contains the submodule's files.

2. Once the above has been actioned, double-check the path configuration before updating the submodules:
  
  **git submodule status**
  

3. Once complete, execute the same command as prior - you should no longer see the error message:
  
  **git submodule update --init --recursive**
  

4. Now that the .gitmodules file has been updated with the new path/old path has been removed from cache/updated in the config - you will need to commit the changes and push them to your remote repository to resolve the issue.