# renovate-reproduction/33370

Reproduction of [Renovate discussion 33370](https://github.com/renovatebot/renovate/discussions/33370).

## Current Behavior

Renovate does not properly parse the `packageName` using the `terragrunt` manager
when the source refers to a sub-directory (\_i.e., contains double slashes in the
URL path) and the source protocol is either missing or is not one of `http` or
`https`.

Given the following `terragrunt.hcl` file:

```hcl
terraform {
  source = "git::ssh://git@mygit.com/hashicorp/example.git//subdir/test?ref=v1.0.4"
}
```

The `terragrunt` manager extracts the following dependency

```json
{
  "deps": [
    {
      "currentValue": "v1.0.4",
      "datasource": "git-tags",
      "depName": "mygit.com/hashicorp/example",
      "depType": "gitTags",
      "pacakgeName": "null/hashicorp/example.git"
    }
  ]
}
```

## Expected Behavior

Support sources referring to a sub-directory _and_ `ssh` as the protocol.

```json
{
  "deps": [
    {
      "currentValue": "v1.0.4",
      "datasource": "git-tags",
      "depName": "mygit.com/hashicorp/example",
      "depType": "gitTags",
      "pacakgeName": "ssh://mygit.com/hashicorp/example.git"
    }
  ]
}
```
