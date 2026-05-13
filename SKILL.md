---
name: alchemy-onboarding-health-agent
display_name: Alchemy Onboarding Health Agent
description: "Generate a self-service Alchemy onboarding health dashboard for any WWSO domain. Use when someone asks to create an onboarding hub, check domain health, build an Alchemy landing page, or generate a domain adoption tracker."
icon: "🏥"
trigger: generate alchemy onboarding hub
inputs:
  - name: domain_name
    description: "The WWSO domain name to generate the hub for (e.g., 'Physical AI', 'IoT', 'Retail')"
    type: string
    required: true
  - name: domain_id
    description: "Optional Alchemy domain ID number. If not provided, the skill will search Alchemy by domain_name."
    type: number
    required: false
tools: [run_python, run_javascript, file_write, file_read, file_copy, open_in_session_tab, url_fetch, create_scheduled_agent]
depends-on: [user_mcp__alchemy_taxonomy_mcp]
---

## Overview

This skill generates a complete, interactive HTML onboarding health dashboard for any WWSO domain on the Alchemy platform. It pulls live data from Alchemy (solution areas, use cases, sales plays, SFDC tags, demos, workshops), computes AEIM capability completion, benchmarks against top-performing domains, identifies cross-domain opportunities, and renders a self-service webapp with prioritization tools. It also creates a weekly scheduled agent that refreshes the data every Monday at 8am and notifies the user of changes. Optionally deploys to S3 or GitHub Pages for internal sharing.

## Workflow

### Step 1: Query Alchemy for Domain Data
- **Mode**: `agentic`
- **Tool**: `alchemy_taxonomy_mcp__searchEntities`, `alchemy_taxonomy_mcp__getMetadataForSearch`
- **Input**: `{{domain_name}}` and optionally `{{domain_id}}`
- **Output**: Domain metadata, solution areas, use cases, sales plays, SFDC/OTE tags, demos, workshops
- **Validate**: Domain found with at least a domain ID, name, and status.
- **On failure**: If no domain found, ask user to verify the domain name or provide the ID.

Query sequence:
1. Search for the domain entity to get the domain ID and metadata
2. Search for solution areas within the domain
3. Search for use cases within the domain
4. Search for sales plays within the domain
5. Get SFDC/OTE tag counts from use case metadata
6. Search for demos mapped to use cases within the domain
7. Search for workshops mapped to use cases within the domain

For demos and workshops: search by domain and check which use cases have linked demo or workshop assets. Track both the total count and the UC coverage (how many UCs have at least one demo/workshop mapped).

Work quickly through queries to avoid MCP token expiration.

### Step 2: Gather WGLL Benchmarks
- **Mode**: `agentic`
- **Tool**: `alchemy_taxonomy_mcp__searchEntities`
- **Input**: Known high-performing domains (Retail, Financial Services, CPG, Automotive)
- **Output**: Benchmark data — published play counts, Highspot coverage, SFDC tag counts, demo/workshop counts
- **Validate**: At least 3 benchmark domains return valid data
- **On failure**: Use cached benchmark data: Retail (40 plays, 100% Highspot), Financial Services (49 plays, 173 tags), CPG (40 plays, 113 tags), Automotive (114 tags)

### Step 3: Identify Cross-Domain Opportunities
- **Mode**: `agentic`
- **Tool**: `alchemy_taxonomy_mcp__searchEntities`
- **Input**: `{{domain_name}}` as keyword search across all domains
- **Output**: List of other domains referencing this domain
- **Validate**: Results are deduplicated with clear connection rationale
- **On failure**: Return empty cross-domain section rather than blocking

### Step 4: Compute AEIM Capability Status
- **Mode**: `agentic`
- **Tool**: `run_python`
- **Input**: All data from Steps 1-3
- **Output**: 17-capability AEIM status matrix, 7 integration experience statuses, overall completion %
- **Validate**: All 17 capabilities have a status and all 7 integration experiences assessed

AEIM Capabilities (17):
- Planning (5): Taxonomy Refresh, Use Case Prioritization, Sales Play Prioritization, Stakeholder Alignment, Readiness Assessment
- Authoring (6): Use Case Authoring, Sales Play Authoring, Content Quality Bar Raise, OTE Tag Creation, **Demo Mapping**, **Workshop Mapping**
- Impact (3): GTM Performance Management, Content Lifecycle Management, Engagement Analytics
- Discovery (3): Highspot Integration, Field Recommendations, Cross-Domain Mapping

**Demo Mapping** assessment logic:
- Complete: ≥80% of published UCs have at least one demo linked
- In Progress: 1-79% of published UCs have a demo linked
- Not Started: 0 demos mapped to any UC in the domain

**Workshop Mapping** assessment logic:
- Complete: ≥80% of published UCs have at least one workshop linked
- In Progress: 1-79% of published UCs have a workshop linked
- Not Started: 0 workshops mapped to any UC in the domain

Integration Experiences (7): AWSentral Account Recommendations, Highspot for Field, DataBook, Field Advisor, Autonomous Prospecting, CampaignSentral, GTM Performance Dashboard

### Step 5: Generate the HTML Webapp
- **Mode**: `deterministic`
- **Tool**: `file_write`
- **Input**: All computed data from Steps 1-4
- **Output**: `{domain-slug}-onboarding-hub.html` and `{domain-slug}-data.json`
- **Validate**: HTML renders in session tab, all tabs functional
- **On failure**: Check for JS syntax errors, ensure all data variables defined

Tabs (9): Dashboard, Prioritization Survey, Roadmap, Taxonomy & Sales Plays, Capabilities, Integration Experiences, Cross-Domain, What Good Looks Like, Resources

The **Capabilities tab** must include Demo Mapping and Workshop Mapping as separate rows in the Authoring section. Show:
- Status (Complete / In Progress / Not Started)
- Coverage metric: "X/Y UCs have demos mapped (Z%)" and "X/Y UCs have workshops mapped (Z%)"
- List which UCs have demos/workshops and which are missing them
- Blocker/next action: e.g., "5 UCs missing demo assets — prioritize high-traffic UCs first"

The **Dashboard tab** KPIs should include Demo Coverage % and Workshop Coverage % as additional metrics.

Design: AWS-style (dark navy #1a1a2e, orange #ff9900, white cards), no localStorage, try-catch all init, GA4 placeholder, responsive.

### Step 6: Create Weekly Refresh Agent
- **Mode**: `deterministic`
- **Tool**: `create_scheduled_agent`
- **Input**: Domain name, domain ID, and baseline metrics from Step 4
- **Output**: Scheduled agent ID
- **Validate**: Agent created and enabled
- **On failure**: Notify user that manual refresh will be needed; provide instructions

Create a scheduled agent with:
- **Name**: `alchemy-{domain-slug}-health-refresh`
- **Schedule**: Every Monday at 08:00 (user's timezone)
- **Tools**: `alchemy_taxonomy_mcp__searchEntities`, `alchemy_taxonomy_mcp__getMetadataForSearch`, `run_python`, core file read tools
- **Objective**: Query Alchemy for the domain, compare current metrics to the baseline. If any metric has changed, notify the user with a summary of what changed and the delta. If nothing changed, send a brief "no changes" check-in.
- **Notification behavior**: importance="important" for changes detected, importance="fyi" for no-change check-ins

The baseline metrics to store in the agent objective:
- # Solution Areas (published / total)
- # Use Cases (published / total)
- # Sales Plays (published / total)
- # SFDC/OTE Tags
- Highspot coverage %
- Demo coverage % (UCs with demos / total published UCs)
- Workshop coverage % (UCs with workshops / total published UCs)
- Overall AEIM capability completion %

Check if a scheduled agent already exists for this domain before creating a duplicate.

### Step 7: Deploy (Optional)
- **Mode**: `agentic`
- **Tool**: `run_python`
- **Input**: Generated HTML/JSON files, user's deployment preference
- **Output**: Hosted URL or deployment instructions

**Option A — S3 Static Site + CloudFront + Midway:**
1. Create S3 bucket `alchemy-hub-{domain-slug}` with static hosting
2. Upload HTML + JSON with correct content-types
3. Output CLI commands for CloudFront distribution + OAI setup
4. Provide Lambda@Edge Midway auth attachment instructions
5. Required IAM: `s3:CreateBucket`, `s3:PutObject`, `s3:PutBucketWebsite`

**Option B — GitHub Pages:**
1. Push HTML + JSON to GitHub repo (e.g., `https://github.com/karleigh13/Alchemy-Onboarding-Hub`)
2. Organize files in a folder per domain (e.g., `/physical-ai/index.html`)
3. Enable GitHub Pages in Settings → Pages → Source: main branch
4. Site goes live at `https://karleigh13.github.io/Alchemy-Onboarding-Hub/{domain-slug}/`

Ask the user which deployment method they prefer. If neither, skip this step.

### Step 8: Present Results
- **Mode**: `deterministic`
- **Tool**: `open_in_session_tab`, `file_copy`
- **Input**: Generated files from Step 5
- **Output**: Webapp in session tab + copy in ~/Downloads + scheduled agent confirmation
- **Validate**: Session tab opens, file accessible, agent confirmed active

Present:
1. The hub open in session tab
2. A summary table of domain health vs benchmarks (including demo/workshop coverage)
3. Confirmation of the weekly refresh agent with its schedule and baseline metrics
4. Deployment status (if applicable)

## Output

- **Primary**: Interactive HTML webapp at `artifacts/{domain-slug}-onboarding-hub.html`
- **Data file**: `artifacts/{domain-slug}-data.json`
- **Shareable copy**: `~/Downloads/{domain-slug}-onboarding-hub.html`
- **Scheduled agent**: `alchemy-{domain-slug}-health-refresh` (runs every Monday 8am)
- **Hosted URL** (if deployed): S3 or GitHub Pages URL
- **Summary**: Domain health metrics vs benchmarks comparison table

## Lessons Learned

### Do
- Query Alchemy for ALL entity types separately (domain, SAs, UCs, plays, demos, workshops)
- Include both published AND unpublished counts — the gap is the key health indicator
- Use AEIM's 17 capabilities (including Demo Mapping and Workshop Mapping) as the definitive checklist
- Separate data from presentation (JSON + HTML) for automated refresh
- Wrap all JavaScript initialization in try-catch
- Include the goal-aware prioritization survey — it's the differentiator
- Benchmark against 4 specific domains (Retail, FSI, CPG, Auto)
- Work quickly through Alchemy queries to avoid token expiration
- Store baseline metrics in the scheduled agent objective so it can detect changes
- Create the refresh agent as part of initial generation — don't make the user ask separately
- Show demo/workshop coverage as a percentage AND list which UCs are missing assets

### Don't
- Don't use localStorage — blocked in sandboxed iframes
- Don't use Set constructor with spread — use object-based dedup
- Don't merge tabs to "simplify" — users want the full breakdown
- Don't skip cross-domain search — it surfaces high-value collaboration
- Don't query too many domains sequentially in one session — token can expire
- Don't create duplicate scheduled agents — check if one already exists for the domain first
- Don't combine Demo and Workshop into a single capability — they are tracked separately

### Common Failures
- **Domain not found**: Try both short and long form names (e.g., "IoT" vs "Internet of Things")
- **Empty sales play results**: Search by solution area IDs too
- **Token expiration**: Work fast, proceed with partial data if auth fails
- **HTML rendering issues**: Validate JSON data arrays for missing commas
- **Duplicate agents**: If regenerating a hub, check for existing scheduled agent before creating a new one
- **No demo/workshop entities found**: Some domains may not have these entity types in Alchemy yet — show "Not Started" with a note that no assets exist rather than an error

### When to Ask the User
- If multiple domains match the search — ask which one
- If domain has 0 solution areas — confirm they want to proceed
- Before deployment — confirm bucket name/region or GitHub repo
- If benchmark queries fail — ask whether to use cached benchmarks
- Which deployment option they prefer (S3 vs GitHub Pages vs skip)
