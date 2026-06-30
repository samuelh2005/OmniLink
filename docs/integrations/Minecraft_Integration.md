# Minecraft Integration

The Minecraft integration is designed to be the primary initiating platform for account linking. It is responsible for generating verification tokens.

This integration is implemented as a Minecraft server plugin using the shared Java library, which allows it to communicate with the DB and message bus.

## Commands

The Minecraft plugin provides the full suite of commands for account linking and management. The following commands are available:

1. `/profile link <platform> <username>` - Generates a verification token for the specified platform and username.
2. `/profile link <platform>` - Generates a verification token for the specified platform. The username is not provided if we cannot compute the platform username from the request.
3. `/profile list-accounts` - Lists all accounts linked to the user's profile.
4. `/profile unlink <platform> <username>` - Unlinks the specified account from the user's profile.
5. `/verify <token>` - Proves ownership of the account on the specified platform by verifying the provided token. It must ensure that the token is valid and has not expired, and that the account on the specified platform matches the account associated with the token.
