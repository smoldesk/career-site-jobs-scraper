# Career Site & Remote Jobs Scraper

![Career Site Jobs Scraper: live jobs from 12,000+ company career sites on Greenhouse, Lever, Ashby and Workday](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/career-site/banner.webp)

Search live jobs straight from company career sites on Greenhouse, Lever, Ashby and Workday. Over 12,000 boards and 1 million open jobs. Find remote jobs you can take from your country, with visa sponsorship and salary in USD. No login. Runs on [Apify](https://apify.com/smoldesk/career-site-jobs-scraper).

## What you get

- **Directory search**: filter all boards by title keywords, locations, remote or hybrid, seniority and posted-within days.
- **Company mode**: paste a careers page, an `ats:slug` or a company name. Also covers Workable, Recruitee, SmartRecruiters and Personio.
- **Job data**: title, department, location, workplace type, employment type, seniority, posting date and apply URL.
- **Salary**: from the ATS pay fields, or parsed from the description text, plus the range in USD per year.
- **Remote eligibility**: every remote job tagged `worldwide`, `region` (EMEA, APAC, LATAM, Europe...) or `country` (ISO codes), read from the location and, when that only says "Remote", from rules in the ad. Filter with `remoteEligibleFrom: ["Malaysia"]`.
- **Visa sponsorship** (true / false / null) and **years of experience** asked, read from the ad text.
- **Full description** as Markdown and HTML.
- **Closed jobs**: a free `status: "closed"` record when a posting disappears.
- **New jobs only** mode for daily monitoring.

## Run it

Open the [Career Site Jobs Scraper on Apify](https://apify.com/smoldesk/career-site-jobs-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~career-site-jobs-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "titleKeywords": ["product manager"],
    "workplaceTypes": ["remote"],
    "seniorityLevels": ["senior", "lead"],
    "postedWithinDays": 14,
    "excludeCompanies": ["Jobgether"],
    "maxJobsPerCompany": 5,
    "maxJobs": 200
  }'
```

For one company: `{"companies": ["https://boards.greenhouse.io/stripe", "lever:netflix"]}`.

Remote jobs open to people in Brazil, paying at least $60,000 a year:

```json
{"titleKeywords": ["backend engineer"], "remoteEligibleFrom": ["Brazil"], "minSalaryUsd": 60000, "maxJobs": 50}
```

## Sample output

```json
{
  "status": "open",
  "ats": "ashby",
  "company": "Bjakcareer",
  "title": "Product Manager - AI Finance",
  "department": "Product",
  "location": "United States",
  "workplaceType": "remote",
  "isRemote": true,
  "remoteScope": "country",
  "remoteCountries": ["US"],
  "remoteRegions": [],
  "employmentType": "full-time",
  "seniority": "mid",
  "postedAt": "2026-10-14T07:38:51.919000+00:00",
  "applyUrl": "https://jobs.ashbyhq.com/bjakcareer/12bc7213-c111-4409-8981-...",
  "salaryMin": null,
  "salaryMax": null,
  "salaryCurrency": null,
  "markdown": "**About BJAK** ..."
}
```

Export as JSON, CSV or Excel, or pipe it into Google Sheets, Make, Zapier or n8n.

## Pricing

Pay per result. No monthly fee.

| Result | Price |
|---|---|
| Job | $2 per 1,000 |
| Boards read | $0.006 per 1,000 boards ($0.004 until 21 Nov 2026) |
| Closed-job notice | Free |

A full scan of the directory reads about 12,900 boards, which adds about $0.08.

## Limits

- iCIMS, Taleo, SuccessFactors and BambooHR are not supported.
- Job aggregators on Lever or Greenhouse can flood results. Use `excludeCompanies` and `maxJobsPerCompany`.
- Careers pages with their own front end may not be detected. Paste the board URL instead.
- Some Workday tenants throttle automated requests and are skipped after retries.

## FAQ

**Do I need an account or API key for Greenhouse, Lever or Workday?**
No. This reads the public job boards candidates see.

**How do I search jobs across many companies at once?**
Leave `companies` empty and set `titleKeywords`. The directory covers 12,000+ boards.

**How do I get all jobs of one company?**
Put the company name, careers URL or `ats:slug` in `companies`. Set `maxJobs` to the number you need.

**Can I get only remote jobs I can take from my country?**
Set `remoteEligibleFrom` to your country or region, e.g. `["Germany"]` or `["APAC"]`. Worldwide jobs and jobs for your region pass; "Remote - US" jobs don't. Jobs that only say "Remote" are left out unless you set `includeUnspecifiedRemote`.

**Can I filter remote jobs with a salary?**
Set `workplaceTypes` to `["remote"]` and `salaryOnly` to `true`, or set `minSalaryUsd`.

**Can I find jobs that sponsor visas?**
Set `visaSponsorshipOnly` to `true`. Only ads that say so are kept.

**How do I track new postings?**
Turn on `onlyNewJobs` and schedule the run daily. Closed postings arrive as free `closed` records.

## More scrapers

Career Site Jobs is one of 21 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
