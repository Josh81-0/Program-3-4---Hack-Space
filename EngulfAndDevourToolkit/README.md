# Engulf & Devour Internal Toolkit

## 1. Project Overview

This is a Blazor Server application developed for the fictitious company Engulf & Devour (E&D) to support their customer service and accounting teams. It features two tools: a Reserve Study Inventory for managing HOA reserve components with projections, calculations, CSV export/import, and a chart; and a Balance Sheet Builder for entering assets/liabilities, computing equity, and handling Word (.docx) export/upload metadata. Built with .NET 9 (compatible with .NET 10), using in-memory data, reusable components, services for logic, and unit tests. No databases, auth, or external APIs.

## 2. How to Run

1. Install .NET 9 SDK (or .NET 10).
2. Clone the repository: `git clone <repo-url>`.
3. Navigate to the project: `cd EngulfAndDevourToolkit`.
4. Restore packages: `dotnet restore`.
5. Build: `dotnet build`.
6. Run tests: `dotnet test` (optional).
7. Run the app: `dotnet run`.
8. Access at `https://localhost:5001` (or the displayed port).

## 3. Page Map (Routes) and What Each Page Does

- `/`: Home page - Provides a welcome message and navigation instructions.
- `/reserve-study`: Reserve Study Inventory - Allows adding/removing components (name, useful life, cost), setting number of units, calculating 30-year expenditures and monthly HOA fees, displaying a bar chart of annual projections, and exporting/importing component data via CSV with validation.
- `/balance-sheet`: Balance Sheet Builder - Enables adding/removing asset and liability line items, auto-calculates totals and equity, downloads a formatted Word (.docx) document with the balance sheet, and uploads .docx files (storing metadata like filename and timestamp in memory).

## 4. Component Breakdown (What Components Exist and Why)

- `MainLayout.razor` (Shared/): Defines the overall app layout with navigation sidebar and content area for consistent structure across pages.
- `NavMenu.razor` (Shared/): Provides the navigation menu with links to home, reserve study, and balance sheet pages; ensures easy routing.
- `ReserveComponentRow.razor` (Components/): Reusable input row for a single reserve component; promotes code reuse and clean UI by handling name, life, cost inputs and remove event.
- `LineItemRow.razor` (Components/): Reusable input row for assets/liabilities; avoids duplication in the balance sheet page by managing name, amount, and remove functionality.
- `Index.razor` (Pages/): Optional home page for basic welcome content.
- `ReserveStudy.razor` (Pages/): Manages the reserve study feature, including state, calculations, CSV I/O, and chart.
- `BalanceSheet.razor` (Pages/): Handles the balance sheet feature, including state, totals, document export, and upload metadata.

Components exist to follow Blazor best practices: reusability via parameters and EventCallbacks, and separation of concerns.

## 5. Where State Lives and How It Flows (parent → child, child → parent)

State is owned by each parent page component (e.g., `ReserveStudy.razor` owns lists of components, units, and results; `BalanceSheet.razor` owns assets/liabilities lists and upload metadata). No shared or static state services are used.

Flow:
- Parent → Child: Data (e.g., a single `ReserveComponent` or `LineItem`) is passed down via `[Parameter]` properties to child components like `ReserveComponentRow` or `LineItemRow` for display and editing.
- Child → Parent: Changes or actions (e.g., remove button click) are communicated up via `EventCallback` events, allowing the parent to update its owned state (e.g., remove item from list) and trigger re-renders or calculations.
- Calculations flow from parent to services (e.g., `ReserveService.CalculateAnnualExpenditures(components)`) and back to parent state for display.
- This ensures unidirectional data flow and parent ownership per guidelines.

## 6. Testing Summary (What You Tested)

Unit tests are in a separate project (`EngulfAndDevourToolkit.Tests`) using xUnit, focusing on services for calculation logic.

Tested:
- `ReserveStudyService`:
  - Replacement scheduling: Verified costs are added to correct years (multiples of useful life) over 30 years.
  - Totals and fee calculation: Checked sum of expenditures and monthly fee formula with sample data.
  - Input validation: Ensured exceptions for invalid inputs like zero units.

At least 3 tests: one typical case for scheduling, one for fee calculation, one for exception handling. Tests are deterministic with exact assertions.

## 7. AI Usage Disclosure (Required)

- AI tools used: Grok (xAI)

- How they were used and how output was reviewed/modified: Grok was used sparingly to help with initial code structure ideas and to clarify a few Blazor/OpenXML patterns. All suggestions were manually reviewed, heavily modified, tested locally, and expanded with my own implementation to meet the exact assignment requirements (state flow, calculations, tests, CSV/DOCX handling, etc.). The majority of the code, logic, and documentation was written and verified by hand.
