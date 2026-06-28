# OmniLink RPC Protocol

The OmniLink RPC protocol is a simple JSON-RPC 2.0 protocol over WebSockets, designed to allow integration services to communicate with the OmniLink backend.

Current Protocol Identifier: `ominilink-v1`

## Access

Integration Services must authenticate with the OmniLink backend using a pre-shared secret, which is provided in one of two ways:
- in the `Authorization` header of the WebSocket handshake request, as a Bearer token `Authorization: Bearer <secret>`
- in the `Sec-WebSocket-Protocol` header of the WebSocket handshake request, after the protocol identifier, as a secret token `Sec-WebSocket-Protocol: ominilink-v1,<secret>`

## Methods

All methods names are prefixed with an object namespace, which uses the format `module:object`, where `module` is the name of the module, and `object` is the name of the object. The object namespace is followed by a method name.

### Profiles

Object Namespace: `omnilink:profiles`

| Path | Description | Parameters | Returns | Errors |
|------|-------------|------------|---------|--------|
| `/` | Get all profiles | None | Array<Profile> | None |
| `/get` | Get a profile by ID | `id` (uuid) | Profile | InvalidProfile |
| `/get` | Get a profile by username | `username` (string) | Profile | InvalidProfile |
| `/new` | Create a new profile | `username` (string) | Profile | InvalidUsername, UsernameAlreadyExists |

### Accounts

Object Namespace: `omnilink:accounts`

Rules:
1. Accounts are not created directly, they are created automatically when a profile is linked to a platform.

| Path | Description | Parameters | Returns | Errors |
|------|-------------|------------|---------|--------|
| `/` | Get all accounts | None | Array<Account> | None |
| `/get` | Get an account by ID | `id` (uuid) | Account | InvalidAccount |
| `/get` | Get an account by platform and platform ID | `platform` (string), `platform_id` (string) | Account | AccountNotFound |
| `/get` | Get all accounts for a profile | `profile_id` (uuid) | Array<Account> | InvalidProfile |

### Tokens

Object Namespace: `omnilink:tokens`

Rules:
1. Tokens are created by the OmniLink backend upon request from a platform integration service, and are used to verify that a profile is linked to a platform account.
2. Tokens are single-use, and expire after a configurable amount of time (default: 5 minutes).
3. The OmniLink backend will verify that a token is valid for a given profile and platform account, and will return an error if the token is invalid or expired. 

| Path | Description | Parameters | Returns | Errors |
|------|-------------|------------|---------|--------|
| `/get` | Get a token by token string | `token` (string) | Token | InvalidToken |
| `/get` | Get all tokens for a profile | `profile_id` (uuid) | Array<Token> | InvalidProfile |
| `/new` | Create a new verification token | `profile_id` (uuid), `target_platform` (string), `target_platform_id` (optional string) | Token | InvalidProfile |
| `/update` | Complete account linking | `token` (string), `target_platform` (string) , `target_platform_id` (required string) | Token | InvalidToken, ExpiredToken, IllegalMatch |

## Notifications

### Profiles

Object Namespace: `omnilink:notifications/profiles`

| Path | Description | Parameters |
|------|-------------|------------|
| `/new` | Notify that a new profile has been created | `profile` (Profile) |

### Accounts

Object Namespace: `omnilink:notifications/accounts`

| Path | Description | Parameters |
|------|-------------|------------|
| `/new` | Notify that a new account has been created | `account` (Account), `profile` (Profile) |

### Tokens

Object Namespace: `omnilink:notifications/tokens`

| Path | Description | Parameters |
|------|-------------|------------|
| `/new` | Notify that a new token has been created | `token` (Token), `profile` (Profile) |
| `/update` | Notify that a token has linked to an account | `token` (Token), `profile` (Profile), `account` (Account) |

## Schemas

### Profile

- `id` (uuid): The unique identifier for the profile.
- `username` (string): The username of the profile.
- `created_at` (number): The UNIX timestamp when the profile was created.

### Account

- `id` (uuid): The unique identifier for the account.
- `platform` (string): The platform of the account (e.g., "minecraft", "discord").
- `platform_id` (string): The unique identifier for the account on the platform.
- `platform_username` (string): The username of the account on the platform.

### Token

- `value` (string): Base32/Crockford encoded verification token, at 8 characters long in format `XXXX-XXXX`.
- `status` (string): The status of the token, either "pending", "used", or "expired".
- `created_at` (number): The UNIX timestamp when the token was created.
- `expires_at` (number): The UNIX timestamp when the token expires.
- `profile_id` (uuid): The unique identifier for the profile associated with the token.
- `profile_username` (string): The username of the profile associated with the token.
- `target_platform` (string): Name of a valid platform of the target account.
- `target_platform_id` (optional string): The unique identifier for the account on the target platform if known, otherwise null.

## Errors

Errors use standard JSON-RPC 2.0 error shapes, with custom error codes and messages.

### InvalidProfile

- Code: 100
- Message: "The specified profile does not exist."
- Data: { "profile_id": "<uuid>" }

### InvalidUsername

- Code: 101
- Message: "The specified username is invalid."
- Data: { "username": "<string>" }

### UsernameAlreadyExists

- Code: 102
- Message: "The specified username already exists."
- Data: { "username": "<string>" }

### InvalidAccount

- Code: 200
- Message: "The specified account does not exist."
- Data: { "account_id": "<uuid>" }

### AccountNotFound

- Code: 201
- Message: "The specified account does not exist for the given platform and platform ID."
- Data: { "platform": "<string>", "platform_id": "<string>" }

### InvalidToken

- Code: 300
- Message: "The specified token does not exist."
- Data: { "token": "<string>" }

### ExpiredToken

- Code: 301
- Message: "The specified token has expired."
- Data: { "token": "<string>" }

### IllegalMatch

- Code: 302
- Message: "The token does not match the given profile and platform account."
- Data: { "profile_id": "<uuid>", "target_platform": "<string>", "target_platform_id": "<string>" }
