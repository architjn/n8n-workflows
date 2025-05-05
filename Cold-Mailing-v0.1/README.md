# Cold Mailing n8n Workflow

This workflow automates the process of generating and sending personalized cold emails using AI, leveraging Airtable for contact management and status tracking.

## Overview

The workflow operates in two main parts:
1.  **Email Generation:** Triggered manually, it fetches a contact record marked as 'Pending' from Airtable, generates a personalized email body and subject line using AI (OpenRouter), and updates the Airtable record with the generated content and sets the status to 'Approval Pending'.
2.  **Email Sending:** Triggered on a schedule, it fetches contact records marked as 'Approved' from Airtable, updates their status to 'Sending', sends the email using the pre-generated content, and finally updates the status to 'Sent'.

## Features

*   Fetches contact data from an Airtable base.
*   Uses AI (via OpenRouter models) to generate personalized email copy based on contact details (company, name, industry, location, job title, etc.).
*   Generates catchy, professional subject lines using AI.
*   Integrates a manual approval step within Airtable ('Approval Pending' -> 'Approved').
*   Sends emails via SMTP on a defined schedule for approved contacts.
*   Tracks the status of each email ('Pending', 'Approval Pending', 'Approved', 'Sending', 'Sent') in Airtable.

## Prerequisites

1.  **Airtable Base:**
    *   An Airtable base and table configured to store contact information.
    *   Required fields (names may vary slightly based on your specific Airtable setup, check node configurations):
        *   `status`: Single select field with options: `Pending`, `Approval Pending`, `Approved`, `Sending`, `Sent`.
        *   `Email`: Email field for the recipient's address.
        *   `email_subject`: Text field to store the generated subject line.
        *   `email_copy`: Long text field to store the generated email body.
        *   Fields used for personalization in the AI prompts (e.g., `company_cleaned`, `firstname_cleaned`, `lastname_cleaned`, `industry`, `location`, `job`, `company_website`, `company_industry`, `company_location`, `company_employee_count`).
        *   `id`: The Airtable Record ID (used for matching during updates).
    *   Airtable API Credentials configured in n8n.
2.  **OpenRouter:**
    *   An OpenRouter account and API key.
    *   OpenRouter Credentials configured in n8n.
3.  **SMTP Server:**
    *   An SMTP service (e.g., Mailgun, SendGrid) configured for sending emails.
    *   SMTP Credentials configured in n8n.

## Setup

1.  **Import Workflow:** Import the `Cold_Mailing.json` file into your n8n instance.
2.  **Configure Credentials:**
    *   Navigate to the Credentials section in your n8n settings.
    *   Add new credentials for:
        *   **Airtable (API Token):** Use your Airtable Personal Access Token. Select this credential in all `Airtable` nodes (`Airtable1`, `Get Approved Message`, `Sending Status Change`, `Email Sent Status Change`, `Airtable2`).
        *   **OpenRouter:** Use your OpenRouter API key. Select this credential in the `OpenRouter Chat Model` nodes (`OpenRouter Chat Model`, `OpenRouter Chat Model1`).
        *   **SMTP:** Configure your SMTP provider details (host, port, user, password). Select this credential in the `Send Email` node.
3.  **Prepare Airtable:** Ensure your Airtable base is set up according to the details mentioned in the **Prerequisites** section. Update the Base ID and Table ID in the `Airtable` nodes within the workflow to match your specific Airtable setup if they differ from the defaults.
4.  **Review Node Settings:** Double-check the settings within each node, particularly the Airtable nodes (field mappings, filter formulas) and the `Send Email` node (From Email), to ensure they match your requirements.
5.  **Activate Workflow:** Save and activate the workflow in n8n. The scheduled part will start running based on the "Schedule Trigger" settings, and the manual part can be triggered as needed.

## Usage

1.  **Populate Airtable:** Add contact records to your Airtable table with their status initially set to `Pending`.
2.  **Generate Emails:** Manually trigger the workflow (using the "When clicking 'Test workflow'" node or by running the workflow manually). This will process one 'Pending' record, generate the email/subject, and update its status to `Approval Pending`. Repeat as needed for other pending contacts.
3.  **Approve Emails:** Review the `email_copy` and `email_subject` fields in Airtable for records with status `Approval Pending`. If satisfied, change the status to `Approved`.
4.  **Activate Schedule:** Ensure the "Schedule Trigger" node is active and configured to run at your desired frequency (e.g., every 15 minutes, daily).
5.  **Automatic Sending:** The scheduled workflow will automatically pick up `Approved` records, send the emails, and update their status to `Sent`.

## Workflow Structure

*   **Manual Trigger Path:** `Manual Trigger` -> `Airtable1 (Get Pending)` -> `AI Agent (Generate Body)` -> `Basic LLM Chain (Generate Subject)` -> `Airtable2 (Update to Approval Pending)`
*   **Scheduled Trigger Path:** `Schedule Trigger` -> `Get Approved Message` -> `Sending Status Change` -> `Send Email` -> `Email Sent Status Change`
*   **AI Models:** `OpenRouter Chat Model` and `OpenRouter Chat Model1` provide the language models for the AI Agent and LLM Chain respectively.

## Cost Considerations (Potentially Free)

This workflow can be run essentially for free, depending on the usage volume and chosen services:

*   **n8n:** The workflow platform itself can be self-hosted (requiring your own infrastructure costs) or used via n8n's cloud offering, which has a free tier suitable for many use cases.
*   **Airtable:** Airtable provides a generous free tier with limits on records per base and API calls per month. This workflow is likely to operate within these limits for moderate usage.
*   **AI Model (OpenRouter):** The workflow is currently configured to use a free model (`deepseek/deepseek-r1:free`) via OpenRouter. OpenRouter offers access to various models, including several free options. As long as you stick to free models, there's no cost here.
*   **SMTP Service:** Many email providers (like Mailgun, SendGrid, Brevo/Sendinblue) offer free tiers that allow sending hundreds or thousands of emails per month, which is often sufficient for cold outreach campaigns.

**Note:** Costs may be incurred if you exceed the free tier limits of Airtable or your SMTP provider, choose to use paid AI models via OpenRouter, or opt for a paid n8n cloud plan.

## Customization

*   **Changing Data Source (e.g., to Google Sheets):**
    *   Replace the `Airtable` nodes (`Airtable1`, `Get Approved Message`, `Sending Status Change`, `Email Sent Status Change`, `Airtable2`) with the corresponding nodes for your desired data source (e.g., `Google Sheets`).
    *   Update the data mapping and filtering logic within the new nodes to match the structure of your spreadsheet and the workflow's requirements (fetching pending/approved records, updating status, storing generated content).
    *   Ensure the expressions referencing data from the previous Airtable nodes (like `{{ $('Airtable1').item.json.id }}` or `{{ $json.fields.Email }}`) are updated to use the correct data structure from the new nodes.
*   **Changing AI Models:**
    *   Replace the `OpenRouter Chat Model` nodes (`OpenRouter Chat Model`, `OpenRouter Chat Model1`) with nodes for your preferred AI provider (e.g., `OpenAI Chat Model`, `Anthropic Chat Model`).
    *   Configure the new AI model nodes with your credentials and desired model parameters.
    *   Adjust the prompts in the `AI Agent` and `Basic LLM Chain` nodes if necessary to optimize for the new models.
