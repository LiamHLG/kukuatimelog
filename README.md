# Employee Time Clock System 📱⏱️

A modern, QR code-enabled employee time tracking system that records clock in/out times directly to Google Sheets.

## ✨ Features

- **QR Code Access** - Employees scan a QR code to quickly access the time clock
- **Google Sheets Integration** - All records automatically saved with timestamps
- **Mobile-Friendly** - Works perfectly on smartphones, tablets, and computers
- **Real-Time Clock** - Shows current time and date
- **Automatic Timestamps** - Records exact date and time for each entry
- **No Account Required** - Employees just enter their name and clock in/out
- **Free to Use** - Uses free Google Sheets for storage
- **Professional Design** - Modern, clean interface

## 📁 Files Included

1. **employee-timeclock.html** - Main time clock page (employees use this)
2. **timeclock-setup.html** - Admin setup page with QR code generator
3. **README.md** - This file with instructions

## 🚀 Quick Start Setup

### Step 1: Set Up Google Sheets (5 minutes)

1. Go to [Google Sheets](https://sheets.google.com) and create a new spreadsheet
2. Name it "Employee Time Records" (or whatever you prefer)
3. Add these headers in the first row:
   - A1: `Timestamp`
   - B1: `Employee Name`
   - C1: `Action`
   - D1: `Date`
   - E1: `Time`

4. Click **Extensions → Apps Script**
5. Delete any existing code and paste this:

```javascript
function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    const data = JSON.parse(e.postData.contents);
    
    const timestamp = new Date(data.timestamp);
    const date = Utilities.formatDate(timestamp, Session.getScriptTimeZone(), "yyyy-MM-dd");
    const time = Utilities.formatDate(timestamp, Session.getScriptTimeZone(), "HH:mm:ss");
    
    sheet.appendRow([
      timestamp,
      data.name,
      data.action.toUpperCase(),
      date,
      time
    ]);
    
    return ContentService.createTextOutput(JSON.stringify({
      'status': 'success',
      'message': 'Time logged successfully'
    })).setMimeType(ContentService.MimeType.JSON);
    
  } catch(error) {
    return ContentService.createTextOutput(JSON.stringify({
      'status': 'error',
      'message': error.toString()
    })).setMimeType(ContentService.MimeType.JSON);
  }
}
```

6. Click **Deploy → New deployment**
7. Click the gear icon ⚙️ next to "Select type" and choose **Web app**
8. Configure:
   - Execute as: **Me**
   - Who has access: **Anyone**
9. Click **Deploy**
10. Authorize the app (you may see a security warning - click Advanced → Go to [Project name])
11. **COPY THE WEB APP URL** - you'll need this! It looks like:
    `https://script.google.com/macros/s/ABC123XYZ.../exec`

### Step 2: Configure the Time Clock

1. Open `employee-timeclock.html` in any text editor
2. Find line 245 (around there):
   ```javascript
   const SHEETS_URL = 'YOUR_GOOGLE_SHEETS_WEB_APP_URL_HERE';
   ```
3. Replace `YOUR_GOOGLE_SHEETS_WEB_APP_URL_HERE` with your Web app URL from Step 1
4. Keep the single quotes! Example:
   ```javascript
   const SHEETS_URL = 'https://script.google.com/macros/s/ABC123XYZ.../exec';
   ```
5. Save the file

### Step 3: Choose Your Deployment Method

#### Option A: Free Web Hosting (Recommended for QR codes)

**Netlify (Easiest):**
1. Go to [Netlify Drop](https://app.netlify.com/drop)
2. Drag and drop your `employee-timeclock.html` file
3. Netlify gives you a free URL like `yourname.netlify.app`
4. Done! Use this URL for your QR code

**GitHub Pages (Free):**
1. Create a GitHub account if you don't have one
2. Create a new repository
3. Upload `employee-timeclock.html` (rename it to `index.html`)
4. Enable GitHub Pages in repository settings
5. Get your URL: `yourusername.github.io/repository-name`

**Vercel (Free):**
1. Sign up at [Vercel](https://vercel.com)
2. Import your project
3. Deploy and get your free URL

#### Option B: Local Device (Tablet/Computer)

1. Save `employee-timeclock.html` on a device at your workplace
2. Double-click to open it in a web browser
3. Bookmark it for easy access
4. Employees use this device to clock in/out
5. (QR code won't work with local files, but bookmarking is just as easy)

#### Option C: Your Own Website

1. Upload `employee-timeclock.html` to your web hosting
2. Place it wherever you want (e.g., `yourdomain.com/timeclock`)

### Step 4: Generate QR Code

1. Open `timeclock-setup.html` in a web browser
2. Enter the URL where you hosted the time clock page
3. Click "Generate QR Code"
4. Download the QR code image
5. Print it and post it at your workplace!

## 📱 How Employees Use It

1. Scan the QR code with their smartphone camera
2. The time clock page opens in their browser
3. They enter their full name
4. Click **Clock In** when arriving or **Clock Out** when leaving
5. They'll see a confirmation message
6. Done! Their time is recorded

## 📊 Viewing Your Records

1. Open your Google Sheet
2. All clock in/out records appear automatically with:
   - Full timestamp
   - Employee name
   - Action (IN or OUT)
   - Date
   - Time
3. Sort, filter, and analyze as needed
4. Export to Excel anytime (File → Download → Microsoft Excel)

## 🔧 Troubleshooting

### "Google Sheets not configured" Error
- Make sure you updated the `SHEETS_URL` in the HTML file with your actual Google Apps Script URL
- Check that the URL is in single quotes
- Verify the URL is the deployment URL (ends with `/exec`)

### Records Not Appearing in Google Sheet
- Check that you deployed the Apps Script as a "Web app"
- Make sure "Who has access" is set to "Anyone"
- Try redeploying the Apps Script (Deploy → Manage deployments → Edit → Deploy)

### QR Code Not Generating
- Make sure you entered a valid URL (must start with `http://` or `https://`)
- Try refreshing the setup page

### Clock In/Out Buttons Not Working
- Check your browser console for errors (F12 → Console tab)
- Verify your internet connection
- Make sure the Google Sheets URL is correctly configured

## 💡 Tips

- **Weekly Reports**: Sort your Google Sheet by date to generate weekly reports
- **Export Data**: Use File → Download → CSV to export data for payroll systems
- **Multiple Locations**: Create different sheets for different locations or departments
- **Backup**: Google Sheets automatically backs up your data
- **Privacy**: Only you can access the Google Sheet with the records
- **Editing Records**: You can manually edit any entries in the Google Sheet if needed

## 🎨 Customization

Want to customize the look? You can edit the HTML files:
- Change colors in the CSS `:root` section
- Modify the logo/title
- Add your company logo
- Change fonts
- Add additional fields (you'll need to update both the HTML and the Google Apps Script)

## 📄 Data Format in Google Sheet

| Timestamp | Employee Name | Action | Date | Time |
|-----------|---------------|--------|------|------|
| 2026-02-06 09:00:00 | John Smith | IN | 2026-02-06 | 09:00:00 |
| 2026-02-06 17:30:00 | John Smith | OUT | 2026-02-06 | 17:30:00 |
| 2026-02-06 09:15:00 | Jane Doe | IN | 2026-02-06 | 09:15:00 |

## 🔒 Security Notes

- The Google Apps Script URL is public but doesn't expose your data
- Only you can access the actual Google Sheet
- No passwords or sensitive data are stored
- Employees only need to enter their name
- Consider adding employee ID verification if needed

## 📞 Support

If you need help:
1. Check the troubleshooting section above
2. Review the setup steps in `timeclock-setup.html`
3. Verify all URLs and configurations are correct

## 📜 License

Free to use for personal and commercial purposes. No attribution required.

---

**Built with ❤️ for small businesses who need a simple, effective time tracking solution.**
