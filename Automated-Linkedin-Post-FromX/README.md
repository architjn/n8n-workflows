# Automated LinkedIn Post From X (Twitter) n8n Workflow

This workflow automates the process of scraping trending posts from X (Twitter), generating LinkedIn-ready content using AI, and posting or scheduling them to LinkedIn, with Google Sheets for data management and Telegram for notifications.

## Overview

The workflow operates in several main parts:
1.  **Scrape Trending X Posts:** On a schedule, fetches the latest posts from selected X (Twitter) accounts using RapidAPI.
2.  **Deduplication & Storage:** Cleans and deduplicates posts, storing new ones in a Google Sheet.
3.  **AI Post Generation:** Uses an AI agent to generate LinkedIn-ready posts from the scraped X content.
4.  **Approval & Scheduling:** Allows for manual or automated approval, then schedules posts for LinkedIn.
5.  **Posting & Notification:** Posts to LinkedIn (or prepares for posting), updates status in Google Sheets, and sends a Telegram notification when new posts are available.

## Features

*   Scrapes latest posts from X (Twitter) using RapidAPI.
*   Cleans, deduplicates, and stores posts in Google Sheets.
*   Uses AI to generate LinkedIn-optimized content from X posts.
*   Supports approval workflow and scheduled posting.
*   Notifies via Telegram when new posts are ready.
*   Tracks all post statuses and metadata in Google Sheets.

## Prerequisites

1.  **Google Sheets:**
    *   A Google Sheet to store scraped X posts and generated LinkedIn content.
    *   Google Sheets API credentials configured in n8n.
2.  **RapidAPI:**
    *   RapidAPI account and credentials for the X (Twitter) scraping API.
3.  **AI Model:**
    *   Access to an AI model (e.g., OpenAI, Anthropic, or similar) via n8n's AI nodes for post generation.
4.  **Telegram (optional):**
    *   Telegram bot credentials for notifications.
5.  **LinkedIn API (optional):**
    *   LinkedIn API credentials if you want to automate posting directly to LinkedIn (not included in this workflow by default, but can be added).

## Setup

1.  **Import Workflow:** Import the `Linkedin_Automation.json` file into your n8n instance.
2.  **Configure Credentials:**
    *   Add Google Sheets credentials in n8n and select them in all Google Sheets nodes.
    *   Add RapidAPI credentials for the X (Twitter) scraping node.
    *   Add AI model credentials for the AI Agent node.
    *   Add Telegram bot credentials for the Telegram node (optional).
3.  **Prepare Google Sheets:**
    *   Ensure your sheet has columns for post content, status, metadata, etc., as referenced in the workflow.
4.  **Review Node Settings:**
    *   Double-check all node settings, especially document IDs, sheet names, and field mappings, to match your Google Sheets setup.
5.  **Activate Workflow:**
    *   Save and activate the workflow in n8n. The workflow will run on the defined schedule and can also be triggered manually as needed.

## Usage

1.  **Scrape X Posts:** The workflow will automatically fetch and store new X posts on schedule.
2.  **Generate LinkedIn Content:** AI will generate LinkedIn-ready posts from the scraped X content.
3.  **Review & Approve:** Review generated posts in Google Sheets. Approve or edit as needed.
4.  **Schedule/Publish:** Approved posts are scheduled for posting to LinkedIn (if integrated) or can be posted manually.
5.  **Notifications:** Receive Telegram notifications when new posts are available for review or posting.

## Workflow Structure

*   **Scheduled Scraping Path:** `Schedule Trigger` -> `Get AI Tweets (RapidAPI)` -> `Clean Tweets` -> `Deduplication (Find Existing Tweet)` -> `Save New Tweet (Google Sheets)`
*   **AI Generation Path:** `Limit` -> `AI Agent (AI Model)` -> `Loop Posts` -> `Aggregate` -> `Telegram Notification`
*   **Approval & Posting Path:** `Google Sheets Trigger` -> `Filter Approved` -> `Max 20` -> `Get Post Timings` -> `Loop Approved Posts` -> `Update User/Wait for Posting` -> `Set Posting (Google Sheets)`
*   **Data Storage:** All input and output is managed via Google Sheets for easy access and review.

## Cost Considerations (Potentially Free)

This workflow can be run essentially for free, depending on the usage volume and chosen services:

*   **n8n:** Can be self-hosted or used via n8n Cloud (free tier available).
*   **Google Sheets:** Free for moderate usage within Google account limits.
*   **RapidAPI:** Free tier available, but scraping X (Twitter) may incur costs depending on volume.
*   **AI Model:** May incur costs depending on API/model usage; check your provider's pricing.
*   **Telegram:** Free for notifications.

**Note:** Costs may be incurred if you exceed free tier limits of any service or use paid AI models.

## Customization

*   **Changing Data Source:**
    *   Replace Google Sheets nodes with your preferred data source (e.g., Airtable) and update field mappings accordingly.
*   **Changing AI Models:**
    *   Swap out the AI Agent node for your preferred AI provider (e.g., OpenAI, Anthropic) and adjust prompts as needed.
*   **LinkedIn Integration:**
    *   Add LinkedIn API nodes to automate posting directly to LinkedIn.
*   **Adjusting Output:**
    *   Modify the output Google Sheet structure or add additional columns as required for your workflow.
