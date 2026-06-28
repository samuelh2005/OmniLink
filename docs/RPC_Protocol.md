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

| Path | Description | Parameters | Returns |
|------|-------------|------------|---------|
| `/` | Get all profiles | None | Array<Profile> |
| `/get` | Get a profile by ID | `id` (uuid) | Profile |
| `/get` | Get a profile by username | `username` (string) | Profile |
| `/new` | Create a new profile | `username` (string) | Profile |

### Accounts

Object Namespace: `omnilink:accounts`

Rules:
1. Accounts are not created directly, they are created automatically when a profile is linked to a platform.

| Path | Description | Parameters | Returns |
|------|-------------|------------|---------|
| `/` | Get all accounts | None | Array<Account> |
| `/get` | Get an account by ID | `id` (uuid) | Account |
| `/get` | Get an account by platform and platform ID | `platform` (string), `platform_id` (string) | Account |
| `/get` | Get all accounts for a profile | `profile_id` (uuid) | Array<Account> |

### Tokens

Object Namespace: `omnilink:tokens`

| Path | Description | Parameters | Returns |
|------|-------------|------------|---------|
| `/get` | Get a token by token string | `token` (string) | Token |
| `/get` | Get all tokens for a profile | `profile_id` (uuid) | Array<Token> |
| `/new` | Create a new verification token | `profile_id` (uuid), `target_platform` (string), `target_platform_id` (optional string) | Token |
| `/update` | Complete account linking | `token` (string), `target_platform` (string) , `target_platform_id` (required string) | Token |

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
