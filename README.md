# AMD_AI_Scheduling_Assistant

Overview 

The AI Scheduling Agent is a Flask-based web application designed to automate meeting scheduling by parsing email content and checking attendee availability using the Google Calendar API. It integrates with an AI model (Qwen3-30B-A3B) to extract meeting details and processes attendees' schedules to find suitable time slots.
Features

Email Parsing: Extracts meeting details (summary, duration, date, and time preference) from email content using an AI model.
Google Calendar Integration: Fetches attendees' availability from their Google Calendar events.
Availability Checking: Computes available time slots based on attendees' calendar events and working hours.
REST API: Provides a /receive endpoint to process meeting requests via JSON payloads.
Timezone Support: Handles scheduling in the Asia/Kolkata timezone for accurate time management.

Prerequisites

Python 3.12.10
Google Cloud Project with Google Calendar API enabled
OAuth 2.0 credentials for Google Calendar API
Access to an AI model API (e.g., Qwen3-30B-A3B) running at http://localhost:8000/v1
Required Python packages:
flask
google-auth-oauthlib
google-api-python-client
pydantic-ai
pytz
asyncio



Installation

Clone the repository:git clone <repository-url>
cd ai-scheduling-agent


Install dependencies:pip install -r requirements.txt


Set up Google Calendar API:
Create a Google Cloud Project and enable the Google Calendar API.
Download OAuth 2.0 credentials and save them as .token files in the /home/user/Keys/ directory (e.g., userone.amd.token).
Ensure each .token file corresponds to an email address (e.g., userone.amd@gmail.com).


Configure environment variables:export BASE_URL="http://localhost:8000/v1"
export OPENAI_API_KEY="abc-123"


Run the Flask application:python Submission.ipynb



Usage

Start the Flask Server:

The application runs on http://0.0.0.0:5000 by default.
A background thread starts the Flask server when the notebook is executed.


Send a Meeting Request:

Use the /receive endpoint to send a JSON payload containing meeting details.
Example request:{
    "Request_id": "6118b54f-907b-4451-8d48-dd13d76033a5",
    "Datetime": "09-07-2025T12:34:55",
    "Location": "IIT Mumbai",
    "From": "userone.amd@gmail.com",
    "Attendees": [
        {"email": "usertwo.amd@gmail.com"},
        {"email": "userthree.amd@gmail.com"}
    ],
    "Subject": "Agentic AI Project Status Update",
    "EmailContent": "Hi team, let's meet on Thursday for 30 minutes to discuss the status of Agentic AI Project."
}


Send the request using a tool like curl or Postman:curl -X POST http://localhost:5000/receive -H "Content-Type: application/json" -d @request.json



Response:

The server returns a JSON response with the list of attendees and the parsed meeting details (e.g., meeting summary, duration, and date range).
Example response:[
    ["usertwo.amd@gmail.com", "userthree.amd@gmail.com"],
    {
        "meeting_summary": "discuss the status of Agentic AI Project",
        "duration_minutes": 30,
        "search_start_date": "2025-09-10",
        "search_end_date": "2025-09-10",
        "time_preference": null
    }
]



Project Structure

Submission.ipynb: Main Jupyter Notebook containing the application code.
/home/user/Keys/: Directory for storing Google Calendar API token files (e.g., userone.amd.token).
Flask routes:
/receive: POST endpoint to process meeting requests.


Key functions:
find_token_files_with_emails: Maps email addresses to token files.
get_user_availability: Retrieves available time slots for a user on a given date.
getAttendeesAvailableSlots: Aggregates availability for multiple attendees.
getTimeAndDateUsingEmail: Uses AI to parse email content and extract meeting details.

License
This project is licensed under the MIT License.
