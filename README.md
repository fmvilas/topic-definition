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

**`hitch`**.`accounts`.`1`.`event`.`user`.`signedup`

### Service/Team/Department

The service, team or department in charge of managing the message.

###### Example:

`hitch`.**`accounts`**.`1`.`event`.`user`.`signedup`

### Message version

The version of the message for the given service. This version number should remain the same unless changes in the messages are NOT backward compatible.

###### Example:

`hitch`.`accounts`.**`1`**.`event`.`user`.`signedup`

> Note: This version number is NOT related to your service or program version.

### Message type

It contains the type of the message, e.g., *is it a `command` or an `event`?*. This value should always be `event` unless you're trying to explicitly execute a command in another service, i.e., when using RPC.

###### Example:

`hitch`.`accounts`.`1`.**`event`**.`user`.`signedup`

`hitch`.`email`.`1`.**`command`**.`user`.`welcome`.`send`

### Resources and sub-resources

A word (or words) describing the resource the message refers to. For instance, if you're sending a message to notify a user has just signed up, the resource should be `user`. But, if you want to send a message to notify a user has just changed her full name, you could name it as `user.full_name`.

###### Example:

`hitch`.`accounts`.`1`.`event`.**`user`**.`signedup`

`hitch`.`email`.`1`.`command`.**`user`.`welcome`**.`send`

### Event/Command name

In case message type is `event`, this should be a verb **in past tense** describing what happened to the resource.

In case message type is `command`, this should be a verb **in infinitive form** describing what operation you want to perform.

###### Example:

`hitch`.`accounts`.`1`.`event`.`user`.**`signedup`**

`hitch`.`email`.`1`.`command`.`user`.`welcome`.**`send`**

### Status (optional)

A word describing the status of a previous **command**. When used, **the type of the topic MUST be `event`.** Allowed values are:

- `queued`: The command has been queued.
- `succeeded`: The command has been handled/executed successfully.
- `failed`: The command has failed.
- `done`: The command has finished.

###### Examples:

1. A user has just signed up:
`hitch`.`accounts`.`1`.`event`.`user`.`signedup`

2. We send a welcome email:
`hitch`.`email`.`1`.`command`.`user`.`welcome`.`send`

3. But the email gets queued, so the email service sends a message to:
`hitch`.`email`.`1`.`event`.`user`.`welcome`.`send`.`queued`

4. The email gets finally delivered and the email service sends messages to:
   - `hitch`.`email`.`1`.`event`.`user`.`welcome`.`send`.`succeed`
   - `hitch`.`email`.`1`.`event`.`user`.`welcome`.`send`.`done`
5. Or, the recipient doesn't exist, and the email service sends messages to:
   - `hitch`.`email`.`1`.`event`.`user`.`welcome`.`send`.`failed`
   - `hitch`.`email`.`1`.`event`.`user`.`welcome`.`send`.`done`
6. The user sign up process has been completed so the accounts service sends:
`hitch`.`accounts`.`1`.`event`.`user`.`signup`.`done`
