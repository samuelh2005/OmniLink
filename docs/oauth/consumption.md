# OAuth Consumption

OmniLink supports OAuth 2.0 for account linking with third-party services. This document outlines the steps involved in the OAuth flow for consuming OAuth tokens and linking user accounts.

## OAuth flow

1. The user initiates the OAuth flow by clicking a "Link Account" button on the Web Interface.
2. The Web Interface redirects the user to the OAuth Provider's authorization endpoint, passing along the necessary parameters (client ID, redirect URI, scopes, etc.).
3. The user authenticates with the OAuth Provider and grants permission to OmniLink to access their account information.
4. The OAuth Provider redirects the user back to the Web Interface with an authorization code.
5. The Web Interface exchanges the authorization code for an access token by making a request to the OAuth Provider's token endpoint.
6. The Web Interface uses the access token to retrieve the user's account information from the OAuth Provider's API.
7. The Web Interface sends the user's account information to the OmniLink backend, which updates the user's profile and links the account to their OmniLink profile.
