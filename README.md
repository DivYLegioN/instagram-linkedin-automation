# Instagram-LinkedIn Automation

**Built for Global Trend Workflow Automation Internship Assignment**

Python automation that cross-posts content from Instagram to LinkedIn with automatic Google Sheets logging.

## 📋 Assignment Requirements

✅ Connect two social media accounts (Instagram → LinkedIn)
✅ Auto-post content when triggered from source platform  
✅ Log all cross-posts to Google Sheets with IDs, status, timestamps
✅ Clean Python codebase with environment variables
✅ README documentation

## 🏗️ Project Structure

```
instagram-linkedin-automation/
├── src/
│   ├── __init__.py
│   ├── config.py              # Environment variable configuration
│   ├── main.py                # Main automation workflow
│   ├── instagram_client.py    # Instagram Graph API integration
│   ├── linkedin_client.py     # LinkedIn UGC Posts API integration
│   ├── google_sheets_logger.py # Google Sheets logging
│   └── utils.py               # State management utilities
├── .env.example               # Environment variables template
├── .gitignore                # Python gitignore
├── requirements.txt          # Python dependencies
└── README.md                 # This file
```

## 🚀 Setup Instructions

### 1. Clone the Repository

```bash
git clone https://github.com/DivYLegioN/instagram-linkedin-automation.git
cd instagram-linkedin-automation
```

### 2. Create Virtual Environment

```bash
python -m venv venv

# On Windows:
venv\Scripts\activate

# On Mac/Linux:
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

1. Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```

2. Fill in your credentials in `.env`:

```ini
# Instagram API Credentials
INSTAGRAM_ACCESS_TOKEN=your_instagram_access_token_here
INSTAGRAM_BUSINESS_ACCOUNT_ID=your_instagram_business_account_id_here

# LinkedIn API Credentials  
LINKEDIN_ACCESS_TOKEN=your_linkedin_access_token_here
LINKEDIN_MEMBER_URN=urn:li:person:YOUR_ID_HERE
# OR for organization posting:
# LINKEDIN_ORG_URN=urn:li:organization:YOUR_ORG_ID

# Google Sheets Configuration
GOOGLE_SERVICE_ACCOUNT_JSON_PATH=path/to/service_account.json
GOOGLE_SHEET_ID=your_google_sheet_id_here

# Automation Configuration
SOURCE_PLATFORM=instagram
TARGET_PLATFORM=linkedin
```

### 5. Set Up Google Sheets

1. Create a new Google Sheet
2. Add header row: `timestamp | source_platform | source_post_id | target_platform | target_post_id | status | error_message`
3. Enable Google Sheets API in Google Cloud Console
4. Create a Service Account and download JSON key
5. Share your sheet with the service account email (found in JSON)
6. Copy Sheet ID from URL and add to `.env`

### 6. Get API Credentials

**Instagram:**
- Create Facebook Developer account
- Set up Instagram Business Account
- Create app and get Access Token + Account ID
- [Instagram Graph API Docs](https://developers.facebook.com/docs/instagram-api/)

**LinkedIn:**
- Create LinkedIn Developer account  
- Create app with required permissions (`w_member_social`)
- Generate Access Token
- [LinkedIn API Docs](https://learn.microsoft.com/en-us/linkedin/marketing/integrations/community-management/shares/ugc-post-api)

## 💻 Running the Automation

### Option A: Poll for Latest Posts (Automated)

```bash
python -m src.main
```

This will:
- Check Instagram for the latest image post
- Compare against last processed post (stored in `state.json`)
- If new post found, cross-post to LinkedIn
- Log results to Google Sheets

### Option B: Cross-Post Specific Post (Manual)

```python
from src.main import cross_post_instagram_to_linkedin

# Provide specific Instagram media ID
cross_post_instagram_to_linkedin(media_id="YOUR_MEDIA_ID_HERE")
```

### Scheduling (Production)

For production use, schedule with:

**Windows (Task Scheduler):**
```bash
schtasks /create /tn "Instagram-LinkedIn Automation" /tr "python -m src.main" /sc hourly
```

**Linux/Mac (cron):**
```bash
crontab -e
# Add: 0 * * * * cd /path/to/project && /path/to/venv/bin/python -m src.main
```

**Cloud (GitHub Actions / AWS Lambda / Google Cloud Functions)**

## 📊 Google Sheets Logging

Each cross-post creates a new row with:

| Column | Description |
|--------|-------------|
| timestamp | UTC timestamp of cross-post attempt |
| source_platform | Always "instagram" |
| source_post_id | Instagram media ID |
| target_platform | Always "linkedin" |
| target_post_id | LinkedIn post URN (if successful) |
| status | SUCCESS, FAILED, or ERROR |
| error_message | Error details (if any) |

## 🔧 Tech Stack

- **Language:** Python 3.8+
- **APIs:**
  - Instagram Graph API (v18.0)
  - LinkedIn UGC Posts API (v2)
  - Google Sheets API (v4)
- **Libraries:**
  - `requests` - HTTP requests to APIs
  - `python-dotenv` - Environment variable management
  - `gspread` - Google Sheets integration
  - `google-auth` - Google authentication

## 🎯 Features

✅ **Instagram Integration**
- Fetch latest image posts via Graph API
- Extract caption, media URL, timestamp
- State tracking to avoid duplicate posts

✅ **LinkedIn Integration**  
- Post text-only updates
- Post with image URLs (simplified flow)
- Supports both personal and organization posting

✅ **Google Sheets Logging**
- Automatic logging of all cross-posts
- Detailed error tracking
- Timestamp and status monitoring

✅ **Error Handling**
- Try-catch blocks for API failures
- Graceful error logging
- State preservation on errors

✅ **Extensible Design**
- Easy to add LinkedIn → Instagram
- Modular client architecture
- Configuration-driven platform selection

## 🔒 Security Notes

- **Never commit `.env` file** (already in `.gitignore`)
- **Rotate API tokens regularly**
- **Use service accounts** for Google Sheets (not personal credentials)
- **Set minimum required API permissions**
- **Store tokens securely** in production (AWS Secrets Manager, etc.)

## 📝 Assignment Deliverables

✅ **Code Repository:** [github.com/DivYLegioN/instagram-linkedin-automation](https://github.com/DivYLegioN/instagram-linkedin-automation)
✅ **Documentation:** This README with setup guide
✅ **Demo:** Screenshots/recording showing:
   - Instagram post
   - Automated LinkedIn cross-post  
   - Google Sheets log entry

## 🎬 Demo Flow

1. Post image on Instagram
2. Run `python -m src.main`
3. Automation detects new post
4. Creates LinkedIn post with same caption
5. Logs transaction to Google Sheets
6. Screenshot shows complete workflow

## 🚧 Future Enhancements

- [ ] Support video/reel cross-posting
- [ ] Support carousel posts
- [ ] Bidirectional sync (LinkedIn → Instagram)
- [ ] Web dashboard for monitoring
- [ ] Webhook-based triggers
- [ ] Content transformation rules
- [ ] Multi-account support

## 📄 License

MIT License - Feel free to use for learning and projects

## 👨‍💻 Author

**Your Name**  
Built for Global Trend Workflow Automation Internship Assignment

---

**Assignment Completed:** ✅  
**Submission Date:** December 2025
