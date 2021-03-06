---
title: "HipChat"
description: "Send notification to a HipChat room for Sensu events."
product: "Sensu Enterprise"
version: "3.4"
weight: 7
menu:
  sensu-enterprise-3.4:
    parent: integrations
---
**ENTERPRISE: Built-in integrations are available for [Sensu Enterprise][1]
users only.**

- [Overview](#overview)
- [Configuration](#configuration)
  - [Example(s)](#examples)
  - [Integration Specification](#integration-specification)
    - [`hipchat` attributes](#hipchat-attributes)

## Overview

Send notifications to a [HipChat][2] room for events. After [creating a HipChat
API token][3], configure the handler (integration) with the provided API token.

## Configuration

### Example(s) {#examples}

The following is an example global configuration for the `hipchat` enterprise
event handler (integration).

{{< highlight json >}}
{
  "hipchat": {
    "api_token": "L7kVQzXF7c5eUMYUon6INaSVRDU8mP",
    "api_version": "v2",
    "username": "sensu",
    "room": "Operations",
    "timeout": 10
  }
}
{{< /highlight >}}

### Integration Specification

#### `hipchat` attributes

The following attributes are configured within the `{"hipchat": {} }`
[configuration scope][4].

api_token    | 
-------------|------
description  | The HipChat API token - [https://www.hipchat.com/docs/api/auth][3].
required     | true
type         | String
example      | {{< highlight shell >}}"api_token": "L7kVQzXF7c5eUMYUon6INaSVRDU8mP"{{< /highlight >}}

server_url   | 
-------------|------
description  | The URL of the HipChat server (used for self-hosted HipChat installations)
required     | false
type         | String
example      | {{< highlight shell >}}"server_url": "https://hipchat.example.com"{{< /highlight >}}

api_version  | 
-------------|------
description  | The HipChat API version to use.
required     | false
type         | String
default      | `v2`
example      | {{< highlight shell >}}"api_version": "v2"{{< /highlight >}}


username     | 
-------------|------
description  | The HipChat username to use to notify the room.
required     | false
type         | String
default      | `sensu`
example      | {{< highlight shell >}}"username": "monitoring"{{< /highlight >}}

room         | 
-------------|------
description  | The HipChat room to notify.
required     | false
type         | String
default      | `sensu`
example      | {{< highlight shell >}}"room": "Search"{{< /highlight >}}

notify       | 
-------------|------
description  | Configures whether notifications sent from Sensu Enterprise to HipChat should trigger a user notification (change the tab color, play a sound, notify mobile phones, etc). Each recipient's notification preferences are taken into account.
required     | false
type         | Boolean
default      | false
example      | {{< highlight shell >}}"notify": true{{< /highlight >}}

filters        | 
---------------|------
description    | An array of Sensu event filters (names) to use when filtering events for the handler. Each array item must be a string. Specified filters are merged with default values.
required       | false
type           | Array
default        | {{< highlight shell >}}["handle_when", "check_dependencies"]{{< /highlight >}}
example        | {{< highlight shell >}}"filters": ["recurrence", "production"]{{< /highlight >}}

severities     | 
---------------|------
description    | An array of check result severities the handler will handle. _NOTE: event resolution bypasses this filtering._
required       | false
type           | Array
allowed values | `ok`, `warning`, `critical`, `unknown`
default        | {{< highlight shell >}}["warning", "critical", "unknown"]{{< /highlight >}}
example        | {{< highlight shell >}} "severities": ["critical", "unknown"]{{< /highlight >}}

timeout      | 
-------------|------
description  | The handler execution duration timeout in seconds (hard stop).
required     | false
type         | Integer
default      | `10`
example      | {{< highlight shell >}}"timeout": 30{{< /highlight >}}



[?]:  #
[1]:  /sensu-enterprise
[2]:  https://www.hipchat.com?ref=sensu-enterprise
[3]:  https://www.hipchat.com/docs/api/auth?ref=sensu-enterprise
[4]:  /sensu-core/1.2/reference/configuration#configuration-scopes
