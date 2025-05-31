# Real-Life Login System n8n Workflow

This workflow implements a real-world login and user management system using Telegram for authentication, Google Sheets for user data storage, and Redis for session management—all orchestrated via n8n.

## Overview

The workflow operates in several main parts:
1.  **Telegram Authentication:** Listens for messages from users via a Telegram bot and uses their chat ID as a unique identifier.
2.  **User Lookup & Registration:** Checks if the user exists in Google Sheets. If not, generates a new user ID and registers the user.
3.  **Session Management:** Uses Redis to cache user sessions for fast lookup and login state management.
4.  **User Data Management:** Stores and retrieves user details (name, Telegram chat ID, etc.) in Google Sheets.
5.  **Workflow Integration:** Supports triggering from other workflows and merging user/session data as needed.

## Features

*   Telegram-based login and authentication.
*   Google Sheets as a user database (stores user ID, name, Telegram chat ID, etc.).
*   Redis for fast session caching and lookup.
*   Automatic user registration for new users.
*   Easy integration with other n8n workflows.
*   Modular design for real-life login and session management scenarios.

## Prerequisites

1.  **Telegram Bot:**
    *   A Telegram bot and its API token, configured in n8n.
2.  **Google Sheets:**
    *   A Google Sheet with a `Users` sheet to store user data (UserId, Name, Telegram Chat Id, etc.).
    *   Google Sheets API credentials configured in n8n.
3.  **Redis:**
    *   Redis instance and credentials configured in n8n for session caching.

## Setup

1.  **Import Workflow:** Import the `Real_Life_Login_System.json` file into your n8n instance.
2.  **Create Local Redis Instance:**
    *   If you don't already have Redis running, you can start a local instance for development/testing:
        *   **On macOS (with Homebrew):**
            ```sh
            brew install redis
            brew services start redis
            ```
        *   **On Linux:**
            ```sh
            sudo apt-get install redis-server
            sudo service redis-server start
            ```
        *   **On Windows:**
            Download and run from https://github.com/microsoftarchive/redis/releases
    *   By default, Redis will run on `localhost:6379`.
3.  **Configure Credentials:**
    *   Add Telegram bot credentials in n8n and select them in the Telegram Trigger node.
    *   Add Google Sheets credentials for all Google Sheets nodes.
    *   Add Redis credentials for all Redis nodes (use `localhost:6379` for local development).
4.  **Prepare Google Sheets:**
    *   Ensure your sheet has columns for UserId, Name, Telegram Chat Id, etc., as referenced in the workflow.
5.  **Review Node Settings:**
    *   Double-check all node settings, especially document IDs, sheet names, and field mappings, to match your Google Sheets setup.
6.  **Activate Workflow:**
    *   Save and activate the workflow in n8n. The workflow will listen for Telegram messages and manage user sessions automatically.

## Usage

1.  **User Sends Message:** A user sends a message to your Telegram bot.
2.  **User Lookup:** The workflow checks if the user exists in Google Sheets.
3.  **Registration (if needed):** If the user is new, a user ID is generated and the user is registered in Google Sheets.
4. **Session Caching:** The user's session is cached in Redis for fast future lookups.
5.  **Integration:** The workflow can be triggered by other workflows to fetch or update user/session data as needed.

## Workflow Structure

*   **Telegram Trigger Path:** `Telegram Trigger` -> `Edit Fields` -> `Merge` -> `Find Cached User` -> `Is Cached` -> `Find User`/`UserId`/`Get UserId`/`Create User`/`Cache User`
*   **Google Sheets Integration:** Used for both user lookup and registration.
*   **Redis Integration:** Used for session caching and fast user lookup.
*   **Workflow Trigger:** Can be triggered by other workflows for modular integration.

## Cost Considerations (Potentially Free)

This workflow can be run essentially for free, depending on the usage volume and chosen services:

*   **n8n:** Can be self-hosted or used via n8n Cloud (free tier available).
*   **Google Sheets:** Free for moderate usage within Google account limits.
*   **Telegram:** Free for bots and messaging.
*   **Redis:** Free if self-hosted or using a free-tier cloud instance.

**Note:** Costs may be incurred if you exceed free tier limits of any service or use paid cloud Redis providers.

## Customization

*   **Changing Data Source:**
    *   Replace Google Sheets nodes with your preferred data source (e.g., Airtable, SQL) and update field mappings accordingly.
*   **Changing Session Store:**
    *   Swap out Redis nodes for your preferred session/cache provider.
*   **Expanding User Data:**
    *   Add more fields to the Google Sheet and update the workflow to handle additional user attributes.
*   **Integration:**
    *   Use the workflow as a module in larger automation systems for real-life login and session management.
