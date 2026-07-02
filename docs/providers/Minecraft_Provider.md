# Minecraft Provider

The Minecraft Provider is responsible for directing the Minecraft user to the Web Interface by:

1. requesting a verification token from the OmniLink backend
2. providing the user with a link to the Web Interface that includes the verification token as a query parameter

It is implemented as a Minecraft server plugin that communicates with the OmniLink backend via the REST API.

## Commands

The Minecraft server plugin provides a minimal set of commands for account linking and management including:

1. `/link` - Requests a verification token from the OmniLink backend and provides the user with a link to the Web Interface.
2. `/profile` - Tells the Minecraft user which profile they are currently linked to, if any. If the user is not linked to a profile, it will inform them that they are not linked.
