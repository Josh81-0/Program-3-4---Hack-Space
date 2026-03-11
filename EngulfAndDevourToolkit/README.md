# Engulf & Devour Internal Toolkit

A Blazor Server web application built for the fictitious company **Engulf & Devour (E&D)** to support customer service and accounting teams with two essential internal tools:

1. **Reserve Study Inventory** — Manage HOA reserve components, project 30-year expenditures, calculate recommended monthly fees, and export/import CSV data.
2. **Balance Sheet Builder** — Build simple balance sheets with assets and liabilities, compute equity automatically, generate downloadable Word (.docx) documents, and track uploaded document metadata (in-memory only).

## Technology Stack

- **Framework**: .NET 9 (Blazor Server)  
  *(Note: Code is compatible with .NET 10 once available — only the target framework needs updating)*
- **UI**: Bootstrap 5 (via CDN and custom styles)
- **Charts**: Chart.js (via CDN + JS interop)
- **Word Documents**: DocumentFormat.OpenXml
- **Testing**: xUnit
- **State Management**: Component-based (parent owns state, children use `[Parameter]` + `EventCallback`)
- **Storage**: In-memory only — no database, no file system writes, no external APIs

## Features Implemented

- Multi-page application with proper routing and shared navigation
- Reusable components with parameters and event callbacks
- Clear state ownership (pages own lists/models, children receive & raise events)
- Business logic extracted into injectable services
- Unit tests for calculation logic (at least 3 per required service)
- CSV export/import with basic validation (Reserve Study)
- Word .docx document generation and download (Balance Sheet)
- File upload metadata tracking (in-memory, no parsing) (Balance Sheet)
- Simple bar chart of annual expenditures (Reserve Study)

## How to Run the Application

1. Clone the repository
2. Navigate to the project folder
3. Restore packages:
   ```bash
   dotnet restore