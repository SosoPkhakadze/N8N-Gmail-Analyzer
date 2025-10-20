# N8N-Gmail-Analyzer

# Intelligent Gmail Organizer 🧠 automated-gmail-sorter

This n8n workflow automates the process of sorting, categorizing, and archiving Gmail messages using a powerful hybrid approach that combines rule-based logic with AI-powered classification.

The system first attempts to categorize emails with high confidence using predefined rules. If an email doesn't match any rule, it is passed to an OpenAI model (GPT-4o-mini) for intelligent classification. The AI can even create new labels in Gmail on the fly if it identifies a new, relevant category.

![Workflow Diagram]
<img width="914" height="270" alt="Gmail Analyzer" src="https://github.com/user-attachments/assets/f6bfefe9-7a19-4025-8b12-ed290621d0a7" />

---

## Key Features

- **Hybrid Classification:** Uses a fast, rule-based engine for common emails (promotions, job alerts, system notifications) and leverages AI for ambiguous ones.
- **Dynamic Label Creation:** If the AI suggests a category that doesn't exist as a Gmail label, the workflow automatically creates it.
- **Intelligent Content Pre-processing:**
  - Cleans and summarizes long promotional emails.
  - Extracts meaningful URLs and filters out tracking links.
  - Normalizes email content for more accurate analysis.
- **Fully Automated:** Runs on a daily schedule to keep your inbox organized without any manual intervention.
- **Self-Improving:** The system adapts over time as the AI creates new, more specific labels for your emails.

## How It Works

The workflow follows a logical sequence to process incoming mail:

1.  **Schedule Trigger:** Runs automatically once a day.
2.  **Fetch Emails:** Retrieves all emails from the last 24 hours.
3.  **Pre-process & Clean:** Each email's content is cleaned, summarized, and important URLs are extracted.
4.  **Rule-Based Engine:** The system checks the email against a series of high-confidence rules (e.g., sender domain, subject keywords).
5.  **Decision Point:**
    - **If a rule matches:** The email is immediately labeled and archived.
    - **If no rule matches:** The email is sent to the AI for classification.
6.  **AI Classification Path:**
    - The workflow fetches all existing Gmail labels.
    - It generates a detailed prompt for the OpenAI API, including the email content and the list of available labels.
    - The AI returns the best-fit label name (either existing or a new suggestion).
    - The workflow checks if the suggested label exists. If not, it creates it.
    - Finally, the correct label is applied, and the email is archived.

## Technology Stack

- **Automation:** [n8n.io](https://n8n.io/)
- **Core Integration:** Gmail API, OpenAI API (GPT-4o-mini)
- **Scripting:** JavaScript (within n8n Code Nodes)

## Setup & Configuration

To use this workflow in your own n8n instance:

1.  **Import the Workflow:** Copy the contents of the `Analyze_Gmail.json` file and import it into your n8n canvas.
2.  **Configure Credentials:**
    - You will need to create and configure credentials for:
      - **Gmail (OAuth2):** Connect your Gmail account.
      - **OpenAI:** Add your API key.
    - Assign these credentials to the respective `Gmail` and `OpenAI` nodes in the workflow.
3.  **Personalize the Rules (Optional):**
    - Open the **"Determine Message"** Code node.
    - Modify the `personalContacts`, `marketingDomains`, and other arrays to match your specific needs.

## A Note on Security

The provided `.json` file has had all credential and webhook information removed. **Never commit your personal credentials or API keys to a public repository.** This workflow is designed to work with credentials stored securely within your n8n instance.
