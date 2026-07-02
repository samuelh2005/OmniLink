# Minecraft Provider

The Minecraft Provider allows users to link their Minecraft accounts to their OmniLink profiles.

It is implemented as a Minecraft server plugin that communicates with the OmniLink backend via the REST API.

## Commands

The Minecraft server plugin provides a minimal set of commands for account linking and management including:

1. `/link <token>` - Links the Minecraft account to the user's OmniLink profile using the provided token. The token is used a signing key for a JWT payload that contains the Minecraft account's UUID, this JWT would be sent to the OmniLink backend for verification and linking. The backend would perform all validation and linking logic, and return a response to the Minecraft server plugin indicating whether the linking was successful or not. The Minecraft server plugin would then inform the user of the result.
2. `/profile` - Tells the Minecraft user which profile they are currently linked to, if any. If the user is not linked to a profile, it will inform them that they are not linked.
