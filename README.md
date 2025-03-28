# CircleCI Usage Report Generator

This tool generates usage reports for CircleCI organizations on a shared plan. It fetches data for all organizations sharing your plan and creates comprehensive CSV reports.

## Features

- Fetch all organizations on your shared CircleCI plan
- Generate usage reports for specific time periods
- Download and extract usage data in CSV format
- Combine data from multiple organizations in a single request
- Progressive backoff for large jobs to prevent API rate limiting

## Requirements

- Python 3.6+
- CircleCI account with access to your organization
- CircleCI API token

## Installation

1. Clone this repository:
   ```
   git clone https://github.com/CircleCI-Support/multi_org_usage_report.git
   cd circleci-usage-report-generator
   ```

2. Install the required dependencies:
   ```
   pip install -r requirements.txt
   ```

3. Create a `.env` file in the root directory with the following variables:
   ```
   CIRCLE_TOKEN=your-circle-ci-token
   PRIMARY_ORG_ID=your-primary-org-id
   START_DATE=2024-11-01T00:00:00Z
   END_DATE=2024-11-30T23:59:59Z
   ```

## Usage

Run the script with:

```
python usage_report.py
```

The script will:
1. Connect to CircleCI and retrieve all organizations on your shared plan
2. Create a usage export job that includes all organizations
3. Monitor the job until completion
4. Download and extract the report files
5. Save the reports to the `usage_reports` directory

## Environment Variables

- `CIRCLE_TOKEN`: Your CircleCI personal API token
- `PRIMARY_ORG_ID`: The ID of your primary CircleCI organization
- `START_DATE`: Start date for the report in ISO format (YYYY-MM-DDThh:mm:ssZ)
- `END_DATE`: End date for the report in ISO format (YYYY-MM-DDThh:mm:ssZ)

## How to Get Your CircleCI API Token

1. Log in to CircleCI
2. Go to User Settings → Personal API Tokens
3. Create a new token and copy it
4. Add it to your `.env` file

## How to Get Your Organization ID

Your organization ID can be found in the URL when you visit your CircleCI organization page:
```
https://app.circleci.com/pipelines/github/YOUR-ORG-NAME
```

Or via the API:
```
curl -X GET https://circleci.com/api/v2/me --header "Circle-Token: $CIRCLE_TOKEN"
```

## Output Files

Reports are saved in the `usage_reports` directory with the following naming convention:
```
all_orgs_TIMESTAMP_START-DATE_END-DATE.csv
```

## Troubleshooting

- **API Rate Limiting**: The script includes progressive backoff to handle rate limiting. However, CircleCI does rate limit the number of request you may make per day
- **Job Processing Time**: For large organizations, the job may take several minutes to complete
- **Authentication Errors**: Check that your CircleCI token has the correct access for your organization

## Notes

- The CircleCI Usage API limits how far back you can request data for the past 13 months
- For large date ranges, consider breaking your request into smaller chunks as you can only request data in a 32 day range
- Processing time increases with the number of organizations and the date range size

# multi_org_usage_report
