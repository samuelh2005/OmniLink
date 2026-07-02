# Minecraft Provider

The Minecraft Provider allows users to link their Minecraft accounts to their OmniLink profiles.

It is implemented as a Minecraft server plugin that communicates with the OmniLink backend via the REST API.

## Commands

The Minecraft server plugin provides a minimal set of commands for account linking and management including:

1. `/link <token>` - Links the Minecraft account to the user's OmniLink profile using the provided token.
2. `/profile` - Tells the Minecraft user which profile they are currently linked to, if any. If the user is not linked to a profile, it will inform them that they are not linked.

## Minecraft flow

1. The user initiates the account linking process by clicking a "Link Account" button on the Web Interface.
2. The Web Interface generates a unique token and displays it to the user.
3. The user enters the token in the Minecraft server using the `/link <token>` command.
4. The Minecraft server plugin sends signs a JWT payload with the token containing the Minecraft account's UUID with the token and sends it to the OmniLink backend for verification and linking.
5. The OmniLink backend verifies the token and links the Minecraft account to the user's OmniLink profile.
6. The OmniLink backend returns a response to the Minecraft server plugin indicating whether the linking was successful or not.
