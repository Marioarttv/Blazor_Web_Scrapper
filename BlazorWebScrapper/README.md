# Blazor LSF Web Scraper - Setup Tutorial

A Blazor Server application for scraping course and student data from German university LSF (Lehre, Studium, Forschung) systems.

## Prerequisites

### Required Software

1. **Microsoft .NET 9.0 SDK**
   - Download from: https://dotnet.microsoft.com/download/dotnet/9.0
   - Choose ".NET 9.0 SDK" (not just Runtime)
   - Verify installation: Open Command Prompt/Terminal and run `dotnet --version`
   - Should show version 9.0.x

2. **Git** (to clone the repository)
   - Download from: https://git-scm.com/downloads
   - Or use GitHub Desktop: https://desktop.github.com/

3. **Code Editor** (choose one):
   - **Visual Studio 2022** (Recommended for Windows)
     - Download Community edition (free): https://visualstudio.microsoft.com/vs/community/
     - During installation, select "ASP.NET and web development" workload
   - **Visual Studio Code** (Cross-platform)
     - Download from: https://code.visualstudio.com/
     - Install "C# Dev Kit" extension
   - **JetBrains Rider** (Commercial)

### System Requirements

- **Operating System**: Windows 10/11, macOS 10.15+, or Linux
- **RAM**: Minimum 4GB, recommended 8GB+
- **Storage**: At least 2GB free space
- **Internet Connection**: Required for LSF access and downloading dependencies

## Installation Steps

### 1. Clone the Repository

```bash
# Using HTTPS
git clone https://github.com/YOUR_USERNAME/BlazorWebScrapper.git

# Or using SSH (if you have SSH keys set up)
git clone git@github.com:YOUR_USERNAME/BlazorWebScrapper.git

# Navigate to the project directory
cd BlazorWebScrapper/BlazorWebScrapper
```

### 2. Restore Dependencies

Open a terminal/command prompt in the project folder and run:

```bash
dotnet restore
```

This will download all required NuGet packages including:
- HtmlAgilityPack (for HTML parsing)
- PuppeteerSharp (for browser automation)

### 3. Build the Application

```bash
dotnet build
```

If successful, you should see "Build succeeded" with 0 errors.

### 4. Run the Application

```bash
dotnet run
```

The application will start and display URLs like:
```
Now listening on: https://localhost:7227
Now listening on: http://localhost:5124
```

### 5. Access the Application

Open your web browser and navigate to:
- **HTTPS**: https://localhost:7227 (recommended)
- **HTTP**: http://localhost:5124

## First Time Setup

### Browser Download
When you first run the scraper, PuppeteerSharp will automatically download a Chromium browser (~150MB). This only happens once and the download location will be shown in the status messages.

### LSF Credentials
You'll need valid LSF (university system) credentials:
- **LSF URL**: Your university's LSF base URL
- **Username**: Your university username
- **Password**: Your university password

**Security Note**: Credentials are only stored temporarily in memory during the scraping session and are not saved to disk.

## Usage Guide

### Basic Workflow

1. **Enter LSF Details**:
   - Faculty URL (e.g., search results page for your courses)
   - LSF Username and Password
   - Semester Code (e.g., "SoSe25", "WiSe24")

2. **Select Course Types**:
   - Choose which types of courses to scrape (Vorlesung, Seminar, etc.)
   - Multiple selections are supported

3. **Start Scraping**:
   - Click "Start Data Gathering"
   - Monitor progress in the status log
   - Wait for completion (usually 2-5 minutes depending on course count)

4. **Review and Export**:
   - Review the collected course data in the table
   - Select/deselect courses as needed
   - Set minimum student count filter (optional)
   - Click "Export Selected" to download CSV files

### Export Files

The application generates two files:
- **Professor/Course Data**: Contains course information with professor details
- **Student Data**: Contains student enrollment information

Both files include:
- ✅ German umlaute (ä, ö, ü) properly encoded for Excel
- ✅ Semester codes appended to course codes
- ✅ UTF-8 BOM for automatic Excel recognition

## Troubleshooting

### Common Issues

1. **"Command 'dotnet' not found"**
   - Solution: Install .NET 9.0 SDK and restart your terminal

2. **Build errors about missing packages**
   - Solution: Run `dotnet restore` in the project directory

3. **Browser automation fails**
   - Solution: Ensure you have sufficient disk space for Chromium download
   - Check your internet connection

4. **LSF login fails**
   - Solution: Verify your credentials work in a regular browser first
   - Some universities use 2FA - this tool doesn't support 2FA

5. **No professor data appears**
   - Solution: Check that the LSF URLs are accessible and not rate-limited
   - Try running the scraper at off-peak hours

6. **German characters display incorrectly in Excel**
   - Solution: The files include UTF-8 BOM, but if issues persist, try opening with "Data > From Text/CSV" in Excel and select UTF-8 encoding

### Performance Tips

- **Smaller batches**: Scrape fewer course types at once for faster processing
- **Off-peak hours**: LSF systems are typically faster outside business hours
- **Stable connection**: Ensure reliable internet connection during scraping

## Development

### Project Structure

```
BlazorWebScrapper/
├── Components/
│   ├── Pages/
│   │   └── Home.razor          # Main application logic
│   ├── Layout/
│   └── App.razor              # Root component
├── Properties/
│   └── launchSettings.json    # Development server settings
├── Program.cs                 # Application entry point
├── BlazorWebScrapper.csproj   # Project file
└── README.md                  # This file
```

### Modifying the Code

- **Main logic**: All scraping logic is in `Components/Pages/Home.razor`
- **Styling**: CSS is embedded in the same file
- **Course type mapping**: Modify the switch statement in the ExtractData method
- **Export format**: Modify the ExportSelectedData method

### Building for Production

```bash
# Build optimized release version
dotnet publish -c Release

# Files will be in: bin/Release/net9.0/publish/
```

## Security Considerations

⚠️ **Important Security Notes**:

- This tool handles student personal data - ensure GDPR/privacy compliance
- LSF credentials are stored temporarily in memory only
- Use HTTPS when possible (default configuration)
- Don't commit credentials to version control
- Consider running on a secure, isolated network for sensitive data

## Legal and Ethical Use

- ✅ **Authorized use only**: Only use with proper authorization from your institution
- ✅ **Respect rate limits**: Don't overwhelm LSF servers
- ✅ **Data protection**: Handle student data according to privacy regulations
- ✅ **Terms of service**: Comply with your university's LSF terms of service

## Support

For issues or questions:

1. Check the troubleshooting section above
2. Review the status messages in the application for specific errors
3. Ensure all prerequisites are properly installed
4. For code-related issues, check the project's GitHub Issues

## License

[Add your license information here]

---

**Note**: This tool is designed for educational/administrative purposes and should only be used in compliance with your institution's policies and applicable privacy laws.