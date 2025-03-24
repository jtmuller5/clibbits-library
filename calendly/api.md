# API Conventions

The Calendly API allows our users to create their own custom features by providing them programmatic access to their Calendly data and functionality. In order to give our users a best-in-class experience when using our API, we have developed this API with standard and consistent conventions in mind. We've created well-defined conventions to make integrations against our API an intuitive and simple experience for our users.

## Resource Reference by Uniform Resource Identifier (URI)

Instead of referencing unique resources by an ID, which can be common in some RESTful APIs, we've decided that a more uniform approach to identifying a resource is to use a Uniform Resource Identifier (URI).

### Requests

For requests, this means that when a parameter is a resource reference, it has to be a URI. For example, when calling GET /organization_memberships, we pass the URI of a user to filter my results by that user:

```http
// GET /organization_memberships?user=https://api.calendly.com/users/ABC123XYZ789

{
  collection: [...],
  pagination: {...}
}
```

### Responses

When calling one of our API endpoints, you may notice that within the response data, any attribute referring to another resource has a URI value. For example, when calling GET /event_types, you may notice that owner has a URI value:

```http
// GET /event_types

{
  "collection": [
    {
      "profile": {
        "name": "John Doe",
        "owner": "https://api.calendly.com/users/ABC123XYZ789",
        "type": "User"
      },
      ...
    }
  ],
  ...
}
```

Since owner is a separate resource, we reference it through a URI. If the user wishes to access more data for the owner, a request to the provided URI can be made:

```http
// GET /users/ABC123XYZ789

{
  "resource": {
    "avatar_url": null,
    "created_at": "2020-04-20T20:44:29.052644Z",
    "email": "john.doe@fakeaddress.com",
    "name": "John Doe",
    "scheduling_url": "https://www.calendly.com/john-doe",
    "slug": "john-doe",
    "timezone": "America/New_York",
    "updated_at": "2020-09-08T19:11:15.831274Z",
    "uri": "https://api.calendly.com/users/ABC123XYZ789"
  }
}
```

## Keyset-based Pagination

Also referred to as "cursor-based pagination", this approach is how we handle pagination for all of our endpoints that can return a collection of multiple resources. Unlike offset-limit pagination, this approach allows us to provide accurate data to our users, despite resources possibly being added/removed to the collection on subsequent page retrievals.

When calling an endpoint that returns a collection of multiple resources, you will notice a pagination object in your response body. Inside of the pagination object is a next_page attribute. If there are more resources than what has been returned, the next_page attribute will contain the URL to the next page of resources; otherwise, next_page will be null:

```http
// GET /scheduled_events (has another page of resources)
{
  collection: [...],
  pagination: {
    "count": 20,
    "next_page": "https://api.calendly.com/scheduled_events?count=5&page_token=nODUpzeh8eNPGLrNaBoMK1HijZiO6H1t&user=https%3A%2F%2Fapi.calendly.com%2Fusers%2FAFEHBEDFE6WUE2R7"
  }
}

// GET /scheduled_events (has no more pages of resources)
{
  collection: [...],
  pagination: {
    "count": 10,
    "next_page": null
  }
}
```

This makes it convenient for our users to retrieve the next page of data for a large collection of resources.

## Deterministic Irrespective of Requester

Our API will always return data based on the request being made and never based on the requester making the data. For example, if the user requesting the data has an "admin" role, the data will be exactly the same as if the requesting user had a "user" role. This approach helps ensure that our users never become confused around the data being requested and the requester of the data. The response will be based solely on the request.

As a result of this approach, our API users will always need to be very explicit about what data they are requesting. Our API users will never get responses based on the requester of the data.

For example, despite whether my role is "admin" or "user", when making a request to /event_types, the response will also be based on the actual request made:

```http
Current User Role is `admin`
Current User Role is `user`
// GET /event_types?user=<user_a>
{
  collection: [
    { "uri": "https://api.calendly.com/event_types/A", ... },
    { "uri": "https://api.calendly.com/event_types/B", ... },
    { "uri": "https://api.calendly.com/event_types/C", ... }
  ],
  ...
}
```

### Permission

This does not mean, however, that all users will have the same level of access to the data. For example, if the requester has the role of "admin", they may be able to request the organization's webhooks successfully:

```http
// Current User Role: "admin"
// GET /webhook_subscriptions?scope=organization&organization=<org_a>

Status: 200 OK
```

...while a requester with the role of "user" may not have access when making the same request:

```http
// Current User Role: "user"
// GET /webhook_subscriptions?scope=organization&organization=<org_a>

Status: 403 Permission Denied
```

# Calendly OAuth 2.0

API Base URL
- **Live Server:** https://auth.calendly.com
- **Mock Server:** https://stoplight.io/mocks/calendly/api-docs/591407

## Security

### Basic Auth
Use `client_id` for username and `client_secret` for password.

Basic authentication is a simple authentication scheme built into the HTTP protocol. To use it, send your HTTP requests with an Authorization header that contains the word Basic followed by a space and a base64-encoded string `username:password`.

Example: `Authorization: Basic ZGVtbzpwQDU1dzByZA==`

### OAuth 2.0

#### Additional Information
- Contact Calendly Developer Support
- Terms of Service

Use your application's client ID and secret to authorize using OAuth.
* For mobile or native applications, use a specific `redirect_uri`, Proof Key for Code Exchange (PKCE), and SH256 for `code_challenge_method`. For more information, see this native and mobile authentication guide.
* To retrieve the access token, you'll need to access Calendly data:
   1. To initiate the OAuth authorization flow, redirect your user to Get Authorization Code endpoint.
   2. To retrieve the access token, make a POST request to Get Access Token endpoint.
   3. To test the access token, make a call to the Get Current User endpoint.

## Get Authorization Code

```http
GET https://auth.calendly.com/oauth/authorize
```

The **Authorization Code** is a temporary code that your client exchanges for an access token.

### Web applications
To receive user's Access Token, have your app redirect the user to Calendly's authorization page with the `client_id` and `redirect_uri` replaced with your application's `client_id` and `redirect_uri` (see example below). Note that this url must be requested using a web browser.

```
https://auth.calendly.com/oauth/authorize?client_id=CLIENT_ID&response_type=code&redirect_uri=https://my.site.com/auth/calendly
```

When a user grants access, their browser is redirected to the specified `redirect_uri`, the Authorization Code is passed inside the `code` query parameter:

```
https://my.site.com/auth/calendly?code=f04281d639d8248435378b0365de7bd1f53bf452eda187d5f1e07ae7f04546d6
```

### Native applications
The flow for mobile or native applications requires PKCE conforming to the RFC 7636 specification. For more information, see this guide. An example of a javascript implementation can be found here.

To receive an Authorization Code:
* Generate a `CODE_VERIFIER`
* Build a `CODE_CHALLENGE`
* Redirect the user to Calendly's authorization page with the `client_id`, `redirect_uri`, and `code_challenge` replaced with your application's `client_id`, `redirect_uri`, and the `code_challenge` generated in the step above (see example below). Note that this url must be requested using a web browser.

```
https://auth.calendly.com/oauth/authorize?client_id=CLIENT_ID&response_type=code&redirect_uri=com.site.app://auth/calendly&code_challenge_method=S256&code_challenge=CODE_CHALLENGE
```

After the user grants access, they will be redirected back to your app with the Authorization Code:

```
com.site.app://auth/calendly?code=f04281d639d8248435378b0365de7bd1f53bf452eda187d5f1e07ae7f04546d6
```

### Request

#### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| **client_id** | string | required | The ID provided by Calendly for the web application |
| **code_challenge** | string | required | Use a code challenge for native clients |
| **code_challenge_method** | string | required | The method used for native clients<br>Examples: S256 |
| **redirect_uri** | string | required | The redirect URI for your OAuth application<br>Examples: https://my.site.com/auth/calendly |
| **response_type** | string | required | The response type (which is always "code")<br>Allowed value: code |

### Responses

**200** - OK

# Get Access Token

```http
POST https://auth.calendly.com/oauth/token
```

Use Access Tokens to access Calendly resources on behalf of an authenticated user.

The Basic HTTP Authentication security scheme is for web clients only.

To make a call to this endpoint:
* Web clients: pass client_id and client_secret via Basic HTTP Authorization header
* Native clients: include client_id as a body param within the API call

Note:
* Access Tokens expire after 2 hours.
* Refresh Tokens don't expire until they are used.

## Request

### Security: Basic Auth

### Headers

| Header | Type | Required | Description |
|--------|------|----------|-------------|
| **Content-Type** | string | required | Indicates the media type of the resource<br>Allowed value: application/x-www-form-urlencoded<br>Default: application/x-www-form-urlencoded |

### Body
application/x-www-form-urlencoded

#### Web Token Request with Authorization Code
(any of)

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| **grant_type** | string | required | The method used to retrieve an access token<br>Allowed value: authorization_code |
| **code** | string | required | The authorization code from an OAuth response |
| **redirect_uri** | string<uri> | required | The redirect URI. Note this must be the same as the **redirect_uri** passed in the authorization request.<br>Example: https://my.site.com/auth/calendly |

## Responses

**200** - OK  
**400**  
**401**  

### Body
application/json

OAuth token response

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **token_type** | string | required | The token type (currently this is always "Bearer")<br>Allowed value: Bearer |
| **access_token** | string | required | The access token provided in the response |
| **refresh_token** | string | required | The refresh token provided in the response |
| **scope** | string | | The scope of the access request<br>Allowed value: default |
| **created_at** | number | required | The UNIX timestamp in seconds when the event was created |
| **expires_in** | number | required | The number of seconds until the access token expires |
| **owner** | string<uri> | required | A link to the resource that owns the token (currently, this is always a user)<br>Example: https://api.calendly.com/users/EBHAAFHDCAEQTSEZ |
| **organization** | string<uri> | required | A link to the owner's current organization<br>Example: https://api.calendly.com/organizations/EBHAAFHDCAEQTSEZ |

## Request Sample: Shell / cURL

```bash
curl --request POST \
 --url https://auth.calendly.com/oauth/token \
 --header 'Accept: application/json' \
 --header 'Authorization: Basic 123' \
 --header 'Content-Type: application/x-www-form-urlencoded'
```

## Response Example

```json
{
  "token_type": "Bearer",
  "expires_in": 7200,
  "created_at": 1548689183,
  "refresh_token": "b77a76ffce83d3bc20531ddfa76704e584f0ee963f6041b8bfc70c91373267d5",
  "access_token": "eyJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL2F1dGguY2FsZW5kbHkuY29tIiwiaWF0IjoxNjMxNzI3Nzk5LCJqdGkiOiI5ZmViM2I0Zi04Njk1LTQzZjgtYWJhMS1kNDRiM2I5ZDdjYzEiLCJ1c2VyX3V1aWQiOiJCR0hIREdGREdIQURCSEoyIiwiYXBwX3VpZCI6IkppNzY5UjZ6eTZzSVN4X3I0OWRmZ0VsN0NPazVmeFVadVA0eHBadFlPbUkiLCJleHAiOjE2MzE3MzQ5OTl9.CrVBOFsqLjyfPK2E834E3sJv3fKU-PPlaNXQQB80Deo",
  "scope": "default",
  "owner": "https://api.calendly.com/users/EBHAAFHDCAEQTSEZ",
  "organization": "https://api.calendly.com/organizations/EBHAAFHDCAEQTSEZ"
}
```

# Get current user

```http
GET https://api.calendly.com/users/me
```

Returns basic information about your user account.

## Request

### Security: 
- OAuth 2.0
- Bearer Auth

## Responses

**200** - OK  
**401**  
**403**  
**404**  
**500**  

### Body
application/json

#### Service response

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **resource** | User | required | Information about the user. |

##### User

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **uri** | string<uri> | required | Canonical reference (unique identifier) for the user<br>Example: https://api.calendly.com/users/AAAAAAAAAAAAAAAA |
| **name** | string | required | The user's name (human-readable format)<br>Example: John Doe |
| **slug** | string | required | The portion of URL for the user's scheduling page (where invitees book sessions), rendered in a human-readable format<br>Example: acmesales |
| **email** | string<email> | required | The user's email address<br>Example: test@example.com |
| **scheduling_url** | string<uri> | required | The URL of the user's Calendly landing page (that lists all the user's event types)<br>Example: https://calendly.com/acmesales |
| **timezone** | string | required | The time zone to use when presenting time to the user<br>Example: America/New York |
| **avatar_url** | string<uri> or null | required | The URL of the user's avatar (image)<br>Example: https://01234567890.cloudfront.net/uploads/user/avatar/0123456/a1b2c3d4.png |
| **created_at** | string<date-time> | required | The moment when the user's record was created (e.g. "2020-01-02T03:04:05.678123Z")<br>Example: 2019-01-02T03:04:05.678123Z |
| **updated_at** | string<date-time> | required | The moment when the user's record was last updated (e.g. "2020-01-02T03:04:05.678123Z")<br>Example: 2019-08-07T06:05:04.321123Z |
| **current_organization** | string<uri> | required | A unique reference to the user's current organization<br>Example: https://api.calendly.com/organizations/AAAAAAAAAAAAAAAA |
| **resource_type** | string | required | Resource type to support polymorphic associations.<br>Example: User |
| **locale** | string | required | The user's language preference<br>Allowed values: en, fr, es, de, pt, ps<br>Example: de |

# List Event Type Available Times

```http
GET https://api.calendly.com/event_type_available_times
```

Returns a list of available times for an event type within a specified date range.
Date range can be no greater than 1 week (7 days).

**NOTE:**
* This endpoint does not support traditional keyset pagination.

## Request

### Security: OAuth 2.0
Put the access token in the `Authorization: Bearer <TOKEN>` header

**Authorization Code OAuth Flow**
- Authorize URL: https://auth.calendly.com/oauth/authorize
- Token URL: https://auth.calendly.com/oauth/token
- Refresh URL: https://auth.calendly.com/oauth/token

### Security: Bearer Auth

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| **end_time** | string | required | End time of the requested availability range.<br>Examples: 2020-01-07T24:00:00.000000Z |
| **event_type** | string<uri> | required | The uri associated with the event type<br>Examples: https://api.calendly.com/event_types/AAAAAAAAAAAAAAAA |
| **start_time** | string | required | Start time of the requested availability range.<br>Examples: 2020-01-02T20:00:00.000000Z |

## Responses

**200** - OK  
**400**  
**401**  
**403**  
**404**  
**500**  

### Body
application/json

#### Service Response

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **collection** | array[Event Type Available Time] | required | The set of available times for the event type matching the criteria |

##### Event Type Available Time

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **status** | string | required | Indicates that the open time slot is "available"<br>Example: available |
| **invitees_remaining** | number | required | Total remaining invitees for this available time. For Group Event Type, more than one invitee can book in this available time. For all other Event Types, only one invitee can book in this available time.<br>Example: 2 |
| **start_time** | string<date-time> | required | The moment the event was scheduled to start in UTC time<br>Example: 2020-01-02T20:00:00.000000Z |
| **scheduling_url** | string<uri> | required | The URL of the user's scheduling site where invitees book this event type<br>Example: https://calendly.com/acmesales/discovery-call/2020-01-02T20:00:00Z?month=2020-01&date=2020-01-02 |

# Get Event Type

```http
GET https://api.calendly.com/event_types/{uuid}
```

Returns information about a specified Event Type.

## Request

### Security: OAuth 2.0
Put the access token in the `Authorization: Bearer <TOKEN>` header

**Authorization Code OAuth Flow**
- Authorize URL: https://auth.calendly.com/oauth/authorize
- Token URL: https://auth.calendly.com/oauth/token
- Refresh URL: https://auth.calendly.com/oauth/token

### Security: Bearer Auth

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| **uuid** | string | required | |

## Responses

**200** - OK  
**400**  
**401**  
**403**  
**404**  
**500**  

### Body
application/json

#### Event Type

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **uri** | string<uri> | required | Canonical reference (unique identifier) for the event type<br>Example: https://api.calendly.com/event_types/AAAAAAAAAAAAAAAA |
| **name** | string or null | required | The event type name (in human-readable format)<br>Example: 15 Minute Meeting |
| **active** | boolean | required | Indicates if the event is active or not. |
| **slug** | string or null | required | The portion of the event type's URL that identifies a specific web page (in a human-readable format)<br>Example: acmesales |
| **scheduling_url** | string<uri> | required | The URL of the user's scheduling site where invitees book this event type<br>Example: https://calendly.com/acmesales |
| **duration** | number | required | The length of sessions booked with this event type<br>Example: 15 |
| **duration_options** | array[integer] or null | required | Array of duration options. Always null for adhoc event types.<br>Example: [1,13,15,720] |
| **kind** | string | required | Indicates if the event type is "solo" (belongs to an individual user) or "group"<br>Allowed values: solo, group |
| **pooling_type** | string or null | required | Indicates if the event type is "round robin" (alternates between hosts) or "collective" (invitees pick a time when all participants are available) or "multi-pool" (considers availability delineated by pools of participants) or "null" (the event type doesn't consider the availability of a group participants)<br>Allowed values: round_robin, collective, multi_pool, null |
| **type** | string | required | Indicates if the event type is "AdhocEventType" (ad hoc event) or "StandardEventType" (standard event type)<br>Allowed values: StandardEventType, AdhocEventType |
| **color** | string | required | The hexadecimal color value of the event type's scheduling page<br>Example: #fff200<br>Match pattern: ^#[a-f\d]{6}$ |
| **created_at** | string<date-time> | required | The moment the event type was created (e.g. "2020-01-02T03:04:05.678123Z")<br>Example: 2019-01-02T03:04:05.678123Z |
| **updated_at** | string<date-time> | required | The moment the event type was last updated (e.g. "2020-01-02T03:04:05.678123Z")<br>Example: 2019-08-07T06:05:04.321123Z |
| **internal_note** | string or null | required | Contents of a note that may be associated with the event type<br>Example: Internal note |
| **description_plain** | string or null | required | The event type's description (in non formatted text)<br>Example: 15 Minute Meeting |
| **description_html** | string or null | required | The event type's description (formatted with HTML)<br>Example: <p>15 Minute Meeting</p> |
| **profile** | Profile or null | required | The publicly visible profile of a User or a Team that's associated with the Event Type (note: some Event Types don't have profiles) |
| **secret** | boolean | required | Indicates if the event type is hidden on the owner's main scheduling page |
| **booking_method** | string | required | Indicates if the event type is for a poll or an instant booking<br>Allowed values: instant, poll<br>Example: poll |
| **custom_questions** | array[EventTypeCustomQuestion] | required | |
| **deleted_at** | string<date-time> or null | required | The moment the event type was deleted (e.g. "2020-01-02T03:04:05.678123Z"). Since event types can be deleted but their scheduled events remain it's useful to fetch a deleted event type when you still require event type data for a scheduled event.<br>Example: 2019-01-02T03:04:05.678123Z |
| **admin_managed** | boolean | required | Indicates if this event type is managed by an organization admin |
| **locations** | array[object] or null | required | Configuration information for each possible location for this Event Type |
| **position** | integer | required | Position order of Event Type, starting with 0 (for display purposes) |

# List User's Event Types

```http
GET https://api.calendly.com/event_types
```

Returns all Event Types associated with a specified User. Use:
- `organization` to look up all Event Types that belong to the organization
- `user` to look up a user's Event Types in an organization

Either `organization` or `user` are required query params when using this endpoint.

## Request

### Security: OAuth 2.0
Put the access token in the `Authorization: Bearer <TOKEN>` header

**Authorization Code OAuth Flow**
- Authorize URL: https://auth.calendly.com/oauth/authorize
- Token URL: https://auth.calendly.com/oauth/token
- Refresh URL: https://auth.calendly.com/oauth/token

### Security: Bearer Auth

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| **admin_managed** | boolean | Return only admin managed event types if true, exclude admin managed event types if false, or include all event types if this parameter is omitted. |
| **organization** | string<uri> | View available personal, team, and organization event types associated with the organization's URI. |
| **user** | string<uri> | View available personal, team, and organization event types associated with the user's URI. |
| **user_availability_schedule** | string<uri> | Used in conjunction with user parameter, returns a filtered list of Event Types that use the given primary availability schedule. |
| **active** | boolean | Return only active event types if true, only inactive if false, or all event types if this parameter is omitted. |
| **count** | number | The number of rows to return<br>>= 1<br><= 100<br>Default: 20 |
| **page_token** | string | The token to pass to get the next or previous portion of the collection |
| **sort** | string | Order results by the specified field and direction. Accepts comma-separated list of {field}:{direction} values.<br>Supported fields are: name, position, created_at, updated_at. Sort direction is specified as: asc, desc.<br>Default: name:asc<br>Example: name |

## Responses

**200** - OK  
**400**  
**401**  
**403**  
**404**  
**500**  

### Body
application/json

#### Service response

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **collection** | array[Event Type] | required | |
| **pagination** | Pagination | required | |

##### Event Type

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **uri** | string<uri> | required | Canonical reference (unique identifier) for the event type<br>Example: https://api.calendly.com/event_types/AAAAAAAAAAAAAAAA |
| **name** | string or null | required | The event type name (in human-readable format)<br>Example: 15 Minute Meeting |
| **active** | boolean | required | Indicates if the event is active or not. |
| **slug** | string or null | required | The portion of the event type's URL that identifies a specific web page (in a human-readable format)<br>Example: acmesales |
| **scheduling_url** | string<uri> | required | The URL of the user's scheduling site where invitees book this event type<br>Example: https://calendly.com/acmesales |
| **duration** | number | required | The length of sessions booked with this event type<br>Example: 15 |
| **duration_options** | array[integer] or null | required | Array of duration options. Always null for adhoc event types.<br>Example: [1,13,15,720] |
| **kind** | string | required | Indicates if the event type is "solo" (belongs to an individual user) or "group"<br>Allowed values: solo, group |
| **pooling_type** | string or null | required | Indicates if the event type is "round robin" (alternates between hosts) or "collective" (invitees pick a time when all participants are available) or "multi-pool" (considers availability delineated by pools of participants) or "null" (the event type doesn't consider the availability of a group participants)<br>Allowed values: round_robin, collective, multi_pool, null |
| **type** | string | required | Indicates if the event type is "AdhocEventType" (ad hoc event) or "StandardEventType" (standard event type)<br>Allowed values: StandardEventType, AdhocEventType |
| **color** | string | required | The hexadecimal color value of the event type's scheduling page<br>Example: #fff200<br>Match pattern: ^#[a-f\d]{6}$ |
| **created_at** | string<date-time> | required | The moment the event type was created (e.g. "2020-01-02T03:04:05.678123Z")<br>Example: 2019-01-02T03:04:05.678123Z |
| **updated_at** | string<date-time> | required | The moment the event type was last updated (e.g. "2020-01-02T03:04:05.678123Z")<br>Example: 2019-08-07T06:05:04.321123Z |
| **internal_note** | string or null | required | Contents of a note that may be associated with the event type<br>Example: Internal note |
| **description_plain** | string or null | required | The event type's description (in non formatted text)<br>Example: 15 Minute Meeting |
| **description_html** | string or null | required | The event type's description (formatted with HTML)<br>Example: <p>15 Minute Meeting</p> |
| **profile** | Profile or null | required | The publicly visible profile of a User or a Team that's associated with the Event Type (note: some Event Types don't have profiles) |
| **secret** | boolean | required | Indicates if the event type is hidden on the owner's main scheduling page |
| **booking_method** | string | required | Indicates if the event type is for a poll or an instant booking<br>Allowed values: instant, poll<br>Example: poll |
| **custom_questions** | array[EventTypeCustomQuestion] | required | |
| **deleted_at** | string<date-time> or null | required | The moment the event type was deleted (e.g. "2020-01-02T03:04:05.678123Z"). Since event types can be deleted but their scheduled events remain it's useful to fetch a deleted event type when you still require event type data for a scheduled event.<br>Example: 2019-01-02T03:04:05.678123Z |
| **admin_managed** | boolean | required | Indicates if this event type is managed by an organization admin |
| **locations** | array[object] or null | required | Configuration information for each possible location for this Event Type |
| **position** | integer | required | Position order of Event Type, starting with 0 (for display purposes) |

##### Pagination

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **count** | integer | required | The number of rows to return<br>>= 0<br><= 100<br>Example: 20 |
| **next_page** | string<uri> or null | required | URI to return the next page of an ordered list ("null" indicates no additional results are available)<br>Example: https://api.calendly.com/event_types?count=1&page_token=sNjq4TvMDfUHEl7zHRR0k0E1PCEJWvdi |
| **previous_page** | string<uri> or null | required | URI to return the previous page of an ordered list ("null" indicates no additional results are available)<br>Example: https://api.calendly.com/event_types?count=1&page_token=VJs2rfDYeY8ahZpq0QI1O114LJkNjd7H |
| **next_page_token** | string or null | required | Token to return the next page of an ordered list ("null" indicates no additional results are available)<br>Example: sNjq4TvMDfUHEl7zHRR0k0E1PCEJWvdi |
| **previous_page_token** | string or null | required | Token to return the previous page of an ordered list ("null" indicates no additional results are available)<br>Example: VJs2rfDYeY8ahZpq0QI1O114LJkNjd7H |

# List User Availability Schedules

```http
GET https://api.calendly.com/user_availability_schedules
```

Returns the availability schedules of the given user.

## Request

### Security: OAuth 2.0
Put the access token in the `Authorization: Bearer <TOKEN>` header

**Authorization Code OAuth Flow**
- Authorize URL: https://auth.calendly.com/oauth/authorize
- Token URL: https://auth.calendly.com/oauth/token
- Refresh URL: https://auth.calendly.com/oauth/token

### Security: Bearer Auth

### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| **user** | string<uri> | required | A URI reference to a user<br>Example: https://api.calendly.com/users/abc123 |

## Responses

**200** - OK  
**400**  
**401**  
**403**  
**404**  
**500**  

### Body
application/json

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **collection** | array[User Availability Schedule] | required | |

#### User Availability Schedule

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **uri** | string<uri> | required | A URI reference to this Availability Schedule.<br>Example: https://api.calendly.com/user_availability_schedule/abc123 |
| **default** | boolean | required | This is the default Availability Schedule in use. |
| **name** | string | required | The name of this Availability Schedule.<br>Example: Working Hours |
| **user** | string<uri> | required | A URI reference to a User.<br>Example: https://api.calendly.com/users/abc123 |
| **timezone** | string | required | The timezone for which this Availability Schedule is originated in.<br>Example: America/New_York |
| **rules** | array[Availability Rule] | required | The rules of this Availability Schedule. |

# Get User Availability Schedule

```http
GET https://api.calendly.com/user_availability_schedules/{uuid}
```

This will return the availability schedule of the given UUID.

## Request

### Security: OAuth 2.0
Put the access token in the `Authorization: Bearer <TOKEN>` header

**Authorization Code OAuth Flow**
- Authorize URL: https://auth.calendly.com/oauth/authorize
- Token URL: https://auth.calendly.com/oauth/token
- Refresh URL: https://auth.calendly.com/oauth/token

### Security: Bearer Auth

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| **uuid** | string | required | The UUID of the availability schedule. |

## Responses

**200** - OK  
**400**  
**401**  
**403**  
**404**  
**500**  

### Body
application/json

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **resource** | User Availability Schedule | required | The availability schedule set by the user. |

#### User Availability Schedule

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| **uri** | string<uri> | required | A URI reference to this Availability Schedule.<br>Example: https://api.calendly.com/user_availability_schedule/abc123 |
| **default** | boolean | required | This is the default Availability Schedule in use. |
| **name** | string | required | The name of this Availability Schedule.<br>Example: Working Hours |
| **user** | string<uri> | required | A URI reference to a User.<br>Example: https://api.calendly.com/users/abc123 |
| **timezone** | string | required | The timezone for which this Availability Schedule is originated in.<br>Example: America/New_York |
| **rules** | array[Availability Rule] | required | The rules of this Availability Schedule. |

# How to get event type and user calendar availability data

## Event Type Availability

Calendly users can retrieve information about availability for a specific event type when they generate and use a personal access token or OAuth token to authenticate requests to Calendly's API.

### Use cases:
* Take appropriate action based on available times for a given event type.
* Effectively share available meeting times from an external application to increase the probability of meetings being booked and enhance the invitee experience.
   * For example:
      * Display specific available times when sending communications to invitees directly from external applications.
      * Include and share first 'X' available times.

### To retrieve a list of available times for a specific event type within a specified date range:

1. Make a call to `/event_types` to retrieve a list of all event types (using the organization scope to get all event types across the organization or using the user scope to get all event types for a specific user). The endpoint will return a collection of event types with an event type uri for each item in the array.

2. Make a call to `/event_type_available_times` and pass the event type uri for the event type you would like to retrieve information about. Please note that `start_time` and `end_time` parameters are required, must be in the future, and cannot be a range greater than 7 days.

## User Calendar Availability

Calendly users can retrieve information about the availability of users in their organization when they generate and use a personal access token or OAuth token to authenticate requests to Calendly's API.

### Use cases:
* Programmatically calculate total hours available for a given user for a given period of time. For example:
   * Total available hours
   * Total scheduled hours
* Build a custom 'Calendar View' for a given user.
* View availability schedules and date overrides to determine if a user needs to open up more availability options.

### To retrieve information about a specific user's availability schedule:

1. Make a call to `/organization_memberships` to return a collection of users in an organization which includes a user.uri for each item in the array.

2. Make a call to `/user_availability_schedules` and pass the specified user.uri.

### To retrieve busy times for a specific user (based upon the connected calendars marked as "Check for Conflicts" in Calendly):

1. Make a call to `/organization_memberships` to return a collection of users in an organization which includes a user.uri for each item in the array.

2. Make a call to `/user_busy_times` and pass the specified user.uri. Please note that `start_time` and `end_time` parameters are required, must be in the future, and cannot be a range greater than 7 days.

# Frequently Asked Questions

## What Calendly subscription do I need to use the API?
Developers can make GET and POST requests to API endpoints on behalf of a Calendly user on any subscription plan, including the Free plan (with the exception of a few endpoints that are specific to our Enterprise plan).

The Enterprise-specific endpoints are:
- List activity entry logs
- Delete invitee data
- Delete scheduled event data

As for webhooks, the Calendly user account for which the webhook subscription is being made will need a paid subscription on the Standard, Teams, or Enterprise plan. You can read more about the features of each subscription on our pricing page.

For the v2 API, the developer's account is not connected to their Calendly account even if the same email address is utilized.

## Can I schedule an event through the API?
At this time, our API endpoints do not support scheduling events. To schedule an event from your website, you can use one of our embed options.

## Can I cancel or reschedule an event through the API?
Yes, to cancel an event via the API, you can send a POST request to the cancel event endpoint.

At this time, there is no API endpoint that supports rescheduling an event or invitee, however the cancel and reschedule URLs are on the invitee resource.

However, the cancel and reschedule URLs can be found on the v2 API webhook payload as well. You can see an example of the payload here.

These URLs are always inserted at the bottom of calendar events or email confirmations, but they are available on the API to be more easily retrievable so you can place them wherever you want in the app that you are building.

Please note that cancel event endpoint can be used to cancel an event, including a Group event, but not an individual invitee in a Group event.

## Can I update my schedule or availability through the API?
At this time, our API endpoints do not support setting or managing availability. To set availability, users will need to visit their Event Types page within their Calendly account.

## Can I create or manage an event type through the API?
At this time, our API endpoints do not support creating or managing event types. To manage an event or event type, users will need to visit their Event Types page within their Calendly account.

## How do I get my Organization or User URI?
Using either OAuth or personal access tokens, you can make a call to the Get User API endpoint or Get Current User endpoint to receive the user value on the uri and the organization value on the current_organization value on the response.

If you are using OAuth with the v2 API, the user and organization URI is given in the payload of the access token as the value of the owner and organization fields.

If you already know the user or organization URI, another way to get the values is to make a GET request to the List Organization Memberships endpoint which returns a collection of all the Calendly members. In the user object of each collection, Calendly returns the user's organization URI.

Finally, in regards to the format of these values:
- URI format: https://api.calendly.com/organizations/012345678901234567890
- The authority: https://api.calendly.com
- The path: organizations
- The UUID: 012345678901234567890

## Why is an organization URI required when using the user scope in webhook?
An organization URI value is needed with a user-scoped webhook because a user can have multiple organizations associated with their data in Calendly, such as when they had their personal Calendly account before they were a member of an organization or after they leave an organization. As such, the organization is needed to confirm the specific user data that should be returned.

## What happens to a webhook subscription when the user is removed from the organization?
If an administrator is removed from an organization, organization-scoped webhooks created by the removed administrator will continue to work.

An administrator can delete organization-scoped webhooks created by another administrator using the Delete Webhook Subscription endpoint.

If a user is removed from an organization, user-scoped webhooks created by the removed user will no longer work.

## What happens to organization-scoped webhooks when the user's role changes from an Administrator to User?
If a user's role changes from an administrator to a user, organization-scoped webhooks will continue to work and will remain scoped to the organization.

## How do I get additional invitee information that is not included in the webhook payload?
The v2 API webhook payload provides a JSON object as seen here in the documentation. Note that this object does not expose all of the invitee details. To get further details about the event, the webhook payload includes the invitee URI which can be used to make a GET request to the Get Event Invitee endpoint to expose additional invitee details.

## How long is a single-use scheduling link valid for?
Single use scheduling links (that haven't been used to book an event) expire after 90 days.

## Can I retrieve information for a User that has been removed from the Organization?
When a user is removed from the organization, they become a solo account. Events that were scheduled with the user while they were part of the organization are still associated with the organization. You will be able to see a removed user's events when you use the organization parameter when making a call to the List Events endpoint (using an access token associated with an Owner or Admin in the organization). Note that you will only see events that were scheduled when the user was part of the organization; any events scheduled with the solo account after removal will not be included.

## Can custom query parameters be passed into the booking page and retrieved via the API?
Yes, to send unique or custom query parameters into your scheduled event, you can use UTM parameters on your scheduling link to pass in your custom data values. This data will be hidden from the booking flow, but will be visible on your URL.

When you add these to your scheduling URL, this data will then be associated with the event and available in webhook payload (tracking) and when making a call to the List Event Invitees endpoint (tracking). More information about using UTM parameters can be found right here.

The UTM parameters are: utm_source, utm_medium, utm_campaign, utm_content, and utm_term. While these are typically used for tracking and conversion purposes, you're welcome to use other values in these such as a userID or anything else you need to track.

For example:
```
https://calendly.com/YOUR_EVENT?utm_source=12345&utm_medium=Smith
```

This page is also a great reference for passing UTM parameters specifically to a Calendly embed, if you are currently utilizing one of our embed options.

## What are the conditions that result in an access token being revoked?
Both OAuth access tokens and personal access tokens can be revoked manually, but there are also certain conditions in the Calendly account that will cause the token to be revoked.

Both OAuth and personal access tokens will be revoked if:
- The account's login email is changed.
- The account's password is changed.
- The account's login method is changed.

Access tokens can also be revoked manually. OAuth access tokens can be revoked by submitting a POST request to the revoke access/refresh token endpoint. Personal access tokens can be revoked by clicking 'Revoke' next to the token on the account's API & Webhooks page.