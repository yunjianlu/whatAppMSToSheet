# n8n Invoice Processing Workflow

![Demo](assets/demo.png)

## Overview

This project demonstrates an automated invoice processing workflow using [n8n](https://n8n.io/). It integrates WhatsApp, Microsoft services, and Google Sheets to streamline invoice management and data extraction.

## Features

- Automated invoice collection from WhatsApp
- Data extraction and validation
- Integration with Microsoft services (e.g., Outlook, OneDrive)
- Storage and tracking in Google Sheets
- Error handling and notifications

## Project Structure

```
workflows/         # n8n workflow JSON exports
assets/            # Images, diagrams, or demo screenshots
docs/              # Documentation and architecture
scripts/           # Helper scripts (e.g., setup)
```

## Getting Started

1. **Clone the repository:**
   ```powershell
   git clone https://github.com/yunjianlu/whatAppMSToSheet.git
   cd whatAppMSToSheet
   ```
2. **Import the workflow:**
   - Open n8n.
   - Import the JSON file from `workflows/invoice-processing.json`.
3. **Configure credentials:**
   - Set up WhatsApp, Microsoft, and Google Sheets credentials in n8n.
4. **Run the workflow:**
   - Trigger the workflow and monitor execution.

## Architecture

See [docs/architecture.md](docs/architecture.md) for a detailed overview.

## Demo

Add a screenshot or GIF of your workflow in action in the `assets/` folder and reference it above.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

## Contact

For questions or collaboration, please contact [your email or LinkedIn].
