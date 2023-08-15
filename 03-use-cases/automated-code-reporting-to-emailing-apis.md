Creating a complete API with Google Drive and Gmail integration involves multiple steps and components. I'll provide you with a high-level overview of the process and some code snippets to help you get started. Please note that this is a simplified example, and you might need to adapt and expand the code to meet your specific requirements.

**Step 1: Set Up Google Cloud APIs:**
1. Create a Google Cloud project and enable the Google Drive API and Gmail API.
2. Create credentials (OAuth 2.0 Client ID) to authenticate your application.

**Step 2: Google Drive Script:**
1. Write a Google Apps Script to generate the code report in Google Drive. This script could retrieve code information from a source (e.g., GitHub) and create a Google Docs document.

Here's a simplified example of a Google Apps Script that creates a new Google Docs document and adds content to it:

```javascript
function createCodeReport() {
  var doc = DocumentApp.create('Code Report');
  var body = doc.getBody();
  
  // Add content to the document
  body.appendParagraph('Code Report');
  // Add more content as needed
  
  doc.saveAndClose();
}
```

**Step 3: API Server:**
1. Set up a server using a web framework (e.g., Flask) in Python. This server will receive requests to trigger the creation of the code report and sending it via email.

**Step 4: Server Code:**
Here's a simplified example of how you might structure your Flask API to create the code report and send it via Gmail:

```python
from flask import Flask, request
from googleapiclient.discovery import build
from googleapiclient.http import MediaFileUpload
import base64
import os

app = Flask(__name__)

# Set up Google Drive API
drive_service = build('drive', 'v3', credentials=YOUR_DRIVE_CREDENTIALS)

# Set up Gmail API
gmail_service = build('gmail', 'v1', credentials=YOUR_GMAIL_CREDENTIALS)

@app.route('/create_code_report', methods=['POST'])
def create_and_send_report():
    # Call Google Apps Script to generate code report in Google Drive
    # Replace 'scriptId' with your actual script ID
    response = drive_service.scripts().run(scriptId='YOUR_SCRIPT_ID', body={}).execute()

    # Get the URL of the newly created Google Docs document
    doc_url = response['response']['result']

    # Send an email with the code report attachment
    message = create_message('sender@gmail.com', 'mouaazfarrukh99@gmail.com', 'Code Report', 'Please find the code report attached.', doc_url)
    send_message(gmail_service, 'me', message)

    return "Code report created and email sent."

if __name__ == '__main__':
    app.run(debug=True)
```

**Note:** The above code snippets are simplified and may require additional error handling, authentication, and configuration setup. You'll also need to fill in the actual credentials, script IDs, email addresses, and adjust the logic according to your requirements.

Remember that integrating Google APIs involves dealing with sensitive data and permissions. Make sure to handle authentication and authorization securely, and follow best practices for protecting user data.