## Bucket Management Module

### Overview
Bucket keeps related data or files together in an organized way for dealer users. This interactive module lets users configure buckets, connect to data sources, upload files, and map data fields for downstream processing.

### Goals
- Create and manage buckets
- Connect a data source (File Upload or API)
- Map uploaded data to system fields
- View and edit file structure over time (diff old vs new)

### 1. Bucket Configuration Interface
Users can:
- Create a new bucket by providing a name
- Connect a data source
- Save bucket settings

#### Fields & Validation
- Bucket Name
  - Required
  - Minimum length: 8 characters
- Data Source (required): choose one
  - File Upload
  - API Connection

#### Actions
- Save: validate inputs, create bucket; on success redirect to Field Mapping interface
- Cancel: discard changes and return to bucket list

### 2. Data Source Options
#### Option 1: File Upload
- Allowed formats: CSV, XLSX, JSON
- Max file size: 20 MB
- File preview before saving
  - CSV: show the first N rows
  - XLSX: list sheet names and show the first N rows per selected sheet
  - JSON: preview first N records or top-level keys

#### Option 2: API Connection
- Endpoint URL (required)
- Authentication
  - API Key: header name and value
  - OAuth 2.0: client credentials and token URL (scopes as needed)
- Test Connection
  - Validate connectivity and credentials
  - Fetch and display a sample payload/schema when possible
- Store credentials securely (server-side vault/secrets)

- On successful bucket creation, redirect to the Field Mapping interface.

### 3. Field Mapping Interface
Shown after a successful upload or a successful API schema fetch.

#### Context
- Sheets
  - Read-only list of sheet names detected from the uploaded file (one or more may be present)
  - Example: "Sheet1", "Sheet2", "Summary"

#### Mappings
- Model (Dropdown): single select from available model list
- Branch (Dropdown): single select from available branches
- Consultant (Dropdown): single select from available consultants
- Date (Multi-Select Dropdown): select one or more columns for date fields (e.g., "Booking Date", "Delivery Date")

Notes:
- Dropdown options are populated from the uploaded file's column headers (per sheet, when applicable)
- Buttons: Cancel, Save

On Save:
- Persist mapping configuration to the database (associated with the bucket and sheet where applicable)
- Provide an option to reuse this mapping for future uploads to the same bucket

### 4. File Structure Management (After Bucket Creation)
Allows users to view and edit the bucket's file structure as the source format evolves.

#### View File Structure
1. Open the bucket from the bucket list
2. Click "View Structure"
3. Display:
   - Current bucket headers
   - Current sheets linked to the bucket
4. Close to exit, or click "Edit Structure" to make changes

#### Edit File Structure
1. Click "Edit Structure"
2. Upload the new file with the updated structure
3. System compares old vs new and shows:
   - Added headers
   - Removed headers
   - Added sheets
   - Removed sheets
4. For each change:
   - Map new headers to existing fields or create new ones
   - Re-map or remove sheets as required
5. Adjust field data types or formats if needed
6. Preview a sample with the updated mapping
7. Save: system updates structure without losing old data

### Example Scenario
- Old file: 5 headers, 2 sheets
- New file: 6 headers, 3 sheets
- Action:
  - View differences
  - Map the extra header to a new field
  - Link the extra sheet to the bucket of files
  - Save and confirm changes