---
id: provider
title: Getting Started
---

[Bitbucket Server](https://www.atlassian.com/software/bitbucket) is the self-hosted version of Bitbucket.
Whilst terraform provides a default bitbucket provider, this only works for _Bitbucket Cloud_ - this provider
unlocks the power of terraform to manage your self-hosted Bitbucket Server instance. 

## Provider Installation

Download a binary for your system from the [release page](https://github.com/gavinbunney/terraform-provider-bitbucketserver/releases) and remove the `-os-arch` details so you're left with `terraform-provider-bitbucketserver`.
Use `chmod +x` to make it executable and then either place it at the root of your Terraform folder or in the Terraform plugin folder on your system. 

## Provider Configuration

The provider supports parameters to determine the bitbucket server and admin user/password to use.

```hcl
provider "bitbucketserver" {
  server   = "https://mybitbucket.example.com"
  username = "admin"
  password = "password"
}
```

### Authentication

The `username` and `password` specified should be of a user with sufficient privileges to perform the operations you are after.
Typically this is a user with `SYS_ADMIN` global permissions.

### Environment Variables

You can also specify the provider configuration using the following env vars:

* `BITBUCKET_SERVER`
* `BITBUCKER_USERNAME`
* `BITBUCKET_PASSWORD`

> Note: The hcl provider configuration takes precedence over the environment variables.

## Example - Creating a Project and Repository

Creating a project and repository is super simple with this provider:

```hcl
provider "bitbucketserver" {
  server   = "https://mybitbucket.example.com"
  username = "admin"
  password = "password"
}

resource "bitbucketserver_project" "test" {
  key         = "TEST"
  name        = "test-01"
  description = "Test project"
}

resource "bitbucketserver_repository" "test" {
  project     = bitbucketserver_project.test.key
  name        = "repo-01"
  description = "Test repository"
}
```
