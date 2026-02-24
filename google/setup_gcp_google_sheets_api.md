# GCP OAuth Setup with Google Sheets API — For LLM Agents
You are an autonomous browser-control agent. Your task is to navigate the Google Cloud Console (GCP) using direct URLs to create a new project, enable the Google Sheets API, configure the OAuth consent screen, and create a Desktop OAuth Client ID.

# Prerequisites
- The user is already logged into their Google account. If a login screen appears, pause and ask the user to log in manually, then resume.
- All GCP Console URLs use `hl=en` to force English UI.

# Important Notes
- **Project context:** After creating the project in Step 1, verify that the project is selected in the top navigation bar at the beginning of **every subsequent step**. If a different project is selected, switch to `Sheet Auto App`.
- **UI changes:** Google Cloud Console UI may change over time. If the exact buttons or layout do not match, adapt to achieve the same goal.

# Step-by-Step Execution Plan

## Step 1: Create a New Project
1. Navigate to: `https://console.cloud.google.com/projectcreate?hl=en`
2. **First-time GCP users:** If a Terms of Service agreement or welcome screen appears, agree to the terms and click **"Agree and Continue"**. Select the user's country if prompted.
3. Set the Project Name to `Sheet Auto App`.
   - If a project with this name already exists, use `Sheet Auto App 2` (increment as needed).
4. Click **"Create"** and wait for the notification confirming project creation.
5. Switch to the newly created `Sheet Auto App` project via the notification or the project selector. **Confirm it is the active project.**
6. Note down the **Project ID** (e.g., `sheet-auto-app-xxxxx`) — it will be needed in Step 6.

## Step 2: Enable Google Sheets API
1. **Verify** `Sheet Auto App` is the active project.
2. Navigate to: `https://console.cloud.google.com/apis/library/sheets.googleapis.com?hl=en`
3. Click the blue **"Enable"** button and wait for the API overview to load.
   - If it says **"Manage"** instead, the API is already enabled — proceed to Step 3.

## Step 3: Configure OAuth Consent Screen
1. **Verify** `Sheet Auto App` is the active project.
2. Navigate to: `https://console.cloud.google.com/apis/credentials/consent?hl=en`
3. The page will show **"Google Auth Platform not configured yet"** with a **"Get started"** button. Click **"Get started"**.
4. Set App name to `Sheet Auto App`. Click **"Next"**.
5. Select **"External"** as the user type. Click **"Next"**.
6. Enter the logged-in email as the contact email. Click **"Next"**.
7. Agree to the policy checkbox. Click **"Continue"** → **"Create"**.

## Step 4: Publish to Production
1. Navigate to: `https://console.cloud.google.com/auth/audience?hl=en`
2. **Verify** `Sheet Auto App` is the active project.
3. Check the **Publishing status** on this page.
   - If it shows **"Testing"**: Click **"Publish App"** → **"Confirm"**.
   - If it already shows **"In production"**: Proceed to Step 5.
4. Confirm the page now displays **"In production"**.

## Step 5: Create OAuth Client ID
1. **Verify** `Sheet Auto App` is the active project.
2. Navigate to: `https://console.cloud.google.com/apis/credentials/oauthclient?hl=en`
   - If redirected to the Google Auth Platform Clients page, click **"+ Create client"**.
3. Select **"Desktop app"** as the Application type.
4. Clear the Name field and set it to `RPA Client`, then click **"Create"**.
5. **CRITICAL — Extract the credentials from the modal:**
   A modal will appear showing the **Client ID** and **Client Secret**.
   - Copy the **Client ID** (e.g., `865774558105-xxxx.apps.googleusercontent.com`)
   - Copy the **Client Secret** (e.g., `GOCSPX-xxxx`)
   - ⚠️ **Do NOT click "Download JSON"** — file downloads may not work in AI browser environments.
   - ⚠️ **Do NOT close the modal** until both values are captured. The Client Secret cannot be viewed again after closing.
   - Only as a **last resort**, if the Client Secret is not visible in the modal, use **"+ Add secret"** on the client detail page.

## Step 6: Create credentials.json
Browser file downloads may not save to the user's local filesystem. **Create the file directly** using file system tools.

Create a file named `credentials.json` with this structure:

```json
{
  "installed": {
    "client_id": "<CLIENT_ID>",
    "project_id": "<PROJECT_ID>",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_secret": "<CLIENT_SECRET>",
    "redirect_uris": ["http://localhost"]
  }
}
```

Replace `<CLIENT_ID>`, `<PROJECT_ID>`, `<CLIENT_SECRET>` with the values from Steps 1 and 5.
Save to the user's preferred directory, or Desktop if not specified.

# Success Criteria
- Project `Sheet Auto App` exists and is active.
- Google Sheets API is enabled.
- OAuth consent screen is published to **Production**.
- OAuth Client ID `RPA Client` (Desktop app) exists.
- `credentials.json` is created with valid credentials.

**When all steps are complete, report to the user that the setup is finished and provide the full absolute path where `credentials.json` was saved.**

# Error Handling
- **403 / permission error**: Verify the correct project is selected.
- **"Enable" button not visible**: API is already enabled — look for "Manage".
- **Verification warning on consent screen**: Expected for production apps, proceed normally.
- **Missed the Client Secret**: Click **"+ Add secret"** on the client detail page. The new secret is only visible before page refresh.

---

# (Optional) Service Account Setup
> **Only execute this section if the user explicitly requests a service account.**
> Service accounts are used for server-to-server authentication without user interaction.

## Step A: Create a Service Account
1. **Verify** `Sheet Auto App` is the active project.
2. Navigate to: `https://console.cloud.google.com/iam-admin/serviceaccounts?hl=en`
3. Click **"+ Create Service Account"** at the top.
4. Set the **Service account name** to `rpa-service-account`.
   - The **Service account ID** will auto-fill (e.g., `rpa-service-account@sheet-auto-app-xxxxx.iam.gserviceaccount.com`). Note this email — it is needed to share Google Sheets with the service account.
5. Optionally add a description (e.g., `Service account for RPA automation`).
6. Click **"Create and Continue"**.
7. **(Optional) Grant roles:** If the service account needs access to other GCP resources, select a role (e.g., `Editor`). For Google Sheets API-only usage, this can be skipped.
8. Click **"Continue"** → **"Done"**.
9. Confirm the service account appears in the list.

## Step B: Create a Service Account Key (JSON)
The service account key file contains an RSA private key that cannot be easily captured from the browser screen. **Guide the user to download the key manually.**

1. **Verify** `Sheet Auto App` is the active project.
2. Open the service account's **Keys** page. Provide the user with this URL:
   `https://console.cloud.google.com/iam-admin/serviceaccounts/details/<SERVICE_ACCOUNT_UNIQUE_ID>/keys?project=<PROJECT_ID>&hl=en`
   - Replace `<SERVICE_ACCOUNT_UNIQUE_ID>` with the numeric ID (e.g., `116496770613376489058`).
   - Replace `<PROJECT_ID>` with the project ID from Step 1.
   - Alternatively, navigate to `https://console.cloud.google.com/iam-admin/serviceaccounts?hl=en`, click the service account email, then go to the **"Keys"** tab.
3. **Ask the user** to perform the following:
   1. Click **"Add Key"** → **"Create new key"**
   2. Select **JSON** as the Key type
   3. Click **"Create"**
   4. The key file will be auto-downloaded to their computer
4. Ask the user to provide the **downloaded file path** or place the file in the desired directory and rename it to `service-account-key.json`.

> ⚠️ **This key can only be downloaded once.** If the file is lost, delete the key and create a new one.

> **Tip:** To use the service account with Google Sheets, the target spreadsheet must be **shared** with the service account email (e.g., `rpa-service-account@sheet-auto-app-xxxxx.iam.gserviceaccount.com`) with **Editor** permission.

## Service Account — Additional Error Handling
- **Service account key download failed**: Delete the key from the Keys tab, then create a new key and try again.
- **"Permission denied" when using service account with Sheets**: Ensure the spreadsheet is shared with the service account email address.