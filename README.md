# AsyncAPI Topic Definition

## Introduction

The purpose of this document is to describe how an AsyncAPI topic should be structured. The goal is to create a definition that suites most use cases and establish the foundation for community tooling and better interoperability between products when using AsyncAPI.

## What is a topic?

A topic is a string representing where an AsyncAPI can publish or subscribe. For the sake of comparison they are like URLs in a REST API.

## Components

Below we describe the components of an AsyncAPI topic.

### Organization

The name of the organization or company.

###### Example:

**`hitch`**.`accounts`.`1`.`0`.`event`.`user`.`signup`

### Service

The service or department in charge of managing the message.

###### Example:

`hitch`.**`accounts`**.`1`.`0`.`event`.`user`.`signup`

### Message version

The version of the message for the given service. We use a variation of semantic versioning, with only the **major** and **minor** numbers. This way, a service can safely listen/publish to all `X.*` versions because the message format does not contain any breaking changes.

###### Example:

`hitch`.`accounts`.**`1`.`0`**.`event`.`user`.`signup`

> Note: This version number is NOT related to your service or program version.

### Message type

It contains the type of the message, e.g. *it is an `action` or an `event`*. This value should always be `event` unless you're trying to explicitly execute an action in the service, i.e. when using RPC.

###### Example:

`hitch`.`accounts`.`1`.`0`.**`event`**.`user`.`signup`

`hitch`.`email`.`1`.`0`.**`action`**.`user`.`welcome`.`send`

### Resources and sub-resources

A word (or words) describing the resource the message refers to. For instance, if you're sending a message to notify a user has just signed up, the resource should be `user`. But, if you want to send a message to notify a user has just changed her full name, you could name it as `user.full_name`.

###### Example:

`hitch`.`accounts`.`1`.`0`.`event`.**`user`**.`signup`

`hitch`.`email`.`1`.`0`.`action`.**`user`.`welcome`**.`send`

### Event/Action name

A verb **in infinitive form** describing what happened to the resource (in case of event) or what operation you want to perform (in case of action).

###### Example:

`hitch`.`accounts`.`1`.`0`.`event`.`user`.**`signup`**

`hitch`.`email`.`1`.`0`.`action`.`user`.`welcome`.**`send`**

### Status (optional)

A word describing the status of a previous action. **When used, the type of the topic MUST be `event`.** Allowed values are:

- `queued`: The event/action has been queued.
- `succeed`: The event/action has been handled/executed successfully.
- `failed`: The event/action has failed.
- `done`: The event/action has finished.

###### Examples:

1. A user has just signed up:
`hitch`.`accounts`.`1`.`0`.`event`.`user`.`signup`

2. We send a welcome email:
`hitch`.`email`.`1`.`0`.`action`.`user`.`welcome`.`send`

3. But the email gets queued, so the email service sends a message to:
`hitch`.`email`.`1`.`0`.`event`.`user`.`welcome`.`send`.`queued`

4. The email gets finally delivered and the email service sends messages to:
   - `hitch`.`email`.`1`.`0`.`event`.`user`.`welcome`.`send`.`succeed`
   - `hitch`.`email`.`1`.`0`.`event`.`user`.`welcome`.`send`.`done`
5. Or, the recipient doesn't exist, and the email service sends messages to:
   - `hitch`.`email`.`1`.`0`.`event`.`user`.`welcome`.`send`.`failed`
   - `hitch`.`email`.`1`.`0`.`event`.`user`.`welcome`.`send`.`done`
6. The user sign up process has been completed so the accounts service sends:
`hitch`.`accounts`.`1`.`0`.`event`.`user`.`signup`.`done`
