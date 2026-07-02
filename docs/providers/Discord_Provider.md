# Discord Provider

The Discord Provider is responsible for directing the Discord user to the Web Interface by:

1. requesting a verification token from the OmniLink backend
2. providing the user with a link to the Web Interface that includes the verification token as a query parameter

It is implemented as a Discord bot that communicates with the OmniLink backend via the REST API.

## Commands

The Discord bot provides a minimal set of commands for account linking and management including:

1. `/link` - Requests a verification token from the OmniLink backend and provides the user with a link to the Web Interface.
2. `/profile` - Tells the Discord user which profile they are currently linked to, if any. If the user is not linked to a profile, it will inform them that they are not linked.
