# Terraform Provider Webitel (Terraform Plugin Framework)

_This template repository is built on the [Terraform Plugin Framework](https://github.com/hashicorp/terraform-plugin-framework). The template repository built on the [Terraform Plugin SDK](https://github.com/hashicorp/terraform-plugin-sdk) can be found at [terraform-provider-scaffolding](https://github.com/hashicorp/terraform-provider-scaffolding). See [Which SDK Should I Use?](https://developer.hashicorp.com/terraform/plugin/framework-benefits) in the Terraform documentation for additional information._

## Requirements

- [Terraform](https://developer.hashicorp.com/terraform/downloads) >= 1.0
- [Go](https://golang.org/doc/install) >= 1.23.1

## Using the provider

This is a small example of how to create contact. Please read the [documentation](./docs) for more
information.

```hcl
provider "webitel" {
  endpoint = "https://webitel.example.com/api"
  token    = "token"

  insecure = false

  retry {
    attempts = 3
    delay_ms = 2000
  }
}

resource "webitel_contact" "example" {
  name   = "foo"
  about  = "about foo bar"
  labels = ["label-foo", "label-bar"]

  variables = [
    {
      key   = "foo-key"
      value = "foo-value"
    }
  ]

  phones = [
    {
      code        = "1"
      destination = "123"
    }
  ]
}
```

## Developing the Provider

If you're new to provider development, a good place to start is the [Extending
Terraform](https://www.terraform.io/docs/extend/index.html) docs.

If you wish to work on the provider, you'll first need [Go](http://www.golang.org) installed on your machine (see [Requirements](#requirements) above).

Terraform allows to use local provider builds by setting a `dev_overrides` block in a configuration 
file called `.terraformrc`. This block overrides all other configured installation methods. Terraform searches for 
the `.terraformrc` file in your _home directory_ and applies any configuration settings you set.

1. Find the `GOBIN` path where Go installs your binaries. 
Your path may vary depending on how your Go environment variables are configured.

```shell
$ go env GOBIN
/Users/<Username>/go/bin
```

2. Create a new file called `.terraformrc` in your home directory (`~`), then add the `dev_overrides` block below. 
Change the `<PATH>` to the value returned from the go env `GOBIN` command above.

```hcl
provider_installation {
  dev_overrides {
    "webitel/webitel" = "<PATH>" # this path is the directory where the binary is built
  }
          
  # For all other providers, install them directly from their origin provider
  # registries as normal. If you omit this, Terraform will _only_ use
  # the dev_overrides block, and so no other providers will be available.
  direct {}
}
```

3. Use the `go install` command from the repository's root directory to compile the provider into a binary and install 
it in your `GOBIN` path. Terraform will use the binary you just built for every `terraform plan`/`apply` (it should print 
out a warning). No need to run `terraform init`.

In order to run the full suite of Acceptance tests, run `make testacc`.

*Note:* Acceptance tests create real resources, and often cost money to run.

```shell
make testacc
```

## Documentation

Documentation is generated with
[tfplugindocs](https://github.com/hashicorp/terraform-plugin-docs). Generated
files are in `docs/` and should not be updated manually. They are derived from:

- Schema `Description` fields in the provider Go code.
- [examples/](./examples)

Use `go generate ./..` to update generated docs.

## Releasing

Builds and releases are automated with GitHub Actions and
[GoReleaser](https://github.com/goreleaser/goreleaser/).

Currently there are a few manual steps to this:

1. Kick off the release:

   ```sh
   RELEASE_VERSION=v... \
   make release
   ```

2. Publish release:

   The Action creates the release, but leaves it in "draft" state. Open it up in
   a [browser](https://github.com/webitel/terraform-provider-webitel/releases)
   and if all looks well, click the `Auto-generate release notes` button and mash the publish button.