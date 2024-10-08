---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "webitel Provider"
subcategory: ""
description: |-
  
---

# webitel Provider



## Example Usage

```terraform
provider "webitel" {
  endpoint = "https://webitel.example.com/api"
  token    = "token"

  insecure = false

  retry {
    attempts = 3
    delay_ms = 2000
  }
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `endpoint` (String) The target Webitel Base API URL in the format `https://[hostname]/api/`. The value can be sourced from the `WEBITEL_BASE_URL` environment variable.
- `token` (String, Sensitive) The authentication token used to connect to Webitel. The value can be sourced from the `WEBITEL_AUTH_TOKEN` environment variable.

### Optional

- `insecure` (Boolean) Explicitly allow the provider to perform "insecure" SSL requests. If omitted, default value is `false`
- `retry` (Block, Optional) Retry request configuration. By default there are no retries. Configuring this block will result in retries if a 420 or 5xx-range status code is received. (see [below for nested schema](#nestedblock--retry))

<a id="nestedblock--retry"></a>
### Nested Schema for `retry`

Optional:

- `attempts` (Number) The number of times the request is to be retried. For example, if 2 is specified, the request will be tried a maximum of 3 times.
- `delay_ms` (Number) The delay between retry requests in milliseconds.
