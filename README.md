Increment Variable
===

***(!) This is a modified version of https://github.com/action-pack/increment
this version will return `new value` and `old value` of the variable.***

Action to increment a repository variable. Useful for increasing a version number for example.

It also supports alphanumeric variables, for example `ABC1` will be increased to `ABC2`.

If the target variable does not exist, it will be automaticly created.

If you want to increment by another amount than the default (1), you can set the ```amount``` parameter.

## Usage

```YAML
- uses: eriknyk/increment-variable-action@v1.0.0
  with:
    id: version_code
    name: 'VERSION_CODE'
    token: ${{ secrets.REPO_ACCESS_TOKEN }}
```

Then you can use it in following steps like:

```YAML
- name: Set app info
  run: |
    echo "APP_VERSION=2.1.44-${{ steps.version_code.outputs.new_value }}" >> $GITHUB_ENV
    echo "PREV_VERSION=2.1.44-${{ steps.version_code.outputs.old_value }}" >> $GITHUB_ENV
```

## Inputs

### name

**Required** `String` Variable name.

### amount

**Optional** `Integer` Increment by this amount (default = 1).

### token

**Required** `String` Repository [Access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)

### owner

**Optional** `String` Owners name.

### repository

**Optional** `String` Repository name.

### org

**Optional** `Boolean` Indicates the repo is an [organization](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-organizations).

## Outputs

### old_value

`Integer|empty` The value prioud being updated. Returns empty if the variable doesn't exist previously.

### new_value

`Integer` The new value after variable incremented

## FAQ

  * ### Why do I get the error '*Resource not accessible by integration*'?

    This will happen if you use ```secrets.GITHUB_TOKEN```.

    You need to create a [personal access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) instead.

    Go to your Github settings, select 'Developer settings' --> 'Personal access tokens' --> 'Tokens (classic)' and create a new token. Store its value in a secret, for example ```MY_TOKEN```.

    Then refer to it like this:
    
    ```yaml
    token: ${{ secrets.MY_TOKEN }}
    ```