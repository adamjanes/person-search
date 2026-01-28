# Person Search

Research tool for gathering context on people before outreach.

## Workflow

1. User provides a LinkedIn URL (or name + company)
2. Claude gathers info from multiple sources:
   - LinkedIn profile via Apify scraper
   - LinkedIn posts (recent activity)
   - Company website
   - MentorCruise, Twitter/X, personal sites
   - Web search for interviews, podcasts, articles
3. Claude synthesizes a profile summary
4. Claude suggests 3 short connection request ideas following the format below

## Connection Request Format

Keep requests SHORT (under 40 words). Follow this structure:
- Find something **shared** (industry, background, mutual interest)
- **Don't pitch** anything
- **Don't compliment** their profile generically
- Ask a **simple, genuine question**

**Good examples:**
> "Hey [Name] - also work in the fractional exec space. Curious - are you seeing more demand from ecommerce brands lately?"

> "Hey [Name] - noticed you bootstrapped in 2015. Did the same. What made you shift to advisory?"

**Bad examples:**
> "I loved your profile and would love to connect to learn more about your journey..." (too generic, no shared context)

> "I help founders with X and think we could collaborate..." (pitching)

## Environment Variables

Copy `.env.example` to `.env` and fill in:

```
APIFY_TOKEN=your_apify_token_here
```

## Apify Scrapers Used

### LinkedIn Profile Posts
Fetches recent posts from a LinkedIn profile.

```bash
curl -X POST "https://api.apify.com/v2/acts/harvestapi~linkedin-profile-posts/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "targetUrls": ["https://www.linkedin.com/in/USERNAME/"],
    "maxPosts": 5,
    "postedLimit": "3months"
  }'
```

### LinkedIn Profile Comments
Fetches comments made BY a profile (shows what they engage with).

```bash
curl -X POST "https://api.apify.com/v2/acts/harvestapi~linkedin-profile-comments/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "profiles": ["https://www.linkedin.com/in/USERNAME/"],
    "maxItems": 20,
    "postedLimit": "3months"
  }'
```

## Knowledge Storage

Research on specific people can be saved to `knowledge/` for future reference:
- `knowledge/jeff-ritger.md`
- `knowledge/jane-doe.md`

## Quick Start

```bash
cd /Users/adamjanes/code/projects/person-search
claude
```

Then: "Research [LinkedIn URL] for outreach"
