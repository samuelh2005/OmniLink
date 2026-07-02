# OmniLink Architecture

OmniLink follows the principles of a microservices architecture, where each platform integration is implemented as a separate service. The OmniLink backend serves as the central hub for account linking and management, while the web frontend provides a user-friendly interface for users to link and manage their accounts.

```mermaid
flowchart LR
    A[Backend]

    B[Social Providers]
    C[Web Frontend]

    B ---|API| A
    C ---|API| A
```

## Rules

### Rule 1: Not an authentication provider

OmniLink is not an authentication provider, it does not manage sessions, and does not provide any authentication services. It is only responsible for linking accounts across platforms.

### Rule 2: Always use IDs, never usernames

Usernames are not unique and can change over time. Therefore, OmniLink always uses unique identifiers (IDs) to link accounts across platforms. For example, in Minecraft, the unique identifier is the UUID, while in Discord, it is the user ID.

## Models

### Profile

A profile is the canonical global representation of a user. It is the source of truth for all account relationships. A profile is created when a user first links an account, and it is never deleted. Profiles are identified by a UUID, and contain the following information:

- `id` PK (uuid): The unique identifier for the profile.
- `username` (string): The username of the user.
- `created_at` (timestamp): The timestamp when the profile was created.
- `updated_at` (timestamp): The timestamp when the profile was last updated.

### Account

An account is a representation of a user's identity on a specific platform. Each account is associated with a profile and contains the following information:

- `id` PK (uuid): The unique identifier for the account.
- `platform` (string): The name of the platform (e.g., "minecraft", "discord").
- `platform_id` (string): The unique identifier for the account on the platform (e.g., UUID for Minecraft, user ID for Discord).
- `profile_uuid` FK (uuid): The UUID of the profile associated with the account.
- `created_at` (timestamp): The timestamp when the account was created.
- `updated_at` (timestamp): The timestamp when the account was last updated.
