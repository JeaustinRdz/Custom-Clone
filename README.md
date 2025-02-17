# Custom Git Clone and Checkout in Azure DevOps Pipeline

## Overview
This pipeline demonstrates how to perform a **custom Git clone and checkout** using `$(System.AccessToken)` for authentication. Instead of relying on the default Azure DevOps checkout process, it manually clones a specific branch using the `--branch` option in `git clone`.

## Features
- **Custom authentication**: Uses `$(System.AccessToken)` to authenticate with Azure DevOps.
- **Branch-specific cloning**: Uses `--branch <branch>` to fetch only the required branch instead of cloning the entire repository.
- **Runs on Windows-based agents**: The pipeline is configured to use `windows-latest` as the VM image.

## Pipeline Configuration


- task: CmdLine@2
    inputs:
    script: |
        git clone --branch UAT --single-branch https://$(System.AccessToken)@dev.azure.com/{Your_Organization_Name}/{Your_Project_Name}/_git/{Your_Repository_Name}
        cd {Your_Repository_Name}

```

## Explanation
- **`git clone --branch UAT --single-branch <repo>`**: Clones only the `UAT` branch to avoid downloading unnecessary data.
- **`$(System.AccessToken)`**: A predefined Azure DevOps system variable used for authentication.


## Prerequisites
- The `UAT` branch must exist in the remote repository.
- The `$(System.AccessToken)` variable must have sufficient permissions to clone the repository.

## Troubleshooting
### 1. `fatal: Remote branch UAT not found in upstream origin`
- Ensure that the `UAT` branch exists in the repository by running:
  ```sh
  git branch -r
  ```
- If missing, create and push the branch:
  ```sh
  git checkout -b UAT
  git push origin UAT
  ```

### 2. `fatal: Authentication failed`
- Check the permissions of the service account "{Your_Organization_Name} Build Service ({Your_Organization})" that is the account owner of the token $(System.AccessToken)
- Check if the pipeline has the **"Allow scripts to access the OAuth token"** option enabled.

## Summary
This pipeline provides a robust way to **clone a specific branch** securely using Azure DevOps authentication. By avoiding the default checkout step, it offers greater flexibility in managing repositories within your CI/CD workflow.

