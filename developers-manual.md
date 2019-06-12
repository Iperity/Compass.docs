---
layout: doc-with-toc
title: Developers manual
permalink: /developers-manual.html
description: Developers manual for using the REST and XMPP APIs.
---

# Developers Manual

Last updated: {{ site.time | date: '%B %d, %Y' }}

## Introduction

This document consists of two parts:

- {{site.compass.reseller.prodname}} REST documentation
- {{site.compass.reseller.prodname}} XMPP documentation

The REST API is suited for configuration of the platform, reading values that change infrequently, and performing actions on the platform.

The XMPP API can be used to query the state of the platform, and to be notified of events happening on the platform in real time, and complements the REST API.


## {{site.compass.reseller.prodname}} REST Interface

### Introduction

This document describes the REST ([Representational State Transfer](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm){:target="_blank"})
interface of the telephony platform.

The following standards are used:

- RFC 2616: HTTP/1.1
- RFC 5789: PATCH method for HTTP
- RFC 1738: Uniform Resource Locators

#### API Reference

Besides this document itself, a complete API reference documenting all available endpoints and their input and output schemas can be found online at [https://rest.{{site.compass.reseller.domain}}/schema](https://rest.{{site.compass.reseller.domain}}/schema){:target="_blank"}. After providing your {{site.compass.reseller.prodname}} credentials, all API calls can be executed interactively.
An OpenAPI [specification](https://github.com/OAI/OpenAPI-Specification){:target="_blank"} is available for each version of the API:

- Version 2: https://rest.{{site.compass.reseller.domain}}/schema/schemas/v2.json
- Version 1: https://rest.{{site.compass.reseller.domain}}/schema/schemas/v1.json (deprecated)

#### Philosophy

Each resource in {{site.compass.reseller.prodname}} is represented by a URI of the format `https://host/type/id`. The first part of this URI indicates the hostname on which the REST interface is reachable. This is the domain of the telephony platform prefixed with `rest`, which amounts to `rest.{{site.compass.reseller.domain}}`. The first path component, the `type`, indicates a type of resource within the platform. The format of the `id` identifier is opaque and no assumptions on its format should be made by the client.
A resource can be created, retrieved, updated and deleted (CRUD). These operations are mapped onto the HTTP methods `POST`, `GET`, `PATCH` and `DELETE`, respectively. Operations that don’t fall under the CRUD paradigm, such as initiating a call, are mapped to `POST`.

### Using the API

A `GET` request consists of a URL and optionally of query parameters. `PATCH` and `POST` requests have bodies, which are encoded as JSON. The result of each request is a HTTP status code indicating success (2xx), a redirect (3xx) or an error (4xx and 5xx). Response data is encoded in JSON.

#### Security

All REST requests are available via HTTPS only. When REST requests are done via HTTP, they are immediately redirected to HTTPS using the HTTP `Location` header.

#### Authentication

Before every REST request, the user as whom the request is being made must be authenticated via HTTP [basic authentication](https://tools.ietf.org/html/rfc1945#section-11.1){:target="_blank"} over HTTPS. If no authentication is included, a `401 Not authorized` response is given. This response also includes a `WWW-Authenticate` header indicating that the client should authenticate itself.

#### Versioning

All objects retrieved from the REST interface are versioned. Versioning is denoted via the `Accept` (client to server) and `Content-type` (both ways) HTTP headers. The `Accept` header is expected to have the following format: `application/vnd.iperity.compass.<api-version-number>+json` A client **must** include a valid `Accept` header for every API request. This will ensure that your application keeps working properly when a new API version is released. Within an API version, there will never be any breaking changes. However, non- breaking changes like adding new endpoints or adding extra properties to the output will not necessarily result in a new API version. It’s valid to include multiple MIME types in your accept header, as specified in [https://tools.ietf.org/html/rfc7231#section-5.3.2](https://tools.ietf.org/html/rfc7231#section-5.3.2){:target="_blank"}. For example, requesting
`Accept: application/vnd.iperity.compass.v1+json; q=0.9, application/vnd.iperity.compass.v2+json; q=1.0`
would indicate to the server that either version is acceptable. This allows you to publish a client that’s compatible with two different API versions; for example, the current API version and the upcoming version that’s soon to be released.
To determine the exact version that was negotiated, look at the `Content-type` header that’s returned by the server.

#### Error handling

If an error occurs, the HTTP response code will either be in the 4xx range or in the 5xx range. The following error codes can be expected, as specified in RFC 2616:

- `400 Bad Request`: the client has sent a request that is invalid.
- `401 Unauthorized`: the credentials specified for authentication are not valid.
- `403 Forbidden`: the client tries to request data to which it has no access, it tries to write to resources on which only read-only access is granted, or it tries to use a feature which has not been enabled for the user. 
- `404 Not Found`: the resource specified by the URL does not exist.
- `405 Invalid Method`: the HTTP request method is invalid or unsupported.
- `500 Internal Server Error`: the platform encountered an error that was not expected.

In some cases, detailed error information will be specified in the body of the response.

### Resources

Each URL is associated with a resource. Each URL corresponds to an object within the {{site.compass.reseller.prodname}} platform. Conceptually, objects are subdivided in two base types:

- resources: queues, dial plan switches, voicemails,...
- entities: resellers, users, companies

**NOTE: the term resource has two different meanings: a base object type within {{site.compass.reseller.prodname}}, as well as a URL within the REST API.** While each {{site.compass.reseller.prodname}} resource is a REST resource, the opposite does not hold, as {{site.compass.reseller.prodname}} entities are also REST resources. Currently, these resources ({{site.compass.reseller.prodname}} objects) exist within the platform.

#### Resellers

A reseller is a container entity that owns a set of companies and/or users. Any user that has access to the reseller is able to access all underlying companies.

#### Companies

A company contains [users](#users), [queues](#queues), [dial plan switches](#dial-plan-switches), [extensions](#extensions), [phones](#phones), [voicemails](#voicemails) and [external numbers](#external-numbers).

#### External numbers

External numbers are phone numbers that can be used as outbound number (CLI) when dialing out from {{site.compass.reseller.prodname}} to an external party, as inbound number (DID) when an external party dials to {{site.compass.reseller.prodname}}, or as both at the same time. An external number is owned by a company and can be appointed as CLI for each identity individually.

#### Extensions

An extension is like an external number but only exists within the contained company: it can be used as either a caller id for internal calls, a dial code for an internal dial plan, or both.

#### Queues

A queue is owned by a company and has various properties, also configurable in the web interface of {{site.compass.reseller.prodname}}. Users can be made an agent of a queue either by using the login operations on a User resource, or by using the login operation of an Identity resource.

#### Phones

A phone represents a phone account within a company: a set of credentials that can be used by a SIP client to authenticate to {{site.compass.reseller.prodname}}.

|                    | reseller rights                   |
| **company rights** | **none**  | **read**  | **write** |
|--------------------|-----------|-----------|-----------|
| **none**           | none      | read      | write     |
| **read**           | read      | read      | write     |
| **write**          | write     | write     | write     |

_Table 1: Effective permissions on company_

#### Voicemails

A voicemail box can be used within any dial plan to allow the calling party to leave a message.

#### Dial plan Switches

Dial plan switches are dynamic switches that have a fixed set of named values. The switches for the authenticated user’s company can be queried and set.

#### Users

Users have a username and a password, which are also used for REST authentication. A user has one or more [identities](#identities).

#### Identities

An identity describes a particular “selfhood” of a user; users can have multiple identities. Using different identities, users can change their CLI, for example.

#### Rights

Rights describe access rights for a user. There are two types of rights: read and write rights. Write rights imply read rights. Rights have a “target” entity on which they grant privileges. Currently, rights can be granted on companies and resellers. For a given entity, a user has either no rights assigned to that entity or 1 right describing its access.
Rights are inherited from a parent entity to its child entities. For example, if user A has read rights on a reseller R, it implicitly may read all companies C1, C2,... under R, and also all users under these companies. These inherited rights are implicit: retrieving a list of rights for user A will only include the read right on R.
The term permission is used to describe the effective rights that a user has. For example, when a user is assigned **read** rights on a company and **write** rights on the reseller that owns the company, the user is said to have **write** permissions on the company. Table 1 shows the effective permissions on the company when a user has specific rights on that company and the containing reseller.

### Requests

#### GET requests

An HTTP GET request is used to read the data associated with objects. The data format is described in a JSON schema. The object is identified and referred to by the URI.

#### PATCH requests

An HTTP PATCH request is used to update an existing object. The body describes how the object should be updated. This description comprises a JSON document, specifying only the changed elements with their new values.
See also the [client compatibility](#client-compatibility) section.

#### POST requests

An HTTP POST request is used for various operations. Please refer to the [API Reference](#api-reference) for a description of the input/output data for each call.

#### Resource URL’s

URL’s of resources consist of a `type` and `identifier`. Each resource returned by the REST interface has a property named `self`, which is the absolute URL to that resource. This is useful when the client needs to retrieve detailed information by issuing a GET request on the object’s URL, or when the client needs to perform any other operation on that resource.
In various PATCH and POST requests you will encounter properties that are a reference to an existing [resource](#resources) within the {{site.compass.reseller.prodname}} platform such as a user or a queue; like `loginQueue`’s `queue` property, or `dialUser`’s `user`. These properties should always be submitted in the absolute URL format, e.g. `https://rest.{{site.compass.reseller.domain}}/queue/`.

#### Bootstrap URL’s

For a client to discover references to the authenticated user’s resource URL and to the authenticated user’s company resource URL, two special URL’s can be used:    

- `https://rest.{{site.compass.reseller.domain}}/user` will respond with `303 See other` and a `Location` header specifying a URL to the authenticated user’s resource.
- `https://rest.{{site.compass.reseller.domain}}/company` will respond with `303 See other` and a `Location` header specifying a URL to the authenticated user’s company resource.

### Client compatibility

#### HTTP PATCH

Because the HTTP PATCH method is fairly new, there can be issues with client compatibility. For that reason, the API allows the usage of the `X-HTTP-Header-Override` HTTP header. The value of this header overrides the actual HTTP request method that is used. For PATCH or DELETE methods, a normal HTTP POST can be used when providing this header. An example of this override header is provided in the [Using X-Method-Override](#using-x-method-override) section.

#### CORS

The REST API follows the cross-origin resource sharing (CORS) specification of the [W3C](https://www.w3.org/TR/cors/){:target="_blank"}. This means that the API can be used cross-origin on modern browsers. The server sends appropriate CORS headers when a cross-origin request is made.
The CORS specification is fairly recent. Client support differs from browser to browser. One particular issue is that some browsers do not follow HTTP redirects when performing cross-origin requests with credentials. By setting the custom HTTP header `X-No-Redirect: true`, the service will internally follow the redirect, returning the actual data object to the client. An example is provided in the [Using X-No-Redirect](#using-x-no-redirect) section.

### Examples

The examples below include only the necessary headers. Requests and responses may contain more headers than described below. Also, newlines are inserted in the JSON body to increase readability.
When characters are used that are not in the range A through Z or 0 through 9, the returned URL’s contain the URL encoding of those characters (using a percent-sign, followed by the two-digit hexadecimal value of that character). See [Uniform Resource Locators](https://www.ietf.org/rfc/rfc1738.txt){:target="_blank"} for details.

#### HTTPS redirect

If a request is performed over HTTP, a redirect is returned to a secure URL.

```
GET /user/100 HTTP/1.1
Host: rest.{{site.compass.reseller.domain}}
Accept: application/vnd.iperity.compass.v2+json

>> HTTP/1.1 308 Permanent Redirect
>> Location: https://rest.{{site.compass.reseller.domain}}/user/
```

#### Get authenticated user

```
GET /user HTTP/1.1
Accept: application/vnd.iperity.compass.v2+json
Host: rest.{{site.compass.reseller.domain}}

>> HTTP/1.1 303 See Other
>> Location: https://rest.{{site.compass.reseller.domain}}/user/
```

#### Retrieve user details

```
GET /user/100 HTTP/1.1
Accept: application/vnd.iperity.compass.v2+json
Host: rest.{{site.compass.reseller.domain}}

>> HTTP/1.1 200 OK
>> Content-Type: application/vnd.iperity.compass.v2+json

>> {
>>  "username" => "testuser",
>>  "extensions" => [ 201, 202 ],
>>  ...
>> }
```

#### Retrieve queues for a company

```
GET /company/100/queues HTTP/1.1
Accept: application/vnd.iperity.compass.v2+json
Host: rest.{{site.compass.reseller.domain}}

>> HTTP/1.1 200 OK
>> Content-Type: application/vnd.iperity.compass.v2+json

>> [
>>  {
>>      "theType" : "queue",
>>      "name" : "Support",
>>      "self" : "https://rest.{{site.compass.reseller.domain}}/queue/2003",
>>      ...
>>  },
>>  {
>>      "theType" : "queue",
>>      "name" : "Intern",
>>      "self" : "https://rest.{{site.compass.reseller.domain}}/queue/2007",
>>      ...
>>  }
>> ]
```

#### Set a dial plan switch

```
PATCH /dpSwitch/100 HTTP/1.1
Accept: application/vnd.iperity.compass.v2+json
Host: rest.{{site.compass.reseller.domain}}

{
    "currentSetting" : 2
}

>> HTTP/1.1 200 OK
>> Content-Type: application/vnd.iperity.compass.v2+json
>>
>> {
>>  "currentSetting" : 2,
>>  "self" : "https://rest.{{site.compass.reseller.domain}}/dpswitch/100",
>>  "name" : "My Switch",
>>  ...
>> }
```

#### Add a user to a queue

```
POST /user/100/loginQueue HTTP/1.1
Accept: application/vnd.iperity.compass.v2+json
Host: rest.{{site.compass.reseller.domain}}

{
    "queue" : "https://rest.{{site.compass.reseller.domain}}/queue/270",
    "priority" : "2",
    "callForward" : false
}

>> HTTP/1.1 200 OK
>> Content-Length: 0
```

#### Using X-Method-Override

```
POST /user/100 HTTP/1.1
X-HTTP-Method-Override: DELETE
Accept: application/vnd.iperity.compass.v2+json
Host: rest.{{site.compass.reseller.domain}}

>> HTTP/1.1 200 OK
>> Content-Length: 0
```

#### Using X-No-Redirect

```
GET /user HTTP/1.1
X-No-Redirect: true
Accept: application/vnd.iperity.compass.v2+json
Host: rest.{{site.compass.reseller.domain}}

>> HTTP/1.1 200 OK
>> Content-Type: application/vnd.iperity.compass.v2+json
>> {
>>  ...
>> }
```

### Changelog

As of this moment, the API serves two versions: v1 and v2.

#### V2

V2 is the current version. In this version, there are a few major changes compared to V1:

- The "full" endpoints like `/company/:id/fullUsers` are renamed to simply `/company/:id/users`, etc. If you want to retrieve only the basic information for a resource or entity, you can still do so using `/company/:id/resourcesFiltered` or `/company/:id/entitiesFiltered`.
- The `/did` endpoints, already deprecated in V1, are now completely removed. Please switch to `/externalNumber` instead.

Also, a large amount of new endpoints were made available. Creating and deleting several resource types via the API is now possible. Please refer to the API Reference for an overview.

#### V1

In V1, `did` is deprecated in favor of `externalNumber`. If your application uses any of the `/did/:id/` endpoints, `/company/:id/outboundClis`, `/company/:id/fullOutboundClis`, or the `outboundCli` property on an `identity`; please start migrating to their `externalNumber` variants to remain compatible with future versions.

### Demo code (PHP)

This following code fragment shows how to perform a GET request to the REST interface to retrieve additional information about the user with which you’ve authenticated.

```php
$username = 'my-username';
$password = 'my-password';

ch = curl_init('https://rest.{{site.compass.reseller.domain}}/user');

url_setopt($ch, CURLOPT_USERPWD, "$username:$password");
url_setopt($ch, CURLOPT_RETURNTRANSFER, true);
url_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
url_setopt($ch, CURLOPT_HTTPHEADER, array(
    'Accept: application/vnd.iperity.compass.v2+json'
));

$res = curl_exec($ch);

if ($res === false) {
    echo 'Request failed: '. curl_error($ch);
    exit(1);
}

$user = json_decode($res);

echo "You are {$user->name} ";
echo "with email address {$user->contact}.\n";
echo "Requests can be performed on URL {$user->self}.\n\n";
echo "Your user data is: \n";
print_r($user);
```

### Demo code (JavaScript)

This following code fragment shows how to perform a GET request to the REST interface to retrieve additional information about the user with which you’ve authenticated. This example uses the jQuery library (The btoa function is not available in Internet Explorer versions 9 or lower).

```javascript
var username = "my-username";
var password = "my-password";

$.ajax({
        type: "GET",
        url: "https://rest.{{site.compass.reseller.domain}}/user",
        headers: {
            "Authorization": "Basic " + btoa(username + ":" + password),
            "X-No-Redirect": true,
            "Accept": "application/vnd.iperity.compass.v2+json"
        },
    })
    .done(function(data) {
        alert("Hello there, " + data.name);
    });
```

## {{site.compass.reseller.prodname}} XMPP API

### Introduction

This document describes the XMPP (Extensible Messaging and Presence Protocol) interface of the {{site.compass.reseller.prodname}} telephony platform.
The API can be used to query the state of the platform, and to be notified of events happening on the platform, in real time. Notifications are sent for events such as new calls as they enter the platform and potentially traverse through the dialplan, for users logging on and off, and for calls and users entering and leaving queues, among others.
This API complements the REST API, which is suited for configuration of the platform, reading values that change infrequently, and performing actions on the platform.

#### XMPP

XMPP, the Extensible Messaging and Presence Protocol (formerly known as Jabber) is an XML-based protocol designed to distribute instant messaging, presence and application-specific XML messages. It is particularly suited for real-time monitoring and interaction with the telephony platform, especially compared to REST, which is traditionally a bad match for listening to real-time events.

#### XMPP on the {{site.compass.reseller.prodname}} platform

We use XMPP to support a standardized way of performing authentication, feature negotiation and providing other basic protocol features. We use the publish-subscribe extension of XMPP in order to provide real-time events of the {{site.compass.reseller.prodname}} platform.
Many XMPP software libraries are available to minimize the implementation time needed to use the {{site.compass.reseller.prodname}} XMPP API. The [XMPP Software Libraries](#xmpp-software-libraries) section mentions a couple of these libraries for various programming languages. Additionally, we distribute a JavaScript library that can be used to greatly simplify and speed up the development of local JavaScript web-applications using this XMPP API. This library is described in the [JavaScript XMPP library](#javascript-xmpp-library) section.

### Using the XMPP API

#### Connecting

The [XMPP-Core specification](https://xmpp.org/rfcs/rfc3920.html){:target="_blank"} provides details on how to establish a connection via XMPP. The hostname to connect to is `uc.{{site.compass.reseller.domain}}`. The standard XMPP port 5222 is used. Clients are required to use stream negotiation to encrypt the stream using [TLS](https://www.ietf.org/rfc/rfc2246.txt){:target="_blank"}.

For web clients, {{site.compass.reseller.prodname}} supports [WebSockets](https://tools.ietf.org/html/rfc6455){:target="_blank"} and the legacy [BOSH protocol](https://xmpp.org/extensions/xep-0124.html){:target="_blank"}.

For WebSockets, connect to `wss://bosh.{{site.compass.reseller.domain}}/ws`. For BOSH, use `https://bosh.{{site.compass.reseller.domain}}/http-bind`.

XMPP is a relatively complex protocol with many implementation details, and for most applications it is not advisable to implement an XMPP client from scratch. Instead, there are many good software libraries available for many platforms and programming languages, some of which have been highlighted in the [XMPP Software Libraries](#xmpp-software-libraries) section.

##### Authentication

Use the username and password of your {{site.compass.reseller.prodname}} account. A username in XMPP is formatted as a JID, which is a node (your {{site.compass.reseller.prodname}} username), followed by the unified communications subdomain of {{site.compass.reseller.prodname}} (`uc.{{site.compass.reseller.domain}}`). For example, if your username is “johndoe”, use `johndoe@uc.{{site.compass.reseller.domain}}` to log in.
If your company has been configured with a company domain, use that to log in. For example, use `johndoe@yourcompany.com` for a company configured with the domain `yourcompany.com`. Note that the XMPP connection hostname is always `uc.{{site.compass.reseller.domain}}`, for all companies.
The password is identical to the password that is used for logging in to the web interface and/or the presence platform.
Each connection (session) in XMPP has an associated “resource”, in addition to the provided username. Multiple connections for the same username can be active by providing a unique resource for each connection. For example, a chat client may be active using resource “pidgin” while a client component uses the API as resource “api”. When multiple connections try to use the same resource, the latter connection will **replace** the former. The first connection will receive a stream error XML packet. If your service requires multiple simultaneous connections, use a resource name that is guaranteed to be unique for each connection. For example, use the machine’s hostname combined with a small random string.
There is a connection limit per JID (so per user) of 10 simultaneous connections. The limit is global and applies to any type of connection: WebSocket, BOSH and plain XMPP.
The XMPP API operates on the same XMPP server as presence and chat functionality. Therefore, care must be taken that these two do not conflict. Besides using a unique resource for the API connection, set your API client to [**invisible mode**](https://xmpp.org/extensions/xep-0126.html){:target="_blank"}. Also, use a negative priority in the client’s presence packet to disallow chat message delivery. **NOTE: the API client must send at least one presence available packet in order to receive pubsub-messages.**

Please note: after 20 incorrect login attempts, the source IP address will be blocked from logging in to XMPP for 10 minutes. Especially when logging in programmatically, avoid retrying on authentication errors too often.

#### Getting data

After connecting and authenticating, most applications will want to retrieve the current state of the platform in order to initialise and seed local data models and representations.

- First, the client should send a `getcompany` request. The system will respond with the company name and company ID for your user. See the [Example of a GetCompany Request](#example-of-a-getcompany-request) section for the exact format of the request and response.
- Then, a current list of users, queues, or calls can be queried from the platform. See the [Example of a Get Request](#example-of-a-getcompany-request) section for more details on the get requests.

Usually, after retrieving the current state of the platform, a client will want to subscribe itself to retrieve notifications, in order to stay up-to-date when the state of the platform changes. This is described in the next paragraph.

#### Receiving notifications

Notifications can be received using the XMPP publish-subscribe protocol, 'pubsub' in short. The {{site.compass.reseller.prodname}} XMPP API follows the publish-subscribe protocol as described in [XEP-0060](https://xmpp.org/extensions/xep-0060.html){:target="_blank"}. The node on which you have to subscribe is a numeric {{site.compass.reseller.prodname}} company identifier. In order to retrieve the company-id for the user with which you are logged in, send the `getcompany` request as explained in the [Example of a GetCompany Request](#example-of-a-getcompany-request) section.

This one subscription to your company’s node is sufficient to receive all events related to your {{site.compass.reseller.prodname}} company. See [Example of a Pubsub subscription Request](#example-of-a-pubsub-subscription-request) for an example request.

When subscribing, make sure to use the value `presence` for the configuration field `pubsub#expire`. This allows the server to remove your subscription when the API client disconnects. This way, you’ll not have to worry about having multiple subscriptions or removing old subscriptions.

### Data Model

This section contains qualitative descriptions with some examples of the most important data types used by the API. For a rigorous breakdown of all data models and their relationships, see the [Appendix B: Schemas](#appendix-b-schemas) for the XML schemas.

#### User

The User data-item represents a user of the platform. A list of users on the platform in this format can be retrieved by sending a get request, as described in the [Example of a Get Request](#example-of-a-get-request) section. The User element comprises the following items:

- Each user has an `id` attribute which identifies the user uniquely. This is the same as the `userId` that’s being referred to in the `queueEntries` element, or the `userEntries` element in the queue data model.
- The `loggedIn` and `name` elements are always present, describing whether the user is logged on to a phone, and the full name of the user.
- Elements describing additional information such as `extensions` (the phone extension), `contact` (an e-mail address that can be used to contact the user) and an `address` element might or might not be present.
- A list of identifiers for the user is visible in the `identifiers` element. The `lisaId`, `xmppJid` and `compassId` are present, denoting the different identifiers used for this user across several parts of the platform. The `lisaId` is the identifier that being used all throughout the API as `userId`.
- A list of queues that the user is currently logged on to, is visible in the `queueEntries` element. Each containing `entry` element describes the queue-id of the queue that the user is logged in to, repeats the user-id of the user, and shows the priority for that user in that queue. For calls entering the queue, calls will be offered to an available user with the highest priority, before users with a lower priority are tried. If a user is logged in and has queue-entries, the user can receive calls from these queues.

Below is an example for a single user.

```xml
<user id="883" xmlns="http://iperity.com/compass">
    <identifiers>
        <lisaId>883</lisaId>
        <xmppJid>alex@domain.nl</xmppJid>
        <compassId>alex</compassId>
    </identifiers>
    <queueEntries>
        <entry>
            <queueId>2</queueId>
            <userId>883</userId>
            <priority>10</priority>
        </entry>
    </queueEntries>

    <name>Alex Groot</name>
    <loggedIn>false</loggedIn>
    <address>Straat 123</address>
    <contact>alex@groot.nl</contact>
    <extensions>333</extensions>
</user>
```

#### Queue

The Queuedata item represents a waiting-queue for calls on the platform. A list of queues on the platform in this format can be retrieved by sending a get request, as described in the [Example of a Get Request](#example-of-a-get-request) section.
A queue has users, which are the agents that receive calls that enter the queue, and calls, which are the calls that are currently waiting in the queue. When a call is answered by a user or is terminated, it will be removed from the queue. In detail, the `Queue` element comprises the following items:

- Each queue has an `id` attribute, which identifies the queue uniquely. This is the same as the `queueId` that’s being referred to in the `userEntries` element, and the `queueEntries` element in the user data model. A `name` is specified as well.
- The `userEntries` element describes the users that are logged in to the queue. This is analogous to the `queueEntries` described in the [User](#user) section.
- The `callIds` element describes the ids for calls that are currently waiting in the queue.

Below is an example for a single queue.

```xml
<queue id="641">
    <name>Queue 1</name>
    <userEntries>
        <entry>
            <queueId>641</queueId>
            <userId>16</userId>
            <priority>10</priority>
        </entry>
    </userEntries>
    <callIds>
        <id>528638e0d5ee-m9u68m629b7.bf9fd24cbe10dd80439bc413.0</id>
    </callIds>
    <xmppJid>queue-641</xmppJid>
</queue>
```

#### Call

A list of calls on the platform in this format can be retrieved by sending a get request, as described in the [Example of a Get Request](#example-of-a-get-request) section.

When setting the `history` attribute to `true` when retrieving calls, a number of past (ended) calls are returned as well for historic purposes. Calls that have their state set to `DISCONNECTED` are no longer active.

- Calls are represented with two endpoints (callpoints): a source and a destination. See the following chapter for more information on callpoints, and on how the calls change in reaction to different events in practice.
- Calls have a state element, that describes the state that the call is currently in. The state can be `CONNECTING`, `RINGING`, `ANSWERED`, or `DISCONNECTED`.
- The `properties` element holds additional information about a call.

Below is an example of a call.

```xml
<call id="30680e334948d50e0ae355db5bae6a05316e5438/1">
    <source id="192" type="User" xsi:type="callpointUser">
        <timeCreated>1384535809</timeCreated>
        <properties/>
        <userId>17</userId>
        <userName>Jan Jansen</userName>
        <identityId>73</identityId>
        <state>CONNECTING</state>
    </source>
    <destination id="194" type="User" xsi:type="callpointUser">
        <timeCreated>1384535809</timeCreated>
        <properties/>
        <userId>16</userId>
        <userName>Alex Groot</userName>
        <identityId>74</identityId>
        <state>CONNECTING</state>
    </destination>
    <state>CONNECTING</state>
    <properties>
        <QueueCallForCall>
            30680e334948d50e0ae355db5bae6a05316e5438
        </QueueCallForCall>
    </properties>
</call>
```

#### Callpoint

Each call has two `callpoints` stored in two elements: a `source`, and a `destination`. Each callpoint has an `id` attribute, a `type` attribute and a `state` attribute. The different callpoint types are explained below. Moreover, all callpoints have a `timeCreated` element, and a `properties` element where additional properties of the callpoint can be displayed. Optionally, a `timeStarted` or `timeEnded` element is present.

Callpoints have a `state` element, that describes the state that the callpoint is currently in. The state can be `CONNECTING`, `RINGING`, `ANSWERED`, or `INACTIVE`. An `INACTIVE` state signifies that that endpoint has requested for the call to be put on hold. For example, when a user presses the *hold* button on her phone, the corresponding user-callpoint will flip to an `INACTIVE` state.

Each callpoint is one of the following types:

- `callpointUser` - A user on the telephony platform. Has a `userId`, a `userName` and an `identityId`.
- `callpointQueue` - A queue on the telephony platform.
- `callpointDialplan` - A dialplan on the telephone platform.
- `callpointExternal` - A phone number that is external to the {{site.compass.reseller.prodname}} platform.

Since each external or internal number has a dialplan attached, each new call starts out with a `callpointDialplan` destination callpoint. Both the destination and source callpoints of a call can change as the call gets routed through the platform, or as calls are forwarded.

#### Interpreting Call Data

The following is true for all calls:

- A `call` is a connection between two `endpoints`.
- The caller (who or what initiated the call) is the source endpoint, the receiver (who or what is being called) is the destination endpoint.
- All calls start with the destination endpoint being of the Dialplan type.
- After that, the destination can change to a user, a queue, a voicemail box or any other entry in your dialplan, depending on the dialplan.
- Both the `source` and `destination` callpoints can change during a call.

##### A ‘simple’ call

A simple call here is defined as a direct call between two endpoints, for example from one user to another user. For the sake of simplicity, the calls here aren’t transferred or forwarded.
Consider the following example of a call between an external caller and a user of the platform, being reachable via an external number of the platform. The following list describes what happens in terms of data objects and updates in the XMPP data model:

1. A call from an external source enters the platform, directed at one of the ‘external numbers’ of your company on the platform.
2. A new call is created, with the source callpoint being of the `callpointExternal` type, and the `destination` callpoint being of the `callpointDialplan` type. The `state` of the new call is `CONNECTING`. A `callStartNotification` is being sent.
3. After following the dialplan, the `destination` callpoint is changed to the correct `callpointUser` callpoint. A `callUpdateNotification` is being sent with `updateType` set to `DESTINATION`.
4. Assuming the user is logged on and not busy; The `state` of the call changes to `RINGING`. A `callUpdateNotification` is being sent with `updateType` set to `STATE`.
5. When the user answers, the `state` of the call changes to `ANSWERED`. The correct `callUpdateNotification` is being sent.
6. When one of both parties hangs up, a `callEndNotification` is sent. The `endReason` element is set to either `SOURCE_HANGUP` or `DESTINATION_HANGUP`.

##### Unattended transfer

In an unattended transfer, one of the endpoints of the call transfers the call to another endpoint.

Let's assume a call (X) takes place from endpoint A to B, in which A and B are persons. Now B wants to transfer the call to C, so that A and C can talk. The call is transferred when C picks up. Until this point, or when either C declines or B cancels the transfer, the original call between the A and B endpoints remains active.

When the transfer is initiated, the platform initiates a new call (Y) with `RINGING` status between A and C. When C picks up, call Y switches to `ANSWERED` status. Call X is terminated, with the `endReason` set to `REPLACE`.

##### Attended transfer

In an attended transfer, similar to an unattended transfer, one of the endpoints of a call is changed. However, the initiator of the transfer (B) is connected with the transfer destination (C) before A and C are connected.

Let us assume a call (X) from endpoint A to B, in which A and B are persons. Now B wants to transfer the call to C, so that A and C can talk. However, before connecting A to C, the persons B and C have a conversation. When B hangs up, A is connected to C. If C never answers or C hangs up on B, B is returned to its previous conversation with A.

When the attended transfer is initiated, the platform initiates a new call (Y) between B and C, after which B and C can communicate. After B hangs up, call Y is terminated and the destination of the original call X is changed to C, after which A and C can communicate.

##### Queue calls

Multiple queue scenarios exist. Basically, the caller enters a queue (end up in a call with a destination endpoint of type queue) with the intention of becoming connected to an agent. If multiple agents occupy the queue, depending on platform configuration, the PBX chooses which agents to ring in which order. Multiple agents can be rung at once. In any case, there is only one agent that picks up the call.

The procedure to connect the user to one of the agents, is mostly analogous with an unattended transfer, where the queue takes the place of endpoint B. While the user (A) is in a call (X) with the queue (B), calls (Y1,Y2,... ) are initiated to several agents (C1,C2,... ). Depending on the configuration of the queue, agents can be called one-by-one, or simultaneously. As soon as an agents answers, the `destination` of call X is changed to be the user (A), and all ringing calls (Y1,Y2,... ) from the queue are terminated with the reason `SOURCE_HANGUP`.

### Requests and Responses

RPC-style requests can be performed over the XMPP connection as well. Both requests and responses are wrapped IQ stanzas, as defined in XMPP-Core. Requests have to be sent to the `phone` subdomain of the main XMPP host: `phone.uc.{{site.compass.reseller.domain}}`.
This section contains examples of the most-frequently used requests and their responses. For a rigorous description of all valid requests and responses, see the XML schemas in the [XSD section]{(#appendix-b-schemas). Schemas for pubsub subscription requests can be found in the relevant XMPP standards, and are not repeated here.

#### Example of a GetCompany Request

As described before in the [Getting data](#getting-data) section, the `getcompany` request is sent to retrieve the company of the user for which the XMPP session has been set up. The `getcompany` request does not have any parameters.

##### Request

The complete IQ request is shown here, in which a unique value should be used for
the `id` attribute.

```xml
<iq id="9055:lisa" to="phone.uc.{{site.compass.reseller.domain}}" type="get" xmlns="jabber:client">
    <request type="GET_COMPANY" xmlns="http://iperity.com/compass"/>
</iq>
```

##### Response

The API responds with an id and unique name for the company.

```xml
<iq from="phone.{{site.compass.reseller.domain}}" id="9055:lisa" to="user@uc.{{site.compass.reseller.domain}}/resource" type="result" xmlns="jabber:client">
    <result xmlns="http://iperity.com/compass" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="getCompanyResult">
        <id>15</id>
        <name>companyname</name>
    </result>
</iq>
```

#### Example of a Get Request

After the company-name is known, additional get requests can be sent. These requests can be used to retrieve the list of users, queues or calls. Which list of objects to retrieve can be configured by setting the `type` attribute of the `filter` element to either `user`, `queue` or `call`. Optionally, the `history` attribute can be specified. If the history attribute is set to `true` and the type is `call`, the platform will return some past calls as well as all currently ongoing calls.

##### Request

In this example, the list of users is retrieved.

```xml
<iq id="2833:lisa" to="phone.uc.{{site.compass.reseller.domain}}" type="get" xmlns="jabber:client">
    <request type="GET" xmlns="http://iperity.com/compass">
        <filter type="user"/>
    </request>
</iq>
```

##### Response

The server responds with a list of the elements of the requested type; in this case the list of users is returned.

```xml
<iq from="phone.uc.{{site.compass.reseller.domain}}" id="2833:lisa" to="user@uc.{{site.compass.reseller.domain}}/resource" type="result" xmlns="jabber:client">
    <result xmlns="http://iperity.com/compass" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="getResult">
        <user id="105">
            <identifiers>
                <lisaId>105</lisaId>
                <xmppJid>testuser@domain.nl</xmppJid>
                <compassId>testuser</compassId>
            </identifiers>
            <queueEntries/>
            <name>Test User</name>
            <loggedIn>false</loggedIn>
            <location>-1</location>
            <shortname/>
            <language>nl</language>
            <contact/>
            <extensions/>
        </user>
        <user id="16">
            <identifiers>
                <lisaId>16</lisaId>
                <xmppJid>othertestuser@domain.nl</xmppJid>
                <compassId>Other Test User</compassId>
            </identifiers>
            <queueEntries>
                <entry>
                    <queueId>641</queueId>
                    <userId>16</userId>
                    <priority>10</priority>
                </entry>
                <entry>
                    <queueId>663</queueId>
                    <userId>16</userId>
                    <priority>10</priority>
                </entry>
                <entry>
                    <queueId>665</queueId>
                    <userId>16</userId>
                    <priority>10</priority>
                </entry>
            </queueEntries>
            <name>Test User</name>
            <loggedIn>false</loggedIn>
            <location>776</location>
            <shortname/>
            <language>en</language>
            <contact>testuser@testdomain.com</contact>
            <extensions>222</extensions>
        </user>
    </result>
</iq>
```

For a detailed description of the data model, see the [Data Model](#data-model) section.

#### Example of a Pubsub subscription Request

As described before in the [Receiving notifications](#receiving-notifications) section, the client should subscribe to the company node to retrieve notifications when the state of the platform changes. The procedure is as described here. Please note that only the node to subscribe to, as configured by the `node` attribute in the subscribe-element, is specific to this API. The subscription procedure itself is part of the XMPP specification.

##### Request

The complete pubsub subscription request is shown here. As always, the id should be replaced by a unique value, that can be used to identify the response from the server. The JID-value should be replaced by the client JID, as explained in the [Authentication](#authentication) section. The node-value should be replaced by the company-id, as retrieved with the `getCompany` call as shown in the [Example of a GetCompany Request](#example-of-a-getcompany-request). Your subscription is automatically removed when the connection is closed, or when the presence state ‘unavailable’ is sent.

```xml
<iq id="8597:pubsub" to="pubsub.uc.{{site.compass.reseller.domain}}" type="set" xmlns="jabber:client">
    <pubsub xmlns="http://jabber.org/protocol/pubsub">
        <subscribe jid="user@uc.{{site.compass.reseller.domain}}/resource"node="15"/>
    </pubsub>
</iq>
```

##### Response

The API responds with a confirmation that the subscription was received and will be processed.

```xml
<iq from="pubsub.uc.{{site.compass.reseller.domain}}" id="8597:pubsub" to="user@uc.{{site.compass.reseller.domain}}/resource" type="result" xmlns="jabber:client">
    <pubsub xmlns="http://jabber.org/protocol/pubsub">
        <subscription jid="user@uc.{{site.compass.reseller.domain}}/resource" node="15" subscription="pending"/>
    </pubsub>
</iq>
```

Soon to be followed by a message that the client is subscribed to the requested node.

```xml
<message from="pubsub.uc.{{site.compass.reseller.domain}}" to="user@uc.{{site.compass.reseller.domain}}/resource" xmlns="jabber:client">
    <event xmlns="http://jabber.org/protocol/pubsub#event">
        <subscription jid="user@uc.{{site.compass.reseller.domain}}/resource" node="15" subscription="subscribed"/>
    </event>
</message>
```

### Notifications

Receiving real-time notifications through the XMPP `pubsub` mechanism, is the main strength of the XMPP API compared to other APIs. Receiving these notifications is simple. As explained before in the [Receiving notifications](#receiving-notifications) section, a single subscription to the node of your company-id suffices to receive all notifications.

This section contains some basic information about notifications. For a rigorous break-down of all notifications that can be received and their formats, refer to the [XSD schema](#appendix-b-schemas).

All notifications are wrapped in a `notification` element with the following attributes:

- `type` - The type of the notification. The relevant type has been described in the tables below.
- `timestamp` - A timestamp indicating when the notification was fired, in `Unix time` format (Unix time format is a representation of time as a long number, as the number of seconds that have elapsed since 00:00:00 UTC on January 1st, 1970. See [https://en.wikipedia.org/wiki/Unix_time](https://en.wikipedia.org/wiki/Unix_time){:target="_blank"} for more information.).

#### propertyChange

Most update notifications use a `propertyChange` element to describe the changed elements in the data model. The `propertyChange` element has the following elements:

- `name` - The name of the element that has changed.
- `oldValue` - The previous value of the changed element.
- `newValue` - The new value of the changed element.

### JavaScript XMPP library

The Compass JavaScript (Compass.js) library is a JavaScript library that can be used to connect to, retrieve data from, and retrieve notifications from the XMPP API of the {{site.compass.reseller.prodname}} telephony platform. The library is designed to run as local JavaScript in a client's web browser.

For more information, see the project repository on [GitHub](https://github.com/Iperity/Compass.js){:target="_blank"}.

### XMPP Software Libraries

XMPP is a widely spread and open standard, resulting in many open-source libraries in different programming languages. While any XMPP library should suffice, these are the libraries we’ve used to write clients for the {{site.compass.reseller.prodname}} platform.

#### Java

[Smack](https://www.igniterealtime.org/projects/smack/){:target="_blank"} is a Java component that provides client-side XMPP connections.

#### PHP

Using our patched version of [XMPPHP](https://github.com/Iperity/xmpphp){:target="_blank"}, you can receive events and perform actions via XMPP. However, we feel that PHP is not the ideal language to handle longlived client-to-server connections. The BOSH variant of XMPPHP might be better suited for most applications.

#### JavaScript

[Strophe](http://strophe.im/){:target="_blank"} provides a good basis for BOSH connections from JavaScript, suitable to be included in web applications. The [Strophe pubsub module](https://github.com/ggozad/strophe.plugins){:target="_blank"} is useful to receive real-time events.

### Appendix A: Example of an XMPP session

This appendix contains an annotated example of an XMPP session. The authentication has been removed, as this should really be done using a software library. Moreover, only responses that need to be parsed are included.

#### Set Invisble

Send:

```xml
<iq xmlns="jabber:client" type="set" id="9efe674b-cd1a-450b-b5f4-dc4eb9b2fb96:lisa">
    <query xmlns="jabber:iq:privacy">
        <list name="invisible">
            <item action="deny" order="1">
                <presence-out />
            </item>
        </list>
    </query>
</iq>

<iq xmlns="jabber:client" type="set" id="bf55f192-2c36-4719-b319-5d54198c4124:lisa">
    <query xmlns="jabber:iq:privacy">
        <active name="invisible" />
    </query>
</iq>
```

#### Send Presence

Send:

```xml
<presence xmlns="jabber:client">
    <priority>1</priority>
</presence>
```

#### Get Company

Send:

```xml
<iq xmlns="jabber:client" to="phone.uc.{{site.compass.reseller.domain}}" type="get" id="24f9da6b-5a48-4199-b308-5eeb0a0fbf7e:lisa">
    <request xmlns="http://iperity.com/compass" type="GET_COMPANY" />
</iq>
```

Receive:

```xml
<iq xmlns="jabber:client" from="phone.uc.{{site.compass.reseller.domain}}" to="testuser@uc.{{site.compass.reseller.domain}}/LisaApi_BplUI0bnYj" type="result" id="24f9da6b-5a48-4199-b308-5eeb0a0fbf7e:lisa">
    <result xmlns="http://iperity.com/compass" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="getCompanyResult">
        <id>8323166</id>
        <name>My Company Name</name>
    </result>
</iq>
```

#### Get User

Send:

```xml
<iq xmlns="jabber:client" to="phone.uc.{{site.compass.reseller.domain}}" type="get" id="16b35ce3-4276-466a-9072-112fe43c249c:lisa">
    <request xmlns="http://iperity.com/compass" type="GET">
        <filter type="user" />
    </request>
</iq>
```

Receive:

```xml
<iq xmlns="jabber:client" from="phone.uc.{{site.compass.reseller.domain}}" to="testuser@uc.{{site.compass.reseller.domain}}/LisaApi_BplUI0bnYj" type="result" id="16b35ce3-4276-466a-9072-112fe43c249c:lisa">
    <result xmlns="http://iperity.com/compass" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="getResult">
        <user id="8323556">
            <identifiers>
                <lisaId>8323556</lisaId>
                <xmppJid>testuser@uc.{{site.compass.reseller.domain}}</xmppJid>
                <compassId>test</compassId>
            </identifiers>
            <queueEntries>
                <entry>
                    <queueId>1442896</queueId>
                    <userId>8323556</userId>
                    <priority>10</priority>
                </entry>
                <entry>
                    <queueId>1761056</queueId>
                    <userId>8323556</userId>
                    <priority>10</priority>
                </entry>
            </queueEntries>
            <name>Test User</name>
            <loggedIn>true</loggedIn>
            <location>1442646</location>
            <language>nl</language>
            <contact>test@iperity.com</contact>
            <extensions>200</extensions>
        </user>
    </result>
</iq>
```

#### Get Queue

Send:

```xml
<iq xmlns="jabber:client" to="phone.uc.{{site.compass.reseller.domain}}" type="get" id="78fb8fa8-3dbd-47eb-8b11-9c81521a9464:lisa">
    <request xmlns="http://iperity.com/compass" type="GET">
        <filter type="queue" />
    </request>
</iq>
```

Receive:

```xml
<iq xmlns="jabber:client" from="phone.uc.{{site.compass.reseller.domain}}" to="testuser@uc.{{site.compass.reseller.domain}}/LisaApi_BplUI0bnYj" type="result" id="78fb8fa8-3dbd-47eb-8b11-9c81521a9464:lisa">
    <result xmlns="http://iperity.com/compass" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="getResult">
        <queue id="1442896">
            <name>My Queue</name>
            <userEntries>
                <entry>
                    <queueId>1442896</queueId>
                    <userId>8323556</userId>
                    <priority>10</priority>
                </entry>
            </userEntries>
            <callIds />
        </queue>
    </result>
</iq>
```

#### Get Calls

Send:

```xml
<iq xmlns="jabber:client" to="phone.uc.{{site.compass.reseller.domain}}" type="get" id="a5f7f827-e585-42cc-bd29-d05f66c2e406:lisa">
    <request xmlns="http://iperity.com/compass" type="GET">
        <filter type="call" />
    </request>
</iq>
```

Receive:

```xml
<iq xmlns="jabber:client" from="phone.uc.{{site.compass.reseller.domain}}" to="testuser@uc.{{site.compass.reseller.domain}}/LisaApi_BplUI0bnYj" type="result" id="a5f7f827-e585-42cc-bd29-d05f66c2e406:lisa">
    <result xmlns="http://iperity.com/compass" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="getResult" />
</iq>
```

#### Pubsub Subscribe

Send:

```xml
-- Sent:
<iq xmlns="jabber:client" to="pubsub.uc.{{site.compass.reseller.domain}}" type="set" id="2dab0bb2-0f88-4852-acfc-031499844f35:pubsub">
    <pubsub xmlns="http://jabber.org/protocol/pubsub">
        <subscribe node="8323266" jid="testuser@uc.{{site.compass.reseller.domain}}/LisaApi_BplUI0bnYj" />
    </pubsub>
</iq>
```

Receive:

```xml
<iq xmlns="jabber:client" from="pubsub.uc.{{site.compass.reseller.domain}}" to="testuser@uc.{{site.compass.reseller.domain}}/LisaApi_BplUI0bnYj" id="2dab0bb2-0
f88-4852-acfc-031499844f35:pubsub" type="result">
    <pubsub xmlns="http://jabber.org/protocol/pubsub">
        <subscription jid="testuser@uc.{{site.compass.reseller.domain}}/LisaApi_BplUI0bnYj" subscription="pending" node="8323266" />
    </pubsub>
</iq>
<message xmlns="jabber:client" from="pubsub.uc.{{site.compass.reseller.domain}}" to="testuser@uc.{{site.compass.reseller.domain}}/LisaApi_BplUI0bnYj">
    <event xmlns="http://jabber.org/protocol/pubsub#event">
        <subscription jid="testuser@uc.{{site.compass.reseller.domain}}/LisaApi_BplUI0bnYj" node="8323266" subscription="subscribed" />
    </event>
</message>
```

#### Example Pubsub Message; First messages for Incoming Call

Receive:

```xml
<message xmlns="jabber:client" from="pubsub.uc.{{site.compass.reseller.domain}}" to="testuser@uc.{{site.compass.reseller.domain}}/LisaApi_BplUI0bnYj" type="headline">
    <event xmlns="http://jabber.org/protocol/pubsub#event">
        <items node="8323266">
            <item id="5D4AC4DA3888">
                <notification xmlns="http://iperity.com/compass" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" timestamp="1492705754" type="notification.call.start" xsi:type="callStartNotification">
                    <call id="IKhDw.MuTOkUTDa8nEV-6dd.c133ada042758fa0b92a6ed8621bd224.0">
                        <source id="8093725" type="External" xsi:type="callpointExternal">
                            <timeCreated>1492705754</timeCreated>
                            <properties />
                            <number>31614241111</number>
                            <name>31614241111</name>
                            <state>CONNECTING</state>
                        </source>
                        <destination id="8093726" type="Dialplan" xsi:type="callpointDialplan">
                            <timeCreated>1492705754</timeCreated>
                            <properties />
                            <exten>31413231111</exten>
                            <state>CONNECTING</state>
                        </destination>
                        <state>CONNECTING</state>
                        <properties />
                    </call>
                </notification>
            </item>
        </items>
    </event>
    <headers xmlns="http://jabber.org/protocol/shim">
        <header name="Collection">8323266</header>
    </headers>
</message>
```

### Appendix B: Schemas

The XML schemas are available for download in `xsd` format.
Every XML response from the server notifies the user of the schema that it can be verified against, by specifying the correct type-name in the `xsi:type` attribute of the element. Requests to the server may include an `xsi:type` attribute to indicate the correct schema to the server, but this is not required. However, if the `xsi:type` attribute is included, it must be correct.

 - [XSD data types](https://www.{{site.compass.reseller.domain}}/docs/xsd/data.xsd)
 - [XSD for notifications](https://www.{{site.compass.reseller.domain}}/docs/xsd/notifications.xsd)
