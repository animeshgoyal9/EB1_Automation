# EB1 Visa Automation - Peer Review Outreach System

An automated email outreach system for applying to serve as a peer reviewer at academic journals. This project was created to support EB1 visa applications by demonstrating ongoing contributions to the academic community through peer review activities.

## 📋 Overview

This system automatically sends personalized peer review application emails to journal editors, tracking sent emails and managing the outreach process efficiently. It prevents duplicate sends, handles errors gracefully, and maintains a comprehensive log of all outreach activities.

### Key Features

- **Automated Email Dispatch**: Sends personalized emails to journal editors at controlled intervals
- **Duplicate Prevention**: Tracks sent emails to avoid re-contacting the same editor
- **Error Handling**: Robust error recovery with automatic retry mechanisms
- **CSV-Based Management**: Easy-to-update editor database in CSV format
- **Logging**: Comprehensive dispatch logging with timestamps and status tracking
- **macOS Integration**: LaunchAgent configuration for automatic background execution
- **UTF-8 Support**: Handles international characters in editor names and affiliations

## 🛠️ Technical Stack

- **Python 3.x**
- **pandas**: CSV data manipulation and tracking
- **smtplib**: Gmail SMTP integration
- **macOS LaunchAgent**: Background process management

## 📁 Project Structure

```
eb1_gemini/
├── auto_script.py.template      # Template script (copy to auto_script.py and configure)
├── additional_editors_200.csv   # Editor contact database (not in repo - create your own)
├── dispatch_log.csv             # Email dispatch log (auto-generated)
├── com.animesh.email_outreach.plist  # macOS LaunchAgent configuration
├── .gitignore                   # Protects sensitive files
└── README.md                    # This file
```

## 🚀 Setup Instructions

### 1. Prerequisites

- Python 3.x installed on macOS
- Gmail account with 2-Step Verification enabled
- pandas library installed: `pip install pandas`

### 2. Get Gmail App Password

1. Go to your [Google Account Security settings](https://myaccount.google.com/security)
2. Enable **2-Step Verification** if not already enabled
3. Navigate to **Security > 2-Step Verification > App passwords**
4. Generate a new app password for "Mail" on "Mac"
5. Copy the 16-character password (no spaces)

### 3. Configure the Script

1. Copy the template to create your working script:
   ```bash
   cp auto_script.py.template auto_script.py
   ```

2. Edit `auto_script.py` and update:
   ```python
   EMAIL_ADDRESS = "your_email@gmail.com"
   EMAIL_PASSWORD = "your_16_char_app_password"
   ```

3. Create your `additional_editors_200.csv` with columns:
   ```
   Name,Email,Journal,Expertise
   ```

### 4. Test the Script

Run manually first to verify configuration:
```bash
cd /Users/animeshgoyal/Downloads/eb1_gemini
python3 auto_script.py
```

The script will:
- Send 1 email every 2 minutes (120 seconds)
- Log all activities to `dispatch_log.csv`
- Skip previously contacted editors
- Handle errors and continue operation

### 5. Set Up Background Execution (Optional)

To run automatically on macOS using LaunchAgent:

1. Copy the plist file to LaunchAgents:
   ```bash
   cp com.animesh.email_outreach.plist ~/Library/LaunchAgents/
   ```

2. Update paths in the plist file if your installation directory is different

3. Load the LaunchAgent:
   ```bash
   launchctl load ~/Library/LaunchAgents/com.animesh.email_outreach.plist
   ```

4. To prevent sleep-related slowdowns, run with `caffeinate`:
   ```bash
   caffeinate -i python3 /Users/animeshgoyal/Downloads/eb1_gemini/auto_script.py
   ```

## ⚙️ Configuration Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `EMAILS_PER_BATCH` | 1 | Number of emails to send per batch |
| `HOURLY_INTERVAL` | 120 | Seconds to wait between batches |
| `EMAIL_ADDRESS` | (required) | Your Gmail address |
| `EMAIL_PASSWORD` | (required) | Your Gmail App Password |
| `EXCEL_FILE` | `additional_editors_200.csv` | Editor database file |
| `LOG_FILE` | `dispatch_log.csv` | Dispatch log file |

## 📊 Editor Database Format

The `additional_editors_200.csv` file should contain:

```csv
Name,Email,Journal,Expertise
John Doe,editor@journal.org,IEEE TPAMI,Computer Vision
Jane Smith,jsmith@university.edu,JMLR,Machine Learning
```

**Important**: This file contains personal contact information and should NOT be committed to version control.

## 📝 Email Template

The system sends personalized emails highlighting:
- Your academic background and qualifications
- Relevant research experience
- Industry expertise and impact metrics
- Commitment to rigorous peer review
- Journal-specific customization

Customize the email body in the `send_email()` function to match your credentials.

## 🔒 Security & Privacy

### Protected Files (Not in Git Repository)

- `auto_script.py` - Contains your Gmail App Password
- `additional_editors_200.csv` - Contains editor contact information
- `dispatch_log.csv` - Contains operational data and email addresses
- `*.log` files - Contain execution logs

### Best Practices

1. **Never commit credentials** - Use the template file for sharing
2. **Protect contact data** - Editor emails are private information
3. **Use App Passwords** - Never use your main Gmail password
4. **Rotate credentials** - Periodically regenerate App Passwords
5. **Review before push** - Always check `git status` before pushing

## 🐛 Troubleshooting

### Emails Slow When Screen Closes

macOS throttles background processes when the screen is off. Solutions:

1. **Use caffeinate** (recommended):
   ```bash
   caffeinate -i python3 auto_script.py
   ```

2. **Adjust Power Settings**:
   - System Preferences > Energy Saver
   - Disable "Put hard disks to sleep"
   - Disable "Enable Power Nap"

3. **Keep screen on** during operation

### SMTP Connection Errors

- Verify 2-Step Verification is enabled
- Regenerate App Password
- Check Gmail allows "Less secure apps" (though App Passwords are preferred)
- Verify internet connectivity

### CSV Parsing Errors

The script uses `on_bad_lines='skip'` to handle malformed CSV rows gracefully. Check the console output for skipped rows.

## 📈 Progress Tracking

Monitor progress via:

1. **Console output**: Real-time status updates
2. **dispatch_log.csv**: Complete history with timestamps
3. **Log files**: `launch_output.log` and `launch_error.log`

Example log entry:
```
2026-02-18 14:17:09,Future Internet Lead,fi-editor@mdpi.com,Future Internet,Sent
```

## 🎯 Use Case: EB1 Visa Support

This automation demonstrates:
- **Sustained contributions** to the academic community
- **Proactive engagement** with peer review processes
- **Systematic approach** to professional development
- **Documentation** of outreach activities for visa applications

The dispatch log serves as evidence of ongoing peer review applications, which can support EB1 visa petitions under the "extraordinary ability" category.

## ⚖️ Legal & Ethical Considerations

- **Compliance**: Ensure compliance with anti-spam laws (CAN-SPAM Act, GDPR)
- **Consent**: Only contact editors who publicly list their contact information
- **Opt-out**: Respect any opt-out requests immediately
- **Rate limiting**: The 2-minute interval prevents server overload
- **Personalization**: Each email is personalized, not mass spam

## 🔄 Maintenance

### Updating Editor Database

1. Export new contacts to CSV format
2. Ensure columns match: Name, Email, Journal, Expertise
3. Replace or append to `additional_editors_200.csv`
4. Script will automatically skip previously contacted editors

### Reviewing Logs

```bash
# View recent dispatch log entries
tail -n 20 dispatch_log.csv

# Count successful sends
grep -c "Sent" dispatch_log.csv

# Find errors
grep "Error" dispatch_log.csv
```

## 📜 License

This is personal automation software. Use responsibly and in compliance with all applicable laws and regulations.

## 🤝 Contributing

This is a personal project, but suggestions for improvements are welcome. Please note that any modifications should maintain:
- Privacy and security of contact information
- Compliance with email regulations
- Ethical outreach practices

## 📞 Support

For issues or questions:
- Review the troubleshooting section above
- Check macOS system logs: `Console.app`
- Verify Gmail account security settings

## 🗓️ Project Timeline

- **Created**: 2026-02
- **Purpose**: Automated peer review outreach for EB1 visa application support
- **Status**: Active and operational

---

**Note**: This README uses the template approach to protect sensitive credentials. Never commit the actual `auto_script.py` with real passwords to version control.
