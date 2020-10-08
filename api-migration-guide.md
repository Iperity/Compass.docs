---
layout: doc-with-toc
title: API migration guide
permalink: /api-migration-guide.html
description: A developers guide to migrate to the latest Compass API version.
---
# API versioning

## Versioning in the REST API

For basic information on the REST interface, read the <a href="developers-manual.html#{{site.compass.reseller.prodname | append: " REST Interface" | slugify}}">Developers Manual</a>.

The Compass REST API uses versioning. Whenever a breaking change is made, we introduce a new API version. This ensures existing applications can continue to function properly after the release of the new API version, while they adapt their code to conform to these latest API changes.

Currently the API supports versions v1, v2 and v3. Together with the introduction of v3, we are deprecating v1 and v2 so it's important for you to start the process of migrating to v3. This document will describe the changes from v1 to v2 and from v2 to v3, to aid you in a smooth migration.

For the technical details about how to implement versioning, please refer to the documentation at <a href="developers-manual.html#using-the-api">Using The Api</a>. To summarize: please ensure that you always pass an `Accept` header in API requests that matches with the API version(s) that your application can accept, e.g.:

```bash
curl "https://rest.{{site.compass.reseller.domain}}/company/1" -H "Accept: application/vnd.iperity.compass.v3+json"
```

## Versioning in the XMPP API

See <a href="developers-manual.html#{{site.compass.reseller.prodname | append: " XMPP API" | slugify}}">Developers Manual</a>.

The Compass XMPP API currently does not know the concept of versioning. We will ensure that there will not be any hard breaking changes, but please keep reading for an overview of functional changes you should be aware of.

# From v2 to v3

For a complete overview of both API v2 and v3 endpoints, please refer to: <https://rest.{{site.compass.reseller.domain}}/schema/>

## Queue pause

API v1 and v2 contain the concept of "queue pause". Pause is a property of a _queue membership_, e.g. an agent can be a member of two queues but only be paused in one of them. The agent will not receive any queue calls from the queues where they are paused.

In API v3 the concept of "queue pause" no longer exists. Instead, we introduce the concept of "user status" which dictates whether users will receive _all calls_, _only direct calls_ (e.g. no queue calls), or _none_ at all. The status is global for all of the user's queue memberships, they will either receive calls from all of their queues or from none of them.

The following has been removed in API v3:

* The `/identity/{id}/pauseQueue` endpoint
* The `/identity/{id}/unpauseQueue` endpoint
* The property `paused` in `/identity/{id}/queueMemberships`

The following replacements were added:

* GET `/user/{id}/status`
* POST `/user/{id}/status`

Both v2's "queue pause" and v3's "user status" influence whether the queue engine will offer a queue call to an agent. However, "user status" is simply ignored in v1 or v2: if a user's status in v3 is set to `receiveOnlyDirect` this will _not_ make their queue memberships in v1 or v2 display as `paused`. Conversely, the concept of "queue pause" is simply ignored in v3: setting all of a user's queue memberships to `paused` in v1 or v2 will _not_ make the user's status in v3 display as `receiveOnlyDirect`.

Therefore, it is critical that **within the same company, either all applications make use of API v1/v2's queue pause, or all applications make use of API v3's user status**. Failing to abide by this would cause the situation where users don't receive any (queue) calls while your application is unable to discover the reason for this. To assist you in a smooth migration, we perform some automatic migration steps on your behalf. As soon as you use the v3 `/user/{id}/status` endpoint, we remove the v2 pause of all of that user's queue memberships. Conversely, as soon as you use v2's `/identity/{id}/pauseQueue` or `/identity/{id}/unpauseQueue`, we set that user's v3 user status to `receiveAll`.

Additionally, if your company makes use of the starcode functionality for pause/unpause, make sure to match them up with the API version you use: `*49` in case of API v1/2 (queue pause) and `*50` in case of API v3 (user status). Refer to the [Service Codes section](user-admin-manual.html#service-codes) in the User & Admin manual for more details.

### XMPP API

When you make use of API v3 (user status), the `pausedSince` property in queueMember entries won't be set.

### User events

When you make use of API v3 (user status), no `pause` or `unpause` user events will be generated. Instead, `status` user events will represent the changes in user status.

## Endpoints that return a callid

In API v1 and v2 the call-control related endpoints return a boolean indicating succes.

In API v3 these endpoints return the callid of the call that was started. If you were checking the return value of these methods, please switch to checking the HTTP status code instead.

This concerns:

* `/user/{id}/dialNumber`
* `/user/{id}/dialUser`
* `/user/{id}/pickupQueueCall`

# From v1 to v2

For a complete overview of both API v1 and v2 endpoints, please refer to: <https://rest.{{site.compass.reseller.domain}}/schema/>

## Outbound CLI and DID renamed to External Number

In API v1 there are separate types for `externalNumber` (represented by `/externalNumber/{id}`) and `outboundCli` (represented by `/did/{id}`), the first being an external number on which the company can receive inbound calls and the latter being a number that can be used as callerID number for outbound calls.

In API v2, these two types are unified into a single `externalNumber` type. The properties `useAsCli` and `useForInbound` can be used as a discriminator, though at the time of writing simply every external number can be used both as Cli and Inbound.

To migrate:

* Replace calls to GET `/did/{id}` with `/externalNumber/{id}`
* Replace calls to GET `/company/{id}/outboundClis` with `/company/{id}/resourcesFiltered?filter=externalNumber`
* Replace calls to GET `/company/{id}/fullOutboundClis` with `/company/{id}/externalNumbers`
* In PATCH `/identity/{id}`, replace `outboundCli` (with value `/did/{id}`) with `cli` (with value `/externalNumber/{id}`)
* In GET `/identity/{id}`, read `cli` (with value `/externalNumber/{id}`) instead of `outboundCli` (with value `/did/{id}`)

## Removal of */resources endpoints

In API v1 all entity types (company, entity, reseller, and user) have an accompanying `/resources` endpoint, which will enumerate _all_ resources that are a child of this entity.

In v2, these endpoints have been removed. If you were using them, please switch to `/resourcesFiltered` instead.

This concerns:

* `/company/{id}/resources`
* `/entity/{id}/resources`
* `/reseller/{id}/resources`
* `/user/{id}/resources`

## Removal of full* endpoints

In API v1 we offer both "base" and "full" endpoints for a company's children, e.g. `/company/{id}/queues` and `/company/{id}/fullQueues`. The "base" endpoint only returns the child's most basic attributes, while the "full" endpoint returns all available attributes.

In API v2 the "base" endpoints have been removed. If you were using them, either switch to the "full" endpoint or alternatively use `/company/{id}/resourcesFiltered` or `/company/{id}/entitiesFiltered` to retrieve only the "base" information.

This concerns:

* `/company/{id}/fullDpSwitches`
* `/company/{id}/fullExternalNumbers`
* `/company/{id}/fullOutboundClis`
* `/company/{id}/fullQueues`
* `/company/{id}/fullUsers`

## Only base information in the resourcesFiltered endpoint

In API v1 the `resourcesFiltered` endpoints returns "full" information, e.g. all available attributes in that resource.

In API v2 it will only return "base" information: the resource's most basic attributes. If you need full information, switch to that resource's dedicated endpoint like `/company/{id}/queues`, `/company/{id}/forwards`.

This concerns:

* `/company/{id}/resourcesFiltered`
* `/entity/{id}/resourcesFiltered`
* `/reseller/{id}/resourcesFiltered`
* `/user/{id}/resourcesFiltered`

## Removal of /*/id and */type endpoints

In API v1 each resource and entity had a dedicated "id" endpoint, e.g. `/queue/{id}/id`. Additionally, each entity had a dedicated "type" endpoint, like `/reseller/{id}/type`.

In API v2 these redundant endpoints have been removed. If you need to know an object's id or type, simply request the corresponding primary endpoint such as `/queue/{id}` and look at the `theType` and `resourceId`/`entityId` properties.

This concerns:

* `/company/{id}/id`
* `/did/{id}/id`
* `/dpSwitch/{id}/id`
* `/entity/{id}/id`
* `/extension/{id}/id`
* `/externalNumber/{id}/id`
* `/identity/{id}/id`
* `/phone/{id}/id`
* `/queue/{id}/id`
* `/reseller/{id}/id`
* `/resource/{id}/id`
* `/user/{id}/id`
* `/voicemail/{id}/id`

And:

* `/company/{id}/type`
* `/entity/{id}/type`
* `/reseller/{id}/type`
* `/user/{id}/type`

## Removal of shortname property from all entities

In API v1 all entity types have a `shortname` property. However, that property is only valid for companies and contained bogus values for other entity types.

In API v2 the `shortname` property was removed for all other entity types, e.g. `reseller`, `user` and `entity`.

## Extension property in identities

In API v1 an identity's `extension` property contains the extension number, e.g. "2355".

In API v2 that `extension` property contains an URL reference to the extension object, e.g. "https://rest.{{site.compass.reseller.domain}}/extension/1". If you need to know that extension's number or any of its other properties, simply perform a GET request on the resulting URL.

## Extensions property in users

In API v1 a user has a property `extensions` which is an array of extension numbers for that user, e.g. ["2355", "3466"].

In API v2 this property was removed. If you need to know a user's extensions, use the separate `/user/{id}/extensions` endpoint.

## Phones

In API v1 the phone endpoint (`/phone/{id}`) contains a mix of both static properties (such as the name and MAC address) and ephemeral properties (such as the phone's online status and IP address).

In API v2 the `/phone/{id}` endpoint only contains the static properties. If you need to retrieve any of the ephemeral properties derived from the phone's active SIP registration, use the separate `/phone/{id}/phoneStatus` endpoint.

## Queues

In API v1 a queue's `joinEmpty` and `leaveWhenEmpty` properties can contain the enum values "yes", "no", "strict" and "unknownQueueEmptyType".

In API v2 a new enum value was added: "loose".

## Input

In API v1, URL parameters in PATCH requests (such as `extension` in PATCH `/identity/{id}`) would accept both `NULL` and `""` (an empty string) to clear/unset the value. Also, both absolute (`"https://rest.{{site.compass.reseller.domain}}/extension/1"`) and relative (`"/extension/1"`) values would be accepted.

In API v2, only `NULL` is allowed to clear/unset the value and sending an empty string will be an invalid request. Also, only absolute URL values will be accepted.
