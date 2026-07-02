# Minecraft Provider

The Minecraft Provider allows users to link their Minecraft accounts to their OmniLink profiles.

It is implemented as a Minecraft server plugin that [implements the device flow](../oauth/integration.md#device-flow) for account linking. The plugin communicates with the OmniLink backend via the REST API.

## Commands

The Minecraft server plugin provides a minimal set of commands for account linking and management including:

1. `/link` - Initiates the device flow for account linking.
2. `/profile` - Tells the Minecraft user which profile they are currently linked to, if any. If the user is not linked to a profile, it will inform them that they are not linked.

## Minecraft flow

1. The user initiates the device flow by executing the `/link` command in the Minecraft server.
2. The Minecraft server plugin generates a unique device code and user code, and displays the user code to the user along with instructions to visit an OmniLink URL on a web browser.
3. The user visits the OmniLink URL on a web browser and enters the user code to authenticate and authorize the Minecraft server plugin to access their OmniLink profile.
4. OmniLink generates an access token and refresh token for the Minecraft server plugin, and returns them to the plugin along with an expiration time for the access token.
5. The Minecraft server plugin will use the access token to POST the Minecraft UUID to the OmniLink backend, which will link the Minecraft account to the user's OmniLink profile.
