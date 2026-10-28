# Indeed Jobs Scraper: export to CSV or JSON with Python, Node.js or curl

Scrape Indeed.com job search results: title, company and its rating, location, remote or hybrid, salary range and period where the employer shows it, job types, posting date, urgent and easy apply flags, snippet and job URL. Filter by keyword, location, date posted and job type.

This repository has ready scripts for the [Indeed Jobs Scraper: Listings, Salary, Company Rating, Remote](https://apify.com/v0iddo/indeed-scraper) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv --max-items 50
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/indeed-scraper), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "query": "data analyst",
  "location": "Remote",
  "datePosted": "any",
  "jobType": "",
  "maxPages": 1,
  "maxItems": 20
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `query` | string | Job title, keywords, or company name. |
| `location` | string | City, state, zip code, or 'remote'. |
| `datePosted` | string | Only return jobs posted within this many days. |
| `jobType` | string | Filter by employment type. |
| `includeSponsored` | boolean | Paid listings Indeed mixes into the results. |
| `maxPages` | integer | Number of result pages to read per search. |
| `maxItems` | integer | The run stops after this many result rows. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "jobKey": "26953338555a1787",
  "title": "Technical Analyst I",
  "company": "Indeed",
  "companyRating": 4.2,
  "companyReviews": 1786,
  "location": "Remote",
  "remote": true,
  "workplace": null,
  "salary": "$69,000 - $144,000 a year",
  "salaryMin": 69000,
  "salaryMax": 144000,
  "salaryPeriod": "year",
  "currency": "USD",
  "jobTypes": [],
  "postedAt": "2026-10-26",
  "sponsored": false,
  "urgentlyHiring": false,
  "easyApply": true,
  "snippet": "Use data analysis to identify trends, workflow gaps, and operational improvement opportunities. Use data analysis to identify patterns and inform actions;…",
  "jobUrl": "https://www.indeed.com/viewjob?jk=26953338555a1787",
  "position": 1,
  "page": 1,
  "searchUrl": "https://www.indeed.com/jobs?q=data+analyst&l=Remote"
}
```

Fields: `jobKey`, `title`, `company`, `companyRating`, `companyReviews`, `location`, `remote`, `workplace`, `salary`, `salaryMin`, `salaryMax`, `salaryPeriod`, `currency`, `jobTypes`, `postedAt`, `sponsored`, `urgentlyHiring`, `easyApply`, `snippet`, `jobUrl`, `position`, `page`, `searchUrl`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/indeed-scraper').call({"query": "data analyst", "location": "Remote", "datePosted": "any", "jobType": "", "maxPages": 1, "maxItems": 20});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~indeed-scraper/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"query": "data analyst", "location": "Remote", "datePosted": "any", "jobType": "", "maxPages": 1, "maxItems": 20}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$0.50 per 1,000 results**, nothing per run. `maxItems` caps the rows, and so the cost, of one run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
