# Competitor Analysis LinkedIn Post n8n Workflow

This workflow automates the process of analyzing competitor LinkedIn posts and generating high-performing, founder-style posts for your own DTC clothing brand, using AI and Google Sheets for data management.

## Overview

The workflow operates in three main parts:
1.  **Competitor Data Collection:** Triggered manually, it fetches a list of competitor LinkedIn URLs from Google Sheets, scrapes their recent posts using Apify, and stores the results in another Google Sheet.
2.  **Post Format Analysis:** Uses AI (Anthropic Claude) to analyze competitor posts, extracting popular hook types and post formats.
3.  **Post Generation:** AI generates new, high-performing LinkedIn posts for your brand, mimicking successful formats but tailored to a founder-led, authentic voice. The generated posts are appended to a Google Sheet for review and use.

## Features

*   Fetches competitor LinkedIn URLs from Google Sheets.
*   Scrapes competitor posts using Apify's LinkedIn Profile Posts API.
*   Analyzes post formats and hooks using Anthropic Claude via n8n's LangChain integration.
*   Generates 5 high-performing, founder-style LinkedIn posts for your brand, based on competitor insights.
*   Stores both the original competitor post and the generated post in Google Sheets for easy review and tracking.

## Prerequisites

1.  **Google Sheets:**
    *   A Google Sheet containing competitor LinkedIn URLs.
    *   A Google Sheet to store scraped posts and generated content.
    *   Google Sheets API credentials configured in n8n.
2.  **Apify:**
    *   An Apify account and API token for the LinkedIn Profile Posts actor.
3.  **Anthropic Claude (via LangChain):**
    *   Access to the Claude 3.7 Sonnet model (or similar) via n8n's LangChain nodes.
    *   API credentials configured in n8n.

## Setup

1.  **Import Workflow:** Import the `competitor-analysis-linkedin-post.json` file into your n8n instance.
2.  **Configure Credentials:**
    *   Add Google Sheets credentials in n8n and select them in all Google Sheets nodes.
    *   Add your Apify API token in the HTTP Request node for scraping LinkedIn posts.
    *   Add Anthropic Claude (or compatible) credentials for the LangChain nodes.
3.  **Prepare Google Sheets:**
    *   Ensure your input sheet contains a column for LinkedIn URLs.
    *   Set up output sheets for scraped posts and generated content as referenced in the workflow.
4.  **Review Node Settings:**
    *   Double-check all node settings, especially document IDs, sheet names, and field mappings, to match your Google Sheets setup.
5.  **Activate Workflow:**
    *   Save and activate the workflow in n8n. Trigger manually as needed to process new competitors or generate new posts.

## Usage

1.  **Populate Competitor List:** Add competitor LinkedIn URLs to your input Google Sheet.
2.  **Run Workflow:** Manually trigger the workflow in n8n. It will:
    *   Fetch competitor URLs from Google Sheets.
    *   Scrape recent posts using Apify.
    *   Store posts in Google Sheets.
    *   Analyze post formats and hooks using AI.
    *   Generate new, founder-style posts for your brand.
    *   Append both the original and generated posts to your output Google Sheet.
3.  **Review Output:** Check your output Google Sheet for both competitor post examples and your new, AI-generated posts.

## Workflow Structure

*   **Manual Trigger Path:** `Manual Trigger` -> `Google Sheets2 (Get Competitor List)` -> `Limit` -> `HTTP Request1 (Apify Scraper)` -> `Limit1` -> `Google Sheets3 (Store Posts)` -> `Analyze Posts - Anthropic` -> `Basic LLM Chain (Generate Posts)` -> `Google Sheets (Store Generated Posts)`
*   **AI Models:** Anthropic Claude 3.7 Sonnet (via LangChain) is used for both post analysis and generation.
*   **Data Storage:** All input and output is managed via Google Sheets for easy access and review.

## Cost Considerations (Potentially Free)

This workflow can be run essentially for free, depending on the usage volume and chosen services:

*   **n8n:** Can be self-hosted or used via n8n Cloud (free tier available).
*   **Google Sheets:** Free for moderate usage within Google account limits.
*   **Apify:** Free tier available, but scraping LinkedIn may incur costs depending on volume.
*   **Anthropic Claude:** May incur costs depending on API/model usage; check your provider's pricing.

**Note:** Costs may be incurred if you exceed free tier limits of any service or use paid AI models.

## Customization

*   **Changing Data Source:**
    *   Replace Google Sheets nodes with your preferred data source (e.g., Airtable) and update field mappings accordingly.
*   **Changing AI Models:**
    *   Swap out Anthropic Claude nodes for your preferred AI provider (e.g., OpenAI, Google Gemini) and adjust prompts as needed.
*   **Adjusting Output:**
    *   Modify the output Google Sheet structure or add additional columns as required for your workflow.
