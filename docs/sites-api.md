---
title: Site provisioning API reference
---

import {Required, Optional} from '../src/js/api-helpers.js';

The Plausible Site provisioning API offers a way to create and manage your sites in your Plausible account programmatically.
It currently allows 3 operations:

* Create a new site
* Delete an existing site
* Find or create a shared link by name

Each request must be authenticated with an API key using the Bearer Token method. Currently, this API is in private preview and therefore it's not
possible to acquire an API key through the dashboard. Please [contact us](https://plausible.io/contact) to get an API key with permissions for the endpoints listed in this document.

## Endpoints

### POST /api/v1/sites

Creates a site in your Plausible account.

```bash title="Try it yourself"
curl -X POST https://plausible.io/api/v1/sites \
  -H "Authorization: Bearer ${TOKEN}" \
  -F 'domain="test-domain.com"' \
  -F 'timezone="Europe/London"'
```

```json title="Response 200 OK"
{
    "domain": "test-domain.com",
    "timezone": "Europe/London"
}
```

#### Post body parameters
<hr / >

**domain** <Required />

Domain of the site to be created in Plausible. Must be a globally unique, the request will fail if the domain is already taken.
<hr / >

**timezone** <Optional />

Timezone name according to the [IANA](https://www.iana.org/time-zones) database. Defaults to `Etc/UTC` when left blank.
<hr / >

### DELETE /api/v1/sites/:site_id

Deletes a site from your Plausible account along with all of it's data and configuration. The API key must belong to the owner of the site.

```bash title="Try it yourself"
curl -X DELETE https://plausible.io/api/v1/sites/test-domain.com \
  -H "Authorization: Bearer ${TOKEN}"
```

```json title="Response 200 OK"
{
    "deleted": "true"
}
```

### PUT /api/v1/sites/shared-links

Finds or creates a shared link for a given `site_id` (use the site domain as the ID). This endpoint is idempotent, it will not fail
if a shared link with the provided name already exists.


```bash title="Try it yourself"
curl -X PUT https://plausible.io/api/v1/sites/shared-links \
  -H "Authorization: Bearer ${TOKEN}" \
  -F 'site_id="test-domain.com"' \
  -F 'name="Wordpress"'
```

```json title="Response 200 OK"
{
    "name": "Wordpress",
    "url": "https://plausible.io/share/site.com?auth=<random>"
}
```

#### Body parameters
<hr / >

**site_id** <Required />

Id of your site in Plausible.

<hr / >

**name** <Required />

Name of the shared link.

<hr / >
