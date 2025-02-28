# Private Action Loader

[![Actions Status](https://github.com/invisionapp/private-action-loader/workflows/ci/badge.svg?branch=develop)](https://github.com/invisionapp/private-action-loader/actions)
[![License: MIT](https://img.shields.io/badge/license-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)
[![Dependency Status](https://david-dm.org/InVisionApp/private-action-loader.svg)](https://david-dm.org/InVisionApp/private-action-loader)
[![devDependency Status](https://david-dm.org/InVisionApp/private-action-loader/dev-status.svg)](https://david-dm.org/InVisionApp/private-action-loader#info=devDependencies)

This action loads and executes a private Action.  This allows private actions to be reused in other private repositories.  All inputs are passed down into the private action. All outputs of the private actions are passed back to the loader action.

---
## **Inputs**
### **`repo-token`**

**Required** An access token with the [repo](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) scope to the repository containing the action.  **This should be stored as a [repo secret](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets)**.

### **`repo-name`**

**Required** The repository containing the action.  A ref can also be appended to specify an exact commit of the action (SHA, tag, or branch). Must be in the format of `{owner}/{repo}` or `{owner}/{repo}@{sha}`.

### **`action-dir`**
The folder containing the action. If you have a repository with multiple actions, you can specify the subfolder containing the action.
e.g `actions/my-first-action`


---
## **Examples**
## Example usage w/ additional parameters
``` yaml
    - uses: invisionapp/private-action-loader@v1
      with:
        repo-token: ${{ secrets.REPO_TOKEN }}
        repo-name: some-org/super-secret-action
        # the following input gets passed to the private action
        input-used-by-other-action: this will be passed to super-secret-action
```

## Example usage w/o additional parameters
``` yaml
    - uses: invisionapp/private-action-loader@v1
      with:
        repo-token: ${{ secrets.REPO_TOKEN }}
        repo-name: some-org/super-secret-action
```

## Example usage w/ SHA
``` yaml
    - uses: invisionapp/private-action-loader@v1
      with:
        repo-token: ${{ secrets.REPO_TOKEN }}
        repo-name: some-org/super-secret-action@b8a83a0
```

## Example usage w/ Branch
``` yaml
    - uses: invisionapp/private-action-loader@v1
      with:
        repo-token: ${{ secrets.REPO_TOKEN }}
        repo-name: some-org/super-secret-action@master
```

## Example usage w/ Tag
``` yaml
    - uses: invisionapp/private-action-loader@v1
      with:
        repo-token: ${{ secrets.REPO_TOKEN }}
        repo-name: some-org/super-secret-action@v1
```

## Example usage w/ actions folder
``` yaml
    - uses: invisionapp/private-action-loader@v1
      with:
        repo-token: ${{ secrets.REPO_TOKEN }}
        repo-name: some-org/our-secret-actions@v1
        action-dir: actions/the-best-action
```

## Example usage w/ Output
``` yaml
    - uses: invisionapp/private-action-loader@v1
      id: output_example
      with:
        repo-token: ${{ secrets.REPO_TOKEN }}
        repo-name: some-org/super-secret-action@v1
    - name: Get the previous output
      run: echo "The previous output was ${{ steps.output_example.outputs.<name of output> }}"
```
---
## **Limitations**
* Only supports javascript actions
* There are no tests yet
* There is very little error handling so far
