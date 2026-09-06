# AI Email Organizer

An AI-powered email automation workflow built with **n8n, Gmail, Google
Gemini, Data Tables, Google Calendar, and Notion**. The system monitors
incoming emails, extracts their important information using AI, prevents
duplicate processing, and automatically creates tasks and calendar
events when action is required.

## 🚀 Project Overview

Managing emails manually can make it easy to miss deadlines, interviews,
assessments, meetings, and other important actions.

The **AI Email Organizer** automates this process by:

1.  Detecting new emails from Gmail.
2.  Retrieving the complete email content.
3.  Normalizing email data for consistent processing.
4.  Generating a unique duplicate key.
5.  Checking whether the email has already been processed.
6.  Using an AI model to classify and extract actionable information.
7.  Creating a **Notion task** when an action is required.
8.  Creating a **Google Calendar event** when an event is required.
9.  Storing processed-email information in a Data Table to avoid
    duplicate processing.

## ✨ Key Features

### 1. Gmail Automation

The workflow starts automatically when a new email arrives in Gmail.

### 2. AI-Powered Email Understanding

The AI analyzes the email and returns structured information such as:

-   Category
-   Priority
-   Action required
-   Summary
-   Task
-   Deadline
-   Event required
-   Event title
-   Event date
-   Event time
-   Company
-   Sender name

### 3. Email Normalization

Incoming email fields are cleaned and standardized before duplicate
detection. This helps ensure that variations in capitalization,
whitespace, and formatting do not create unnecessary duplicate records.

### 4. Duplicate Detection

A unique `duplicate_key` is generated using normalized email
information. The workflow checks the **Processed Emails** Data Table
before continuing.

This prevents the same email from being processed multiple times.

### 5. Automatic Task Creation

If the AI determines:

``` text
action_required = true
```

the workflow creates a task in the Notion task database.

The task can include:

-   Task name
-   Status
-   Priority
-   Category
-   Due date
-   Summary
-   Email sender
-   Email subject
-   Message ID

### 6. Automatic Calendar Events

If the AI determines:

``` text
event_required = true
```

the workflow creates a Google Calendar event using the extracted event
information.

### 7. Centralized Notion Dashboard

Tasks generated from emails are stored in Notion, providing a
centralized view of pending career, academic, personal, and other
activities.

## 🏗️ Workflow Architecture

``` text
Gmail Trigger
      ↓
Get a Message
      ↓
Normalize Email
      ↓
Generate Duplicate Key
      ↓
Check Processed Emails Data Table
      ↓
If Row Does Not Exist
      ↓
Insert Row
      ↓
AI Message Model
      ↓
Code in JavaScript
      ├──────────────→ Event If → Create an Event
      │
      └──────────────→ Task If → Create a Database Page
```

## 🧠 AI Output Structure

The AI produces structured JSON similar to:

``` json
{
  "category": "career",
  "priority": "high",
  "action_required": true,
  "summary": "Complete and submit the interview preparation document before the deadline.",
  "task": "Complete the interview preparation document and submit it.",
  "deadline": "2026-08-29",
  "event_required": false,
  "event_title": null,
  "event_date": null,
  "event_time": null,
  "company": null,
  "sender_name": "Lakshya Garg"
}
```

The structured output allows n8n conditions to decide which automation
path should run.

## 🔄 Decision Logic

### Task Path

The Task If node checks whether:

``` text
action_required = true
```

If true:

``` text
Task If → Create a database page
```

If false, no task is created.

### Event Path

The Event If node checks whether:

``` text
event_required = true
```

If true:

``` text
Event If → Create an event
```

If false, no calendar event is created.

## 🛡️ Duplicate Prevention

The project uses two important identifiers:

### Message ID

The Gmail message ID uniquely identifies the original email.

### Duplicate Key

The normalized duplicate key provides an additional layer of protection
against processing the same email content more than once.

Example:

``` text
lakshyagaarg@gmail.com|technical interview preparation task|hi lakshya...
```

The Data Table stores processed email information so future executions
can detect existing records.

## 🧪 Testing

The workflow was tested with different types of emails, including:

### No-action email

Example:

``` text
This is a test email.
No action is required.
```

Expected result:

``` text
action_required = false
event_required = false
```

No Notion task or Calendar event should be created.

### Task email

Example:

``` text
Please complete the technical interview preparation sheet
before August 29, 2026.
```

Expected result:

``` text
action_required = true
deadline = 2026-08-29
```

A Notion task should be created.

### Event email

Example:

``` text
Your final interview is scheduled for August 30, 2026 at 4:00 PM.
```

Expected result:

``` text
event_required = true
event_date = 2026-08-30
event_time = 16:00
```

A Google Calendar event should be created.

### Duplicate email

Sending or processing the same email again should cause the
duplicate-check stage to identify the existing record and prevent
another task/event from being created.

## 🧰 Technology Stack

  Technology        Purpose
  ----------------- -------------------------------------------------
  n8n               Workflow automation
  Gmail             Email trigger and message retrieval
  Google Gemini     AI-based email classification and extraction
  JavaScript        Data transformation and workflow logic
  n8n Data Table    Duplicate detection and processed-email storage
  Notion            Task management and dashboard
  Google Calendar   Automatic event creation

## 📊 Example Use Cases

The system can be used for:

-   Internship and job emails
-   Interview schedules
-   Online assessment deadlines
-   College assignments
-   Exam notifications
-   Meeting invitations
-   Application deadlines
-   Recruitment updates
-   Personal reminders
-   Important administrative emails

## 🌟 Why This Project Is Useful

Instead of manually reading every email and deciding what needs to be
done, the system converts unstructured email information into structured
actions.

For example:

``` text
Email
  ↓
AI understands the email
  ↓
Extract deadline / task / event
  ↓
Check for duplicates
  ↓
Create Notion task
  +
Create Calendar event
```

This reduces manual work, improves deadline visibility, and provides a
practical example of combining **Generative AI with workflow
automation**.

## 🔮 Future Improvements

Possible industry-level improvements include:

-   Better timezone handling
-   Support for recurring events
-   Attachment-aware AI extraction
-   Email importance scoring
-   Automatic reminders before deadlines
-   Confidence scores for AI predictions
-   Error-handling and retry workflows
-   Logging and monitoring
-   Multiple Notion databases
-   Calendar conflict detection
-   Support for more email providers
-   Human approval before creating high-impact actions
-   Evaluation datasets for measuring AI extraction accuracy

## 📁 Project Structure

``` text
AI-Email-Organizer/
│
├── n8n workflow
├── Gmail integration
├── AI email classifier
├── Email normalization
├── Duplicate detection
├── Data Table
├── Notion task database
└── Google Calendar integration
```

## 👨‍💻 Project Goal

The goal of the **AI Email Organizer** is to demonstrate how AI can be
integrated with workflow automation to transform incoming emails into
reliable, actionable tasks and calendar events while minimizing
duplicate processing.

------------------------------------------------------------------------

**Built with n8n + Gmail + Gemini + Notion + Google Calendar**
