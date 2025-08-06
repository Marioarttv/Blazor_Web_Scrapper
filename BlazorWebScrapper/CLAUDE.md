# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Build and Run
```bash
# Build the application
dotnet build

# Run the application in development mode
dotnet run

# Run with specific profile
dotnet run --launch-profile https
```

### Development Server
The application runs on:
- HTTPS: https://localhost:7227
- HTTP: http://localhost:5124

### Publishing
```bash
# Publish for release
dotnet publish -c Release
```

## Architecture Overview

This is a Blazor Server web application (.NET 9.0) designed for web scraping LSF (Lehre, Studium, Forschung) university course management systems. The application provides a form-based interface for gathering course and student enrollment data from German university LSF systems.

### Core Technology Stack
- **Framework**: Blazor Server with Interactive Server Components (.NET 9.0)
- **Web Scraping**: HtmlAgilityPack for HTML parsing, PuppeteerSharp for browser automation
- **UI**: Single-page application with inline CSS styling
- **Authentication**: Handles LSF login automation through Puppeteer

### Application Structure

#### Main Components
- **Program.cs**: Application entry point with minimal configuration for Blazor Server
- **Components/App.razor**: Root HTML document with InteractiveServer render mode
- **Components/Pages/Home.razor**: Single main page containing the entire application logic (~1,900 lines)
- **Components/Layout/MainLayout.razor**: Basic layout wrapper
- **Components/Routes.razor**: Routing configuration

#### Key Features
1. **Form Input**: Collects LSF URL, credentials, semester code, and course type selections
2. **Web Scraping Pipeline**:
   - Fetches faculty HTML using HttpClient
   - Extracts faculty and course links using HtmlAgilityPack
   - Authenticates via Puppeteer browser automation
   - Scrapes student enrollment data from LSF system
   - Processes professor and course metadata
3. **Data Processing**: Combines course data with student enrollment information
4. **Export Functionality**: Downloads CSV/TXT files with course and student data

### Data Models
The application defines several key classes in Home.razor:
- **DataEntry**: Core course and professor data structure
- **SelectableDataEntry**: Extends DataEntry with selection state for export
- **BelegInformationen**: Student enrollment data structure
- **FormModel**: Form validation and input binding

### Web Scraping Architecture
The scraping process follows this flow:
1. **Faculty Discovery**: Parse main LSF page for faculty links
2. **Course Discovery**: Extract course links filtered by selected course types (Vorlesung, Seminar, etc.)
3. **Authentication**: Use Puppeteer to log into LSF system
4. **Student Data Extraction**: Navigate through course enrollment pages to collect student lists
5. **Professor Data Extraction**: Parse individual course pages for professor information

### Security Considerations
- Handles LSF authentication credentials (stored temporarily in form state)
- Uses Puppeteer in headless mode for automated login
- Processes student personal data (names, emails) - ensure compliance with data protection regulations

### Development Notes
- All application logic is contained within the single Home.razor component
- Uses inline CSS for styling (no separate CSS framework)
- JavaScript interop for file download functionality
- Progress tracking and status logging built into the UI
- Supports filtering and bulk export of collected data

### Course Type Mapping
The application maps LSF course types to numeric codes:
- Vorlesung (1), Seminar (2), Haupt/Proseminar (3), Übung (4), Praktikum (5)
- Hauptseminar (8), Kolloquium (9), Tutorium (10), Ringvorlesung (11)
- Exkursion (12), Sonstige (14), Präsenzphase (15), Kompakt (16)