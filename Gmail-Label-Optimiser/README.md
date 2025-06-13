# Gmail Label Optimiser n8n Workflow

This workflow automates the process of classifying and organizing your Gmail inbox by intelligently labeling emails using AI, making it easier to manage and prioritize your messages.

## Overview

The workflow operates in several key steps:
1.  **Email Trigger:** Automatically triggers on new emails received in your Gmail inbox.
2.  **AI Classification:** Uses an AI text classifier to analyze the content and subject of each email, categorizing it into predefined types (e.g., Marketing, Stock Updates, Newsletter, Security Alert, etc.).
3.  **Label Management:** Checks if the appropriate label exists in Gmail; if not, it creates the label.
4.  **Label Application:** Applies the correct label to the email thread, organizing your inbox in real time.

## Features

*   Automatically classifies incoming emails into categories such as Marketing, Stock Updates, Newsletter, Security Alert, Payment Receipt, and more.
*   Uses AI (OpenRouter GPT-4.1-nano or compatible) for accurate email content classification.
*   Creates new Gmail labels on the fly if they do not already exist.
*   Applies the correct label to each email thread for streamlined inbox management.
*   Fully automated and runs continuously in the background.

## Prerequisites

1.  **Gmail Account:**
    *   Gmail account with OAuth2 credentials configured in n8n.
2.  **n8n Instance:**
    *   Self-hosted or n8n Cloud account.
3.  **AI Provider:**
    *   OpenRouter (or compatible) API credentials for the AI classification node.

## Setup

1.  **Import Workflow:** Import the `Gmail_Optimiser.json` file into your n8n instance.
2.  **Configure Credentials:**
    *   Add Gmail OAuth2 credentials in n8n and select them in all Gmail nodes.
    *   Add your OpenRouter (or compatible) API credentials for the AI classification node.
3.  **Review Node Settings:**
    *   Double-check all node settings, especially credentials and field mappings, to match your Gmail and AI provider setup.
4.  **Activate Workflow:**
    *   Save and activate the workflow in n8n. It will run automatically on new incoming emails.

## Usage

1.  **Enable Workflow:** Ensure the workflow is active in n8n.
2.  **Automatic Operation:**
    *   The workflow will trigger on every new email received.
    *   Each email is analyzed and classified by the AI model.
    *   The appropriate label is created (if needed) and applied to the email thread.
3.  **Inbox Management:**
    *   Check your Gmail inbox for automatically organized and labeled emails.

## Workflow Structure

*   **Trigger Path:** `Gmail Trigger` -> `Text Classifier (AI)` -> `Set Category Nodes` -> `Merge` -> `Gmail (Get/Create Label)` -> `Filter` -> `Switch` -> `Create Label (if needed)` -> `Apply Label`
*   **AI Model:** OpenRouter GPT-4.1-nano (or compatible) is used for email classification.
*   **Label Management:** Labels are created and applied dynamically based on classification results.

## Cost Considerations (Potentially Free)

This workflow can be run essentially for free, depending on the usage volume and chosen services:

*   **n8n:** Can be self-hosted or used via n8n Cloud (free tier available). If you're new to n8n, you can check it out here: [n8n (affiliate)](https://architjn.com/r/n8n)
*   **Gmail:** Free for personal use within Google account limits.
*   **OpenRouter/AI Provider:** May incur costs depending on API/model usage; check your provider's pricing.

**Note:** Costs may be incurred if you exceed free tier limits of any service or use paid AI models.

## Customization

*   **Changing Categories:**
    *   Edit the categories in the AI classification node to match your personal or business needs.
*   **Changing AI Models:**
    *   Swap out the OpenRouter node for your preferred AI provider and adjust prompts as needed.
*   **Adjusting Label Logic:**
    *   Modify the workflow to add, remove, or change label application logic as required for your workflow.

## License

This project is licensed under the MIT License - see the [LICENSE](../LICENSE) file for details.
