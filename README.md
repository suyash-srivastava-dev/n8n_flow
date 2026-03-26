# LetsRyde Training Portal & n8n Flow

This repository contains a full setup for the **LetsRyde Training Report Card** portal.

It features:
1. **Frontend (`index.html`)**: A beautiful web portal allowing users to check their progress using their Mobile Number, and Admins to edit and update records.
2. **Backend (`letsryde_flow.json`)**: An n8n workflow that handles API requests and syncs them directly with a Google Sheet database.

## 1. Google Sheets Setup
You have two options to set up the Google Sheet:

**Option A (Easy - Import CSV):**
1. Open Google Sheets and create a new blank spreadsheet.
2. Go to `File` > `Import` > `Upload` and select `letsryde_database_template.csv` from this folder.
3. Choose "Replace spreadsheet" and click **Import data**.

**Option B (Manual):**
Create a new Google Sheet. Rename the first sheet tab to `Sheet1`.
Create the following exact column headers in the first row (A1 to AC1):

- `Name`
- `Roll number`
- `Mobile number`
- `Email id`
- `Insta ID`
- `Profession`
- `Date Session 1`
- `Date Session 2`
- `Date Session 3`
- `Date Session 4`
- `Date of completion`
- `Practice Session 1`
- `Practice Session 2`
- `Practice Session 3`
- `Practice Session 4`
- `Trainer session 1`
- `Trainer session 2`
- `Trainer session 3`
- `Trainer session 4`
- `Weight height concept`
- `Zero Balance`
- `Round about bike balancing`
- `Walk with bike`
- `Penguin concept`
- `Center stand`
- `Pick the bike`
- `Details parts and control`
- `Gorilla Braking`
- `Safe Sound`

## 2. n8n Setup
1. Open your n8n instance.
2. Go to **Workflows** and click **Import from File**.
3. Select the `letsryde_flow.json` file provided in this folder.
4. Once imported, you will see two branches with Google Sheets nodes.
5. **Connect Credentials**: Double-click the Google Sheets node and connect your Google account.
6. **Set Document**: In the Google Sheets node, select your newly created Google Sheet.
7. **Ensure Webhooks point correctly**:
   - The first webhook listens on `GET /letsryde-user`
   - The second webhook listens on `POST /letsryde-update`
8. **Enable workflow**: Toggle the switch at the top right to make the workflow Active.

*(Note: Webhook calls require CORS. If you are running n8n locally without a tunnel, ensure your n8n environment variables allow CORS: `N8N_CORS_ALLOWED_ORIGINS=*`)*

## 3. Frontend Setup
1. Open `index.html` in your favorite code editor.
2. Around line 360 in the `<script>` section, locate the `N8N_BASE_URL` configuration:
   ```javascript
   const N8N_BASE_URL = localStorage.getItem('n8nBaseUrl') || 'http://localhost:5678/webhook';
   ```
   Modify `'http://localhost:5678/webhook'` to your active n8n webhook URL base (e.g. `https://your-n8n.com/webhook` or `https://your-n8n.com/webhook-test` for testing).
3. Open `index.html` in any web browser to start using the portal!

## 4. Usage
- **Admin**: Select "Admin" at login. Enter the password `admin123`. Search for a user's mobile number. Edit their progress and click **Save Changes**.
- **User**: Select "Trainee" at login. Enter their registered mobile number. They will see a read-only view of their progress identical to the Training Report Card style!
