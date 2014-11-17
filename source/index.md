---
title: API Reference

language_tabs:
  - shell
  - php

toc_footers:
  - Documentation Powered by <a href='http://github.com/tripit/slate'>Slate</a>

includes:
  - errors

search: true
---

# Overview

The Simply Satisfied API is designed to allow you to integrate Simply Satisfied's feedback tools into your own web or mobile application.

Designed around [REST](http://en.wikipedia.org/wiki/Representational_state_transfer) principles, the aim of the API is to return sensible responses from entity-based endpoint calls, and make it as easy as possible to get access to your account's data.

If you have any suggestions to improve the API, please send them along to [support@simplysatisfied.net](mailto:support@simplysatisfied.net), and we'll take them on board.

## Namespaced data return format.

```json
{
  "data": {
    "key1":"Your data here",
    "key2":"Some more data here"
  },
  "meta": {
    "page":1
  }
}
```

Just a note that Simply's API follows the standard of returning response data wrapped in a `data` namespace, that means that when responses come back, they will be available in a nested `data` property.

For "listing" endpoints, `data` will be a collection, where as for endpoints that load a specific ID, they generally return a single object.

## PHP API wrapper

The official PHP API wrapper is available on [Github](http://github.com/SimplySatisfied/simply-php), we will be adding more languages in the future.

You can also install it using composer adding `simplysatisfied/simply-php` to your `composer.json`.

# Authentication

```php
<?php

$client = new Simply\Api('your-api-key-here');
```

```shell
$ curl https://app.simplysatisfied.net/api/v1/
  -H "X-Simply-Auth: your-api-key-here"
```

Authentication with our API is super-easy, just grab your API Key from the *Tools* section of the Simply Satisfied web interface.

You can then provide that API key via a special header called `X-Simply-Auth`, and that's it!

# Users

## List all users

```php
<?php

$api = new Simply\Api('your-api-key-here');

$users = $api->users->all()->get();
```

```shell
$ curl "https://app.simplysatisfied.net/api/v1/user"
  -H "X-Simply-Auth: your-api-key-here"
```

> Example response:

```json
{
  "data":[
    {
      "id": 1,
      "email": "andy@example.com",
      "name": "Andy Dwyer",
      "confirmed": true,
      "company_id": 1,
      "created_at": "2014-10-10 15:51:51",
      "updated_at": "2014-10-10 15:51:51",
      "remember_token": ""
    },
    {
      "id": 2,
      "email": "ron@example.com",
      "name": "Ron Swanson",
      "confirmed": true,
      "company_id": 1,
      "created_at": "2014-10-10 15:51:51",
      "updated_at": "2014-10-10 15:51:51",
      "remember_token": ""
    }
  ],
  "meta":{
    "pagination":{
      "total":2,
      "count":2,
      "per_page":30,
      "current_page":1,
      "total_pages":1,
      "links":[]
    }
  }
}
```

This endpoint retrieves all users for your company's account.

### HTTP Request

`GET https://app.simplysatisfied.net/api/v1/user`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
`include` | null | Include related datasets, `company` is the only related data available for this endpoint.
`page` | 1 | Which page of results to return.


## Get a single user

```php
<?php

$api = new Simply\Api('your-api-key-here');

$users = $api->users->find(21)->get();
```

```shell
$ curl "https://app.simplysatisfied.net/api/v1/user/21"
  -H "X-Simply-Auth: your-api-key-here"
```

> Example response:

```json
{
  "data":{
    "id": 1,
    "email": "tom@example.me",
    "name": "Tom Haverford",
    "confirmed": true,
    "company_id": 1,
    "created_at": "2014-10-10 15:51:51",
    "updated_at":"2014-10-10 15:51:51",
    "remember_token":""
  }
}
```

This endpoint retrieves a specific user.

### HTTP Request

`GET https://app.simplysatisfied.net/api/v1/user/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
`{id}` | The ID of the user to retrieve

### Query Parameters

Parameter | Default | Description
--------- | ----------- | -----------
`include` | null | Include related data, this endpoint only accepts `company` as an include.

## Update a user

```shell
$ curl https://simplysatisfied.net/api/v1/user/{id} \
   -H "X-Simply-Auth: your-api-key-here" \
   -d name=Swan Ronson
```

```php
<?php

$api = new Simply\Api('your-api-key-here');

$api->users->update(21, [
  'name' => 'Swan Ronson',
])->makeRequest();
```

Sometimes you may need to update a user, wether it be to update their name, email or preferences.

### HTTP Request

`PATCH https://app.simplysatisfied.net/api/v1/user/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
`{id}` | The ID of the user to update.

# Company

## List all companies

```php
<?php

$api = new Simply\Api('your-api-key-here');

$users = $api->company->all()->get();
```

```shell
$ curl "https://app.simplysatisfied.net/api/v1/company"
  -H "X-Simply-Auth: your-api-key-here"
```

> Example response:

```json
{
  "data":[
    {
      "id": 1,
      "name": "Example Company",
      "url": "http://acme.org",
      "commercial_sector":null,
      "widget_key": "5pqZNWcD3r3sBAOrQg5ToMCrdwTvi7em",
      "created_at": "2014-10-10 15:51:51",
      "updated_at": "2014-10-10 15:52:24"
    }
  ],
  "meta":{
    "pagination":{
      "total":1,
      "count":1,
      "per_page":30,
      "current_page":1,
      "total_pages":1,
      "links":[]
    }
  }
}
```

List all companies your current user is attached to.

### HTTP Request:
`GET http://simplystaisfied.net/api/v1/company`

<aside class="notice">
Currently Simply Satisfied only allows you to be associated with **one** company, and while this *may* change in the future, we have included both "list all" and "view one" methods in the API, to allow for future expansion and consistency with other endpoints.
</aside>

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
`page` | 1 | Which page of results to return.
`count` | 30 | Number of results per page to return

## Get a single company

```php
<?php

$api = new Simply\Api('your-api-key-here');

$users = $api->company->find(23)->get();
```

```shell
$ curl "https://app.simplysatisfied.net/api/v1/company/23"
  -H "X-Simply-Auth: your-api-key-here"
```

> Example response:

```json
{
  "data":
    {
      "id": 1,
      "name": "Example Company",
      "url": "http://acme.org",
      "commercial_sector":null,
      "widget_key": "5pqZNWcD3r3sBAOrQg5ToMCrdwTvi7em",
      "created_at": "2014-10-10 15:51:51",
      "updated_at": "2014-10-10 15:52:24"
    }
}
```

### HTTP Request

`GET https://app.simplysatisfied.net/api/v1/company/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
`{id}` | The ID of the company to retrieve.

## Update a company

```php
<?php

$api = new Simply\Api('your-api-key-here');

$users = $api->company->update(23, [
  'name' => 'Acme Classic',
  'url' => 'http://acmeclassic.com',
]);
```

```shell
$ curl -X PATCH "https://app.simplysatisfied.net/api/v1/company" \
  -H "X-Simply-Auth: your-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{"name":"Acme Classic","url":"http://acmeclassic.com"}'
```

>Example JSON payload:

```json
{
  "name":"Acme Classic Inc.",
  "url":"http://acmeclassic.com"
}
```

This endpoint allows you to update your company data via the API.

### HTTP Request:

`PATCH https://app.simplysatisfied.net/api/v1/company/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
`{id}` | The ID of the company to update.

# Invites
Invites are the backbone of Simply Satisfied - you cannot create a Response without a valid invite, and the invite stores all the customer data including email, name and an optional reference number.

### Automatic emails
```json
{
  "send_email":false
}
```

Creating an invite through the API automatically sends a lovely looking email invitation to your customers to ask them to give you feedback, but what if you don't want to send that email?

Invites **require** a name and an email address to verify that they're going to real people, but you can set `send_email = false` during creation to halt the sending of an email invitation to the customer.

This is especially handy if you are building an application where you're collecting this information on the fly, then want to redirect the user to a feedback form (i.e. in e-commerce, or when using your SaaS app).

## List all invites

```shell
$ curl https://app.simplysatisfied.net/api/v1/invite
  -H "X-Simply-Auth: your-api-key-here"
```

```php
<?php

$api = new Simply\Api('your-api-key-here');
$invites = $api->invites->all()->get();
```

>Example response:

```json
{
  "data": [
    {
      "id": 1,
      "survey_id": 1,
      "user_id": 1,
      "email": "fwolff@example.net",
      "name": "Madeline Eichmann",
      "message": "Perspiciatis nihil et est at maiores amet. Eum aut rerum aliquid non. Sunt et neque libero laboriosam fuga ex et ut.",
      "reference": "test",
      "viewed": false,
      "responded": false,
      "opened": false,
      "send_email": false,
      "response_hash": "va5B3ma2ZAWwZA47",
      "bounced": false,
      "send_reminders": 1,
      "reminder_unsubscribe_key": "h3s4KsDcMVq3nhMm",
      "created_at": "2014-08-03 02:09:00",
      "updated_at": "2014-10-10 17:23:09",
      "survey_revision_id": 1,
      "link": "http://simply.local:8000/s/va5B3ma2ZAWwZA47"
    },
    {
      "id": 2,
      "survey_id": 1,
      "user_id": 1,
      "email": "tmckenzie@example.com",
      "name": "Dina Deckow",
      "message": "Quis et hic est nisi quod corrupti magni voluptatem. Ea nulla quo pariatur quis et magnam ea quos. Consequuntur dolorem excepturi est unde nam aut a.",
      "reference": "test",
      "viewed": false,
      "responded": false,
      "opened": false,
      "send_email": false,
      "response_hash": "zVidLqkXSfkfcncV",
      "bounced": false,
      "send_reminders": 1,
      "reminder_unsubscribe_key": "f1KuVjhF6j5sPOXX",
      "created_at": "2014-04-03 23:03:34",
      "updated_at": "2014-10-10 17:23:09",
      "survey_revision_id": 1,
      "link": "http://simply.local:8000/s/zVidLqkXSfkfcncV"
    }
  ],
  "meta": {
    "pagination": {
      "total": 101,
      "count": 2,
      "per_page": 2,
      "current_page": 1,
      "total_pages": 51,
      "links": {
        "next": "http://simply.local:8000/api/v1/invite?page=2"
      }
    }
  }
}
```

List all invites your have sent, ordered by most recent first, and paginated.

### HTTP Request:

`GET https://app.simplysatisfied.net/api/v1/invite`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
`page` | 1 | Which page of results to return.
`count` | 30 | Number of results per page to return

## Get a single invite


```shell
$ curl https://app.simplysatisfied.net/api/v1/invite/{id}
  -H "X-Simply-Auth: your-api-key-here"
}
```

```php
<?php

$api = new Simply\Api('your-api-key-here');
$invites = $api->invites->find(2)->get();
```

>Example response:

```json
{
  "data":
    {
      "id": 2,
      "survey_id": 1,
      "user_id": 1,
      "email": "tmckenzie@example.com",
      "name": "Dina Deckow",
      "message": "Quis et hic est nisi quod corrupti magni voluptatem. Ea nulla quo pariatur quis et magnam ea quos. Consequuntur dolorem excepturi est unde nam aut a.",
      "reference": "test",
      "viewed": false,
      "responded": false,
      "opened": false,
      "send_email": false,
      "response_hash": "zVidLqkXSfkfcncV",
      "bounced": false,
      "send_reminders": 1,
      "reminder_unsubscribe_key": "f1KuVjhF6j5sPOXX",
      "created_at": "2014-04-03 23:03:34",
      "updated_at": "2014-10-10 17:23:09",
      "survey_revision_id": 1,
      "link": "http://simply.local:8000/s/zVidLqkXSfkfcncV"
    }
}
```
### HTTP Request:

`GET https://app.simplysatisfied.net/api/v1/invite/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
`{id}` | The ID of the invite to retrieve

## Send a new invite

```php
<?php

$api = new Simply\Api('your-api-key-here');

$invite = $api->invite->create([
  'name' => 'James McTavish',
  'email' => 'james@example.org',
]);
```

```shell
$ curl -X POST "https://app.simplysatisfied.net/api/v1/invite" \
  -H "X-Simply-Auth: your-api-key-here" \
  -H "Content-Type: application/json" \
  -d '{"name":"James McTavish","email":"james@example.org"}'
```

Send an invitation to a customer.

### HTTP Request:

`POST https://app.simplysatisfied.net/api/v1/invite`

<aside class="warning">
Note that creating an invite automatically sends an email invitation to the customer. If you wish to prevent this, pass the `send_email` parameter as `false`.
</aside>

# Responses

## List all responses

```php
<?php

$api = new Simply\Api('your-api-key-here');

$responses = $api->responses->all()->get();
```

```shell
$ curl -X POST "https://app.simplysatisfied.net/api/v1/response" \
  -H "X-Simply-Auth: your-api-key-here"
```

>Example response:

```json
{
  "data": [
    {
      "id": 1,
      "invite_id": 1,
      "created_at": "2014-08-27 07:16:09",
      "updated_at": "2014-10-10 17:23:09",
      "survey_revision_id": 1
    },
    {
      "id": 2,
      "invite_id": 2,
      "created_at": "2014-08-02 16:54:14",
      "updated_at": "2014-10-10 17:23:09",
      "survey_revision_id": 1
    },
    {
      "id": 3,
      "invite_id": 3,
      "created_at": "2014-04-23 16:38:44",
      "updated_at": "2014-10-10 17:23:09",
      "survey_revision_id": 1
    },
    {
      "id": 4,
      "invite_id": 4,
      "created_at": "2014-08-10 23:41:03",
      "updated_at": "2014-10-10 17:23:09",
      "survey_revision_id": 1
    },
    {
      "id": 5,
      "invite_id": 5,
      "created_at": "2014-06-09 15:15:02",
      "updated_at": "2014-10-10 17:23:09",
      "survey_revision_id": 1
    }
  ],
  "meta": {
    "pagination": {
      "total": 100,
      "count": 30,
      "per_page": 30,
      "current_page": 1,
      "total_pages": 4,
      "links": {
        "next": "http://simply.local:8000/api/v1/response?page=2"
      }
    }
  }
}
```

List all responses your have sent, ordered by most recent first, and paginated.

### HTTP Request:

`GET https://app.simplysatisfied.net/api/v1/response`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
`page` | 1 | Which page of results to return.
`count` | 30 | Number of results per page to return

## Get a single response and it's data

```php
<?php

$api = new Simply\Api('your-api-key-here');

$responses = $api->responses->find(4)->get();
```

```shell
$ curl -X POST "https://app.simplysatisfied.net/api/v1/response/4" \
  -H "X-Simply-Auth: your-api-key-here"
```

>Expected response:

```json
{
  "data": {
    "id": 4,
    "invite_id": 4,
    "created_at": "2014-08-10 23:41:03",
    "updated_at": "2014-10-10 17:23:09",
    "survey_revision_id": 1,
    "response_data": {
      "data": [
        {
          "id": 7,
          "response_id": 4,
          "value": "4",
          "created_at": "2014-08-10 23:41:03",
          "updated_at": "2014-10-10 17:23:09",
          "question_revision_id": 1,
          "standard_question": "sf_rating"
        },
        {
          "id": 8,
          "response_id": 4,
          "value": "No",
          "created_at": "2014-08-10 23:41:03",
          "updated_at": "2014-10-10 17:23:09",
          "question_revision_id": 2,
          "standard_question": "sf_recommend"
        }
      ]
    }
  }
}
```

### HTTP Request:

`GET https://app.simplysatisfied.net/api/v1/response/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
`{id}` | The ID of the response you'd like to retrieve.
