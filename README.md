[alchemy-onboarding-hub.html](https://github.com/user-attachments/files/26800111/alchemy-onboarding-hub.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Alchemy Domain Onboarding Hub</title>
    <style>
        :root {
            --aws-orange: #FF9900;
            --aws-dark: #232F3E;
            --aws-light: #37475A;
            --aws-blue: #147EB4;
            --green: #1B9C3F;
            --yellow: #D4A017;
            --red: #D13212;
            --blue: #147EB4;
            --gray: #687078;
            --bg: #F2F3F3;
            --card-bg: #FFFFFF;
            --border: #D5DBDB;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
            background: var(--bg);
            color: #16191F;
            line-height: 1.5;
        }

        /* Header */
        .header {
            background: var(--aws-dark);
            color: white;
            padding: 1rem 2rem;
            display: flex;
            align-items: center;
            justify-content: space-between;
            position: sticky;
            top: 0;
            z-index: 100;
        }
        .header h1 {
            font-size: 1.4rem;
            font-weight: 600;
        }
        .header h1 span { color: var(--aws-orange); }
        .header-meta {
            font-size: 0.85rem;
            color: #AAB7B8;
        }

        /* Navigation */
        .nav {
            background: var(--aws-light);
            padding: 0 2rem;
            display: flex;
            gap: 0;
            overflow-x: auto;
        }
        .nav button {
            background: none;
            border: none;
            color: #AAB7B8;
            padding: 0.75rem 1.25rem;
            font-size: 0.85rem;
            cursor: pointer;
            white-space: nowrap;
            border-bottom: 3px solid transparent;
            transition: all 0.2s;
        }
        .nav button:hover { color: white; }
        .nav button.active {
            color: white;
            border-bottom-color: var(--aws-orange);
        }

        /* Main content */
        .main { padding: 1.5rem 2rem; max-width: 1400px; margin: 0 auto; }

        .section { display: none; }
        .section.active { display: block; }

        /* Cards */
        .card {
            background: var(--card-bg);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 1.25rem;
            margin-bottom: 1rem;
        }
        .card h2 {
            font-size: 1.1rem;
            margin-bottom: 1rem;
            color: var(--aws-dark);
        }
        .card h3 {
            font-size: 0.95rem;
            margin-bottom: 0.75rem;
            color: var(--aws-light);
        }

        /* KPI Row */
        .kpi-row {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 1rem;
            margin-bottom: 1.5rem;
        }
        .kpi {
            background: var(--card-bg);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 1rem 1.25rem;
            text-align: center;
        }
        .kpi-value {
            font-size: 2rem;
            font-weight: 700;
            color: var(--aws-dark);
        }
        .kpi-label {
            font-size: 0.8rem;
            color: var(--gray);
            margin-top: 0.25rem;
        }
        .kpi.highlight .kpi-value { color: var(--aws-orange); }
        .kpi.green .kpi-value { color: var(--green); }
        .kpi.red .kpi-value { color: var(--red); }

        /* Tables */
        .table-wrap { overflow-x: auto; }
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 0.85rem;
        }
        th {
            background: var(--aws-dark);
            color: white;
            padding: 0.6rem 0.75rem;
            text-align: left;
            font-weight: 500;
            white-space: nowrap;
        }
        td {
            padding: 0.6rem 0.75rem;
            border-bottom: 1px solid var(--border);
        }
        tr:hover td { background: #F7F8F8; }

        /* Status badges */
        .badge {
            display: inline-block;
            padding: 0.2rem 0.6rem;
            border-radius: 4px;
            font-size: 0.75rem;
            font-weight: 600;
            white-space: nowrap;
        }
        .badge-green { background: #E6F4EA; color: #1B7D2F; }
        .badge-yellow { background: #FEF7E0; color: #9A7B1A; }
        .badge-red { background: #FCE8E6; color: #C5221F; }
        .badge-blue { background: #E8F0FE; color: #1A73E8; }
        .badge-gray { background: #F1F3F4; color: #5F6368; }

        /* Progress bar */
        .progress-bar {
            width: 100%;
            height: 8px;
            background: #E8EAED;
            border-radius: 4px;
            overflow: hidden;
        }
        .progress-fill {
            height: 100%;
            border-radius: 4px;
            transition: width 0.5s ease;
        }
        .progress-text {
            font-size: 0.75rem;
            color: var(--gray);
            margin-top: 0.2rem;
        }

        /* Domain cards grid */
        .domain-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
            gap: 1rem;
        }
        .domain-card {
            background: var(--card-bg);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 1.25rem;
            cursor: pointer;
            transition: box-shadow 0.2s, border-color 0.2s;
        }
        .domain-card:hover {
            box-shadow: 0 2px 8px rgba(0,0,0,0.1);
            border-color: var(--aws-blue);
        }
        .domain-card-header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-bottom: 0.75rem;
        }
        .domain-card-title {
            font-size: 0.95rem;
            font-weight: 600;
            color: var(--aws-dark);
        }
        .domain-card-meta {
            font-size: 0.8rem;
            color: var(--gray);
            margin-bottom: 0.5rem;
        }
        .domain-card-phase {
            font-size: 0.8rem;
            color: var(--aws-blue);
            font-weight: 500;
        }

        /* Capability categories */
        .cap-category {
            display: inline-block;
            padding: 0.15rem 0.5rem;
            border-radius: 3px;
            font-size: 0.7rem;
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        .cap-planning { background: #E8F0FE; color: #1A73E8; }
        .cap-authoring { background: #FEF7E0; color: #9A7B1A; }
        .cap-impact { background: #E6F4EA; color: #1B7D2F; }
        .cap-discovery { background: #FCE8E6; color: #C5221F; }

        /* Effort badges */
        .effort-ai { background: #E8F0FE; color: #1A73E8; }
        .effort-low { background: #E6F4EA; color: #1B7D2F; }
        .effort-medium { background: #FEF7E0; color: #9A7B1A; }
        .effort-high { background: #FCE8E6; color: #C5221F; }

        /* Phase timeline */
        .timeline {
            display: flex;
            align-items: center;
            gap: 0;
            margin: 1rem 0;
        }
        .timeline-step {
            flex: 1;
            text-align: center;
            position: relative;
        }
        .timeline-dot {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            margin: 0 auto 0.5rem;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.7rem;
            font-weight: 700;
            color: white;
        }
        .timeline-dot.complete { background: var(--green); }
        .timeline-dot.in-progress { background: var(--aws-orange); }
        .timeline-dot.not-started { background: #D5DBDB; color: var(--gray); }
        .timeline-line {
            position: absolute;
            top: 14px;
            left: 50%;
            width: 100%;
            height: 3px;
            z-index: -1;
        }
        .timeline-line.complete { background: var(--green); }
        .timeline-line.pending { background: #D5DBDB; }
        .timeline-label {
            font-size: 0.7rem;
            color: var(--gray);
            line-height: 1.3;
        }

        /* Filter bar */
        .filter-bar {
            display: flex;
            gap: 0.75rem;
            margin-bottom: 1rem;
            flex-wrap: wrap;
            align-items: center;
        }
        .filter-bar select, .filter-bar input {
            padding: 0.4rem 0.75rem;
            border: 1px solid var(--border);
            border-radius: 4px;
            font-size: 0.85rem;
            background: white;
        }
        .filter-bar label {
            font-size: 0.8rem;
            color: var(--gray);
            font-weight: 500;
        }

        /* Detail modal */
        .modal-overlay {
            display: none;
            position: fixed;
            top: 0; left: 0; right: 0; bottom: 0;
            background: rgba(0,0,0,0.5);
            z-index: 200;
            justify-content: center;
            align-items: flex-start;
            padding: 2rem;
            overflow-y: auto;
        }
        .modal-overlay.active { display: flex; }
        .modal {
            background: white;
            border-radius: 12px;
            max-width: 900px;
            width: 100%;
            max-height: 85vh;
            overflow-y: auto;
            padding: 2rem;
        }
        .modal-close {
            float: right;
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: var(--gray);
        }
        .modal h2 {
            font-size: 1.3rem;
            color: var(--aws-dark);
            margin-bottom: 1rem;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .main { padding: 1rem; }
            .kpi-row { grid-template-columns: repeat(2, 1fr); }
            .domain-grid { grid-template-columns: 1fr; }
        }

        /* Survey ranking items */
        .survey-rank-item {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            padding: 0.75rem 1rem;
            background: white;
            border: 1px solid var(--border);
            border-radius: 8px;
            margin-bottom: 0.5rem;
            transition: border-color 0.2s, box-shadow 0.2s;
        }
        .survey-rank-item:hover {
            border-color: var(--aws-blue);
        }
        .survey-rank-item.locked {
            background: #F7F8F8;
            border-color: var(--green);
        }
        .survey-rank-select {
            width: 60px;
            padding: 0.3rem;
            border: 1px solid var(--border);
            border-radius: 4px;
            font-size: 0.85rem;
            font-weight: 600;
            text-align: center;
        }
        .survey-rank-select.locked {
            background: #E6F4EA;
            color: var(--green);
            border-color: var(--green);
        }
        .survey-exp-name {
            font-weight: 600;
            font-size: 0.9rem;
            color: var(--aws-dark);
        }
        .survey-exp-desc {
            font-size: 0.75rem;
            color: var(--gray);
        }
        .survey-exp-icon {
            font-size: 1.3rem;
            width: 32px;
            text-align: center;
            flex-shrink: 0;
        }

        /* Roadmap task cards */
        .roadmap-phase-card {
            background: var(--card-bg);
            border: 1px solid var(--border);
            border-radius: 8px;
            margin-bottom: 1rem;
            overflow: hidden;
        }
        .roadmap-phase-header {
            padding: 0.75rem 1.25rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            cursor: pointer;
        }
        .roadmap-phase-header h3 {
            margin: 0;
            font-size: 0.95rem;
        }
        .roadmap-phase-body {
            padding: 0 1.25rem 1.25rem;
        }
        .roadmap-task {
            display: flex;
            align-items: flex-start;
            gap: 0.75rem;
            padding: 0.6rem 0;
            border-bottom: 1px solid #F1F3F4;
        }
        .roadmap-task:last-child { border-bottom: none; }
        .roadmap-task-check {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            border: 2px solid var(--border);
            flex-shrink: 0;
            margin-top: 2px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 0.65rem;
        }
        .roadmap-task-check.done {
            background: var(--green);
            border-color: var(--green);
            color: white;
        }
        .roadmap-task-check.in-progress {
            background: var(--aws-orange);
            border-color: var(--aws-orange);
            color: white;
        }
        .roadmap-task-content { flex: 1; }
        .roadmap-task-title {
            font-size: 0.85rem;
            font-weight: 500;
        }
        .roadmap-task-meta {
            font-size: 0.75rem;
            color: var(--gray);
            margin-top: 0.15rem;
        }
        .priority-rank-badge {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            width: 24px;
            height: 24px;
            border-radius: 50%;
            font-size: 0.7rem;
            font-weight: 700;
            color: white;
            flex-shrink: 0;
        }
        .priority-rank-1 { background: #D13212; }
        .priority-rank-2 { background: #FF9900; }
        .priority-rank-3 { background: #147EB4; }
        .priority-rank-4 { background: #687078; }
        .priority-rank-5 { background: #AAB7B8; }
        .priority-rank-6 { background: #D5DBDB; color: #687078; }
        .priority-rank-7 { background: #E8EAED; color: #687078; }
    </style>
</head>
<body>

<div class="header">
    <h1><span>Alchemy</span> Domain Onboarding Hub</h1>
    <div class="header-meta">WWPS Domains | Last Updated: July 1, 2025</div>
</div>

<div class="nav" id="nav">
    <button class="active" data-section="dashboard">Dashboard</button>
    <button data-section="domains">Domains</button>
    <button data-section="capabilities">Capabilities</button>
    <button data-section="integrations">Integration Experiences</button>
    <button data-section="phases">Phases & Milestones</button>
    <button data-section="salesplays">Sales Plays</button>
    <button data-section="survey">📋 Survey</button>
    <button data-section="roadmap">🗺️ Roadmap</button>
    <button data-section="about">About AEIM</button>
</div>

<div class="main" id="main">

    <!-- DASHBOARD -->
    <div class="section active" id="dashboard">
        <div class="kpi-row">
            <div class="kpi highlight">
                <div class="kpi-value">8</div>
                <div class="kpi-label">Total Domains</div>
            </div>
            <div class="kpi">
                <div class="kpi-value">7</div>
                <div class="kpi-label">In Progress</div>
            </div>
            <div class="kpi green">
                <div class="kpi-value">0</div>
                <div class="kpi-label">Complete</div>
            </div>
            <div class="kpi red">
                <div class="kpi-value">1</div>
                <div class="kpi-label">Not Started</div>
            </div>
            <div class="kpi highlight">
                <div class="kpi-value">31%</div>
                <div class="kpi-label">Avg Completion</div>
            </div>
        </div>

        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;">
            <div class="card">
                <h2>Domains by Completion</h2>
                <div id="completion-chart"></div>
            </div>
            <div class="card">
                <h2>Domains by Onboarding Phase</h2>
                <div id="phase-chart"></div>
            </div>
        </div>

        <div class="card">
            <h2>Sales Play Metrics</h2>
            <div class="kpi-row">
                <div class="kpi"><div class="kpi-value">17</div><div class="kpi-label">Total Sales Plays</div></div>
                <div class="kpi"><div class="kpi-value">8</div><div class="kpi-label">Draft</div></div>
                <div class="kpi"><div class="kpi-value">7</div><div class="kpi-label">Published</div></div>
                <div class="kpi"><div class="kpi-value">2</div><div class="kpi-label">Prioritized</div></div>
                <div class="kpi highlight"><div class="kpi-value">53%</div><div class="kpi-label">With OTE Tags</div></div>
                <div class="kpi"><div class="kpi-value">2,880</div><div class="kpi-label">Total Views</div></div>
            </div>
        </div>

        <div class="card">
            <h2>Domain Overview</h2>
            <div class="table-wrap">
                <table>
                    <thead>
                        <tr>
                            <th>Domain</th>
                            <th>GTM Lead</th>
                            <th>Current Phase</th>
                            <th>Completion</th>
                            <th>Status</th>
                        </tr>
                    </thead>
                    <tbody id="overview-table"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- DOMAINS -->
    <div class="section" id="domains">
        <div class="filter-bar">
            <label>Filter by Status:</label>
            <select id="domain-filter">
                <option value="all">All</option>
                <option value="In Progress">In Progress</option>
                <option value="Not Started">Not Started</option>
                <option value="Complete">Complete</option>
            </select>
        </div>
        <div class="domain-grid" id="domain-grid"></div>
    </div>

    <!-- CAPABILITIES -->
    <div class="section" id="capabilities">
        <div class="filter-bar">
            <label>Domain:</label>
            <select id="cap-domain-filter"></select>
            <label>Category:</label>
            <select id="cap-category-filter">
                <option value="all">All</option>
                <option value="PLANNING">Planning</option>
                <option value="AUTHORING">Authoring</option>
                <option value="IMPACT / INSIGHT">Impact / Insight</option>
                <option value="DISCOVERY">Discovery</option>
            </select>
        </div>
        <div class="card">
            <div class="table-wrap">
                <table>
                    <thead>
                        <tr>
                            <th>Domain</th>
                            <th>Category</th>
                            <th>Capability</th>
                            <th>Effort</th>
                            <th>Status</th>
                        </tr>
                    </thead>
                    <tbody id="cap-table"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- INTEGRATION EXPERIENCES -->
    <div class="section" id="integrations">
        <div class="filter-bar">
            <label>Domain:</label>
            <select id="int-domain-filter"></select>
        </div>
        <div class="card">
            <div class="table-wrap">
                <table>
                    <thead>
                        <tr>
                            <th>Domain</th>
                            <th>Experience</th>
                            <th>Priority</th>
                            <th>Status</th>
                            <th>Progress</th>
                            <th>% Complete</th>
                        </tr>
                    </thead>
                    <tbody id="int-table"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- PHASES & MILESTONES -->
    <div class="section" id="phases">
        <div id="phases-content"></div>
    </div>

    <!-- SALES PLAYS -->
    <div class="section" id="salesplays">
        <div class="filter-bar">
            <label>Domain:</label>
            <select id="sp-domain-filter"></select>
            <label>Status:</label>
            <select id="sp-status-filter">
                <option value="all">All</option>
                <option value="Draft">Draft</option>
                <option value="Published">Published</option>
                <option value="Prioritized">Prioritized</option>
            </select>
        </div>
        <div class="card">
            <div class="table-wrap">
                <table>
                    <thead>
                        <tr>
                            <th>Domain</th>
                            <th>Sales Play</th>
                            <th>Owner</th>
                            <th>Status</th>
                            <th>Views</th>
                            <th>Downloads</th>
                            <th>OTE Tag</th>
                        </tr>
                    </thead>
                    <tbody id="sp-table"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- SURVEY -->
    <div class="section" id="survey">
        <div class="card" style="max-width:800px;">
            <h2>🗳️ Integration Experience Priority Survey</h2>
            <p style="color:var(--gray);margin-bottom:1.5rem;">
                Welcome! This survey helps us understand which Alchemy integration experiences matter most to your domain.
                Rank the 7 experiences by dragging them into your preferred order, or use the dropdowns. Your #1 priority
                will drive your personalized onboarding roadmap.
            </p>

            <div style="margin-bottom:1.5rem;">
                <label style="font-weight:600;display:block;margin-bottom:0.5rem;">Select Your Domain</label>
                <select id="survey-domain" style="padding:0.5rem 0.75rem;border:1px solid var(--border);border-radius:6px;font-size:0.9rem;width:100%;max-width:400px;">
                    <option value="">— Choose your domain —</option>
                </select>
            </div>

            <div style="margin-bottom:1.5rem;">
                <label style="font-weight:600;display:block;margin-bottom:0.5rem;">Your Name / Alias</label>
                <input type="text" id="survey-name" placeholder="e.g. J. Smith" style="padding:0.5rem 0.75rem;border:1px solid var(--border);border-radius:6px;font-size:0.9rem;width:100%;max-width:400px;">
            </div>

            <div style="margin-bottom:1.5rem;">
                <label style="font-weight:600;display:block;margin-bottom:0.75rem;">Rank Integration Experiences by Priority</label>
                <p style="font-size:0.8rem;color:var(--gray);margin-bottom:0.75rem;">
                    Use the dropdowns to assign a rank (1 = highest priority). Each rank can only be used once.
                    "Start in Alchemy" is always required as a prerequisite and is pre-set.
                </p>
                <div id="survey-rankings"></div>
            </div>

            <div style="margin-bottom:1.5rem;">
                <label style="font-weight:600;display:block;margin-bottom:0.5rem;">Additional Context (optional)</label>
                <textarea id="survey-notes" rows="3" placeholder="Any specific goals, blockers, or context about your domain's GTM motion..." style="padding:0.5rem 0.75rem;border:1px solid var(--border);border-radius:6px;font-size:0.85rem;width:100%;resize:vertical;font-family:inherit;"></textarea>
            </div>

            <div style="display:flex;gap:1rem;align-items:center;">
                <button id="survey-submit" onclick="submitSurvey()" style="background:var(--aws-orange);color:white;border:none;padding:0.65rem 2rem;border-radius:6px;font-size:0.9rem;font-weight:600;cursor:pointer;transition:background 0.2s;">
                    Submit & Generate Roadmap
                </button>
                <span id="survey-status" style="font-size:0.85rem;color:var(--green);display:none;">✓ Submitted! View your roadmap →</span>
            </div>
        </div>

        <div class="card" style="max-width:800px;" id="survey-history-card" style="display:none;">
            <h2>📊 Survey Submissions</h2>
            <div class="table-wrap">
                <table>
                    <thead>
                        <tr><th>Domain</th><th>Submitted By</th><th>Date</th><th>#1 Priority</th><th>#2 Priority</th><th>#3 Priority</th><th>Actions</th></tr>
                    </thead>
                    <tbody id="survey-history-table"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- ROADMAP -->
    <div class="section" id="roadmap">
        <div class="filter-bar">
            <label>Domain:</label>
            <select id="roadmap-domain-filter"></select>
        </div>

        <div id="roadmap-empty" class="card" style="text-align:center;padding:3rem;">
            <div style="font-size:2rem;margin-bottom:0.5rem;">🗺️</div>
            <h2 style="margin-bottom:0.5rem;">No Roadmap Generated Yet</h2>
            <p style="color:var(--gray);">Submit a priority survey for a domain to generate its personalized onboarding roadmap.</p>
            <button onclick="navigateTo('survey')" style="margin-top:1rem;background:var(--aws-orange);color:white;border:none;padding:0.5rem 1.5rem;border-radius:6px;font-size:0.85rem;cursor:pointer;">
                Go to Survey →
            </button>
        </div>

        <div id="roadmap-content" style="display:none;">
            <div class="card">
                <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:1rem;">
                    <div>
                        <h2 id="roadmap-title" style="margin-bottom:0.25rem;"></h2>
                        <p id="roadmap-subtitle" style="font-size:0.85rem;color:var(--gray);"></p>
                    </div>
                    <div id="roadmap-priority-badges"></div>
                </div>
            </div>

            <div id="roadmap-phases"></div>
        </div>
    </div>

    <!-- ABOUT AEIM -->
    <div class="section" id="about">
        <div class="card">
            <h2>Alchemy Experience Integrations Menu (AEIM)</h2>
            <p style="margin-bottom:1rem; color: var(--gray);">An à la carte onboarding approach for WWSO domains</p>

            <h3>Problem</h3>
            <p style="margin-bottom:1rem;">The original phased onboarding approach provided a comprehensive white-glove experience but resulted in tool fatigue and overwhelm. Domains faced an overly large to-do list that created barriers to adoption rather than enabling it. The one-size-fits-all methodology did not account for varying team priorities, resource constraints, or readiness levels.</p>

            <h3>Solution</h3>
            <p style="margin-bottom:1rem;">The AEIM provides an à la carte experience, allowing domains to hand-select which integrations they want to prioritize. This enables a more customized onboarding experience with reduced overhead while maintaining the ability to scale through use of the Alchemy Agent.</p>

            <h3>Onboarding Flow</h3>
            <div style="display: grid; grid-template-columns: repeat(5, 1fr); gap: 0.5rem; margin: 1rem 0;">
                <div style="background: var(--aws-dark); color: white; padding: 0.75rem; border-radius: 6px; text-align: center;">
                    <div style="font-size: 1.5rem; font-weight: 700;">1</div>
                    <div style="font-size: 0.75rem;">Survey &<br>Interest</div>
                </div>
                <div style="background: var(--aws-light); color: white; padding: 0.75rem; border-radius: 6px; text-align: center;">
                    <div style="font-size: 1.5rem; font-weight: 700;">2</div>
                    <div style="font-size: 0.75rem;">Roadmap<br>Generation</div>
                </div>
                <div style="background: var(--aws-blue); color: white; padding: 0.75rem; border-radius: 6px; text-align: center;">
                    <div style="font-size: 1.5rem; font-weight: 700;">3</div>
                    <div style="font-size: 0.75rem;">Working<br>Session</div>
                </div>
                <div style="background: var(--aws-orange); color: white; padding: 0.75rem; border-radius: 6px; text-align: center;">
                    <div style="font-size: 1.5rem; font-weight: 700;">4</div>
                    <div style="font-size: 0.75rem;">30-Day<br>Audit</div>
                </div>
                <div style="background: var(--green); color: white; padding: 0.75rem; border-radius: 6px; text-align: center;">
                    <div style="font-size: 1.5rem; font-weight: 700;">5</div>
                    <div style="font-size: 0.75rem;">Quarterly<br>Review</div>
                </div>
            </div>

            <h3>6 Integration Experiences</h3>
            <div style="display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 0.75rem; margin: 1rem 0;">
                <div class="card" style="margin:0; border-left: 4px solid var(--aws-orange);">
                    <strong style="font-size: 0.85rem;">Start in Alchemy</strong>
                    <p style="font-size: 0.75rem; color: var(--gray); margin-top: 0.25rem;">Establish domain, grant access, orientation</p>
                </div>
                <div class="card" style="margin:0; border-left: 4px solid var(--aws-blue);">
                    <strong style="font-size: 0.85rem;">Field Experiences</strong>
                    <p style="font-size: 0.75rem; color: var(--gray); margin-top: 0.25rem;">AWSentral, DataBook, Highspot integrations</p>
                </div>
                <div class="card" style="margin:0; border-left: 4px solid var(--green);">
                    <strong style="font-size: 0.85rem;">Customer Experiences</strong>
                    <p style="font-size: 0.75rem; color: var(--gray); margin-top: 0.25rem;">AWS.com, Solutions Library, Console</p>
                </div>
                <div class="card" style="margin:0; border-left: 4px solid #9C27B0;">
                    <strong style="font-size: 0.85rem;">Partner Experiences</strong>
                    <p style="font-size: 0.75rem; color: var(--gray); margin-top: 0.25rem;">Partner Central, Competency Program</p>
                </div>
                <div class="card" style="margin:0; border-left: 4px solid var(--red);">
                    <strong style="font-size: 0.85rem;">GTM Performance Mgmt</strong>
                    <p style="font-size: 0.75rem; color: var(--gray); margin-top: 0.25rem;">VP dashboards, OP1 tracking, TAS</p>
                </div>
                <div class="card" style="margin:0; border-left: 4px solid var(--yellow);">
                    <strong style="font-size: 0.85rem;">Cross-Domain Collaboration</strong>
                    <p style="font-size: 0.75rem; color: var(--gray); margin-top: 0.25rem;">Sales play mapping, subscriber model</p>
                </div>
            </div>

            <h3>15 Alchemy Capabilities</h3>
            <div class="table-wrap" style="margin-top: 0.5rem;">
                <table>
                    <thead>
                        <tr><th>Category</th><th>Capability</th><th>Effort</th><th>Integrations</th></tr>
                    </thead>
                    <tbody>
                        <tr><td><span class="cap-category cap-planning">Planning</span></td><td>Demand Planning</td><td><span class="badge effort-ai">AI</span></td><td>All</td></tr>
                        <tr><td><span class="cap-category cap-planning">Planning</span></td><td>Vteam Management</td><td><span class="badge effort-low">Low</span></td><td>All</td></tr>
                        <tr><td><span class="cap-category cap-planning">Planning</span></td><td>Taxonomy Refresh</td><td><span class="badge effort-medium">Medium</span></td><td>All</td></tr>
                        <tr><td><span class="cap-category cap-authoring">Authoring</span></td><td>BOM Automation</td><td><span class="badge effort-medium">Medium</span></td><td>All</td></tr>
                        <tr><td><span class="cap-category cap-authoring">Authoring</span></td><td>Sales Play Creation</td><td><span class="badge effort-high">High</span></td><td>All</td></tr>
                        <tr><td><span class="cap-category cap-authoring">Authoring</span></td><td>Solution Response / Demo / Workshop Mapping</td><td><span class="badge effort-medium">Medium</span></td><td>Cross-Domain, Field/Customer/Partner</td></tr>
                        <tr><td><span class="cap-category cap-authoring">Authoring</span></td><td>Cross-Domain Feedback Loop</td><td><span class="badge effort-ai">AI</span></td><td>Cross-Domain Collaboration</td></tr>
                        <tr><td><span class="cap-category cap-impact">Impact</span></td><td>OTE Tagging Reporting</td><td><span class="badge effort-ai">AI</span></td><td>SMG Ops & STT Dashboard</td></tr>
                        <tr><td><span class="cap-category cap-impact">Impact</span></td><td>Alchemy Agent</td><td><span class="badge effort-ai">AI</span></td><td>All</td></tr>
                        <tr><td><span class="cap-category cap-impact">Impact</span></td><td>Integration into SMG Ops & STT Dashboards</td><td><span class="badge effort-ai">AI</span></td><td>All</td></tr>
                        <tr><td><span class="cap-category cap-discovery">Discovery</span></td><td>OTE Tag Creation</td><td><span class="badge effort-ai">AI</span></td><td>SMG Ops & STT Dashboard</td></tr>
                        <tr><td><span class="cap-category cap-discovery">Discovery</span></td><td>Publishing to Solutions Library</td><td><span class="badge effort-medium">Medium</span></td><td>Customer Experiences</td></tr>
                        <tr><td><span class="cap-category cap-discovery">Discovery</span></td><td>Cross-Domain Sales Play Mapping</td><td><span class="badge effort-medium">Medium/AI</span></td><td>Cross-Domain, Field</td></tr>
                        <tr><td><span class="cap-category cap-discovery">Discovery</span></td><td>Prioritization of Sales Plays</td><td><span class="badge effort-low">Low</span></td><td>Field Experiences</td></tr>
                        <tr><td><span class="cap-category cap-discovery">Discovery</span></td><td>Inclusion in AI-Powered Experiences</td><td><span class="badge">Varies</span></td><td>All</td></tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>

</div>

<!-- Domain Detail Modal -->
<div class="modal-overlay" id="modal-overlay">
    <div class="modal" id="modal-content"></div>
</div>

<script>
// ============================================================
// ALCHEMY DOMAIN ONBOARDING HUB — DATA & INTERACTIVITY
// ============================================================

const domains = [
    { name: "WWPS - Defense & Intelligence", short: "D&I", gtmLead: "J. Smith", builders: "L. Garcia, P. Lee", startDate: "2025-06-01", phase: "Phase 2: Roadmap Generation", completion: 15, status: "In Progress" },
    { name: "WWPS - Federal Civilian", short: "Fed Civ", gtmLead: "A. Johnson", builders: "C. Wilson", startDate: "2025-05-15", phase: "Phase 3: Working Session", completion: 35, status: "In Progress" },
    { name: "WWPS - State & Local Government", short: "S&L Gov", gtmLead: "M. Williams", builders: "D. Taylor, J. Thomas", startDate: "2025-05-01", phase: "Phase 4: 30-Day Audit", completion: 50, status: "In Progress" },
    { name: "WWPS - Education (K-12)", short: "K-12", gtmLead: "R. Brown", builders: "E. Jackson", startDate: "2025-06-10", phase: "Phase 1: Initial Interest & Survey", completion: 10, status: "In Progress" },
    { name: "WWPS - Higher Education", short: "Higher Ed", gtmLead: "K. Davis", builders: "F. White, H. Harris", startDate: "2025-04-15", phase: "Phase 5: Quarterly Business Review", completion: 65, status: "In Progress" },
    { name: "WWPS - Healthcare & Life Sciences", short: "Health", gtmLead: "S. Martinez", builders: "B. Martin", startDate: "2025-05-10", phase: "Phase 3: Working Session", completion: 45, status: "In Progress" },
    { name: "WWPS - Nonprofit", short: "Nonprofit", gtmLead: "", builders: "", startDate: "", phase: "Not Started", completion: 0, status: "Not Started" },
    { name: "WWPS - Space & Aerospace", short: "Space", gtmLead: "T. Anderson", builders: "N. Clark", startDate: "2025-06-05", phase: "Phase 2: Roadmap Generation", completion: 25, status: "In Progress" }
];

const capabilityNames = [
    { name: "Demand Planning (Gap Analysis, Overlap Audit, Strategic Partnerships)", category: "PLANNING", effort: "AI" },
    { name: "Vteam Management", category: "PLANNING", effort: "Low" },
    { name: "Taxonomy Refresh", category: "PLANNING", effort: "Medium" },
    { name: "BOM Automation", category: "AUTHORING", effort: "Medium" },
    { name: "Sales Play Creation", category: "AUTHORING", effort: "High" },
    { name: "Solution Response / Demo / Workshop Mapping", category: "AUTHORING", effort: "Medium" },
    { name: "Cross-Domain Feedback Loop", category: "AUTHORING", effort: "AI" },
    { name: "OTE Tagging Reporting", category: "IMPACT / INSIGHT", effort: "AI" },
    { name: "Alchemy Agent", category: "IMPACT / INSIGHT", effort: "AI" },
    { name: "Integration into SMG Ops & STT Dashboards", category: "IMPACT / INSIGHT", effort: "AI" },
    { name: "OTE Tag Creation", category: "DISCOVERY", effort: "AI" },
    { name: "Publishing to Solutions Library & Marketing Pages", category: "DISCOVERY", effort: "Medium" },
    { name: "Cross-Domain Sales Play Mapping & Subscribing", category: "DISCOVERY", effort: "Medium/AI" },
    { name: "Prioritization of Sales Plays", category: "DISCOVERY", effort: "Low" },
    { name: "Inclusion in AI-Powered Experiences", category: "DISCOVERY", effort: "Varies" }
];

// Capability statuses per domain (index matches domains array, inner index matches capabilityNames)
const capStatuses = {
    "WWPS - Defense & Intelligence": ["In Progress","In Progress","In Progress","In Progress","In Progress","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started"],
    "WWPS - Federal Civilian": ["In Progress","In Progress","In Progress","In Progress","In Progress","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started"],
    "WWPS - State & Local Government": ["In Progress","In Progress","In Progress","In Progress","In Progress","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started"],
    "WWPS - Education (K-12)": ["Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started"],
    "WWPS - Higher Education": ["Complete","Complete","Complete","Complete","Complete","Complete","Complete","Complete","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started"],
    "WWPS - Healthcare & Life Sciences": ["Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started"],
    "WWPS - Nonprofit": ["Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started"],
    "WWPS - Space & Aerospace": ["Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started","Not Started"]
};

const integrationExperiences = [
    { exp: "Start in Alchemy", totalActions: 3 },
    { exp: "Field Experiences", totalActions: 7 },
    { exp: "Customer Experiences", totalActions: 4 },
    { exp: "Partner Experiences", totalActions: 3 },
    { exp: "GTM Performance Management", totalActions: 4 },
    { exp: "Cross-Domain Collaboration", totalActions: 3 },
    { exp: "Content Automation", totalActions: 5 }
];

const intData = {
    "WWPS - Defense & Intelligence": [{p:"High",s:"In Progress",c:2},{p:"High",s:"In Progress",c:2},{p:"High",s:"In Progress",c:2},{p:"Medium",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0}],
    "WWPS - Federal Civilian": [{p:"High",s:"In Progress",c:2},{p:"High",s:"In Progress",c:2},{p:"High",s:"In Progress",c:2},{p:"Medium",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0}],
    "WWPS - State & Local Government": [{p:"High",s:"In Progress",c:2},{p:"High",s:"In Progress",c:2},{p:"High",s:"In Progress",c:2},{p:"Medium",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0}],
    "WWPS - Education (K-12)": [{p:"High",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0}],
    "WWPS - Higher Education": [{p:"High",s:"Complete",c:3},{p:"High",s:"Complete",c:7},{p:"High",s:"Complete",c:4},{p:"Medium",s:"Complete",c:3},{p:"High",s:"Complete",c:4},{p:"Medium",s:"Complete",c:3},{p:"Medium",s:"Complete",c:5}],
    "WWPS - Healthcare & Life Sciences": [{p:"High",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0}],
    "WWPS - Nonprofit": [{p:"High",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0}],
    "WWPS - Space & Aerospace": [{p:"High",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"High",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0},{p:"Medium",s:"Not Started",c:0}]
};

const phaseData = {
    "WWPS - Defense & Intelligence": [{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"In Progress",start:"2025-06-15",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""}],
    "WWPS - Federal Civilian": [{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"In Progress",start:"2025-06-15",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""}],
    "WWPS - State & Local Government": [{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"In Progress",start:"2025-06-15",end:""},{s:"Not Started",start:"",end:""}],
    "WWPS - Education (K-12)": [{s:"In Progress",start:"2025-06-15",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""}],
    "WWPS - Higher Education": [{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"In Progress",start:"2025-06-15",end:""}],
    "WWPS - Healthcare & Life Sciences": [{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"In Progress",start:"2025-06-15",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""}],
    "WWPS - Nonprofit": [{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""}],
    "WWPS - Space & Aerospace": [{s:"Complete",start:"2025-05-01",end:"2025-06-15"},{s:"In Progress",start:"2025-06-15",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""},{s:"Not Started",start:"",end:""}]
};

const salesPlays = [
    { domain: "WWPS - Defense & Intelligence", name: "Zero Trust Architecture for DoD", owner: "L. Garcia", status: "Published", views: 245, downloads: 89, ote: "Yes" },
    { domain: "WWPS - Defense & Intelligence", name: "Cloud Migration for IC Workloads", owner: "P. Lee", status: "Draft", views: 0, downloads: 0, ote: "No" },
    { domain: "WWPS - Defense & Intelligence", name: "AI/ML for Defense Analytics", owner: "L. Garcia", status: "Draft", views: 0, downloads: 0, ote: "No" },
    { domain: "WWPS - Federal Civilian", name: "FedRAMP High Compliance", owner: "C. Wilson", status: "Prioritized", views: 512, downloads: 203, ote: "Yes" },
    { domain: "WWPS - Federal Civilian", name: "Citizen Services Modernization", owner: "C. Wilson", status: "Published", views: 178, downloads: 67, ote: "Yes" },
    { domain: "WWPS - State & Local Government", name: "Smart City Data Platform", owner: "D. Taylor", status: "Published", views: 334, downloads: 122, ote: "Yes" },
    { domain: "WWPS - State & Local Government", name: "Emergency Response Cloud", owner: "J. Thomas", status: "Draft", views: 45, downloads: 12, ote: "No" },
    { domain: "WWPS - State & Local Government", name: "Constituent Services Portal", owner: "D. Taylor", status: "Published", views: 201, downloads: 78, ote: "Yes" },
    { domain: "WWPS - Education (K-12)", name: "Virtual Learning Infrastructure", owner: "E. Jackson", status: "Draft", views: 0, downloads: 0, ote: "No" },
    { domain: "WWPS - Higher Education", name: "Research Computing at Scale", owner: "F. White", status: "Prioritized", views: 423, downloads: 187, ote: "Yes" },
    { domain: "WWPS - Higher Education", name: "Student Success Analytics", owner: "H. Harris", status: "Published", views: 289, downloads: 95, ote: "Yes" },
    { domain: "WWPS - Higher Education", name: "Campus Network Modernization", owner: "F. White", status: "Published", views: 156, downloads: 54, ote: "Yes" },
    { domain: "WWPS - Higher Education", name: "Hybrid Learning Platform", owner: "H. Harris", status: "Draft", views: 32, downloads: 8, ote: "No" },
    { domain: "WWPS - Healthcare & Life Sciences", name: "HIPAA Compliant Data Lake", owner: "B. Martin", status: "Published", views: 398, downloads: 156, ote: "Yes" },
    { domain: "WWPS - Healthcare & Life Sciences", name: "Clinical Trial Data Management", owner: "B. Martin", status: "Draft", views: 67, downloads: 23, ote: "No" },
    { domain: "WWPS - Space & Aerospace", name: "Satellite Data Processing", owner: "N. Clark", status: "Draft", views: 0, downloads: 0, ote: "No" },
    { domain: "WWPS - Space & Aerospace", name: "Ground Station as a Service", owner: "N. Clark", status: "Draft", views: 0, downloads: 0, ote: "No" }
];

// ============================================================
// UTILITY FUNCTIONS
// ============================================================

function statusBadge(status) {
    const map = {
        "Complete": "badge-green",
        "In Progress": "badge-yellow",
        "Not Started": "badge-red",
        "Draft": "badge-gray",
        "Published": "badge-green",
        "Prioritized": "badge-blue"
    };
    return `<span class="badge ${map[status] || 'badge-gray'}">${status}</span>`;
}

function effortBadge(effort) {
    const map = { "AI": "effort-ai", "Low": "effort-low", "Medium": "effort-medium", "High": "effort-high" };
    return `<span class="badge ${map[effort] || ''}">${effort}</span>`;
}

function categoryBadge(cat) {
    const map = { "PLANNING": "cap-planning", "AUTHORING": "cap-authoring", "IMPACT / INSIGHT": "cap-impact", "DISCOVERY": "cap-discovery" };
    const labels = { "PLANNING": "Planning", "AUTHORING": "Authoring", "IMPACT / INSIGHT": "Impact", "DISCOVERY": "Discovery" };
    return `<span class="cap-category ${map[cat]}">${labels[cat] || cat}</span>`;
}

function progressBar(pct, color) {
    const c = color || (pct >= 75 ? '#1B9C3F' : pct >= 40 ? '#FF9900' : pct > 0 ? '#D4A017' : '#D5DBDB');
    return `<div class="progress-bar"><div class="progress-fill" style="width:${pct}%;background:${c}"></div></div><div class="progress-text">${pct}%</div>`;
}

function getPhaseNumber(phaseStr) {
    const m = phaseStr.match(/Phase (\d)/);
    return m ? parseInt(m[1]) : 0;
}

// ============================================================
// NAVIGATION
// ============================================================

document.querySelectorAll('.nav button').forEach(btn => {
    btn.addEventListener('click', () => {
        document.querySelectorAll('.nav button').forEach(b => b.classList.remove('active'));
        document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
        btn.classList.add('active');
        document.getElementById(btn.dataset.section).classList.add('active');
    });
});

// ============================================================
// DASHBOARD
// ============================================================

function renderOverviewTable() {
    const tbody = document.getElementById('overview-table');
    tbody.innerHTML = domains.map(d => `
        <tr>
            <td><strong>${d.name}</strong></td>
            <td>${d.gtmLead || '—'}</td>
            <td>${d.phase}</td>
            <td>${progressBar(d.completion)}</td>
            <td>${statusBadge(d.status)}</td>
        </tr>
    `).join('');
}

function renderCompletionChart() {
    const container = document.getElementById('completion-chart');
    const maxVal = Math.max(...domains.map(d => d.completion), 1);
    container.innerHTML = domains.map(d => {
        const w = (d.completion / 100) * 100;
        const color = d.completion >= 60 ? '#1B9C3F' : d.completion >= 30 ? '#FF9900' : d.completion > 0 ? '#D4A017' : '#D5DBDB';
        return `
            <div style="display:flex;align-items:center;margin-bottom:0.5rem;">
                <div style="width:100px;font-size:0.8rem;color:#687078;flex-shrink:0;">${d.short}</div>
                <div style="flex:1;height:20px;background:#E8EAED;border-radius:3px;overflow:hidden;margin-right:0.5rem;">
                    <div style="height:100%;width:${w}%;background:${color};border-radius:3px;transition:width 0.5s;"></div>
                </div>
                <div style="width:35px;font-size:0.8rem;font-weight:600;text-align:right;">${d.completion}%</div>
            </div>
        `;
    }).join('');
}

function renderPhaseChart() {
    const container = document.getElementById('phase-chart');
    const phases = ["Phase 1", "Phase 2", "Phase 3", "Phase 4", "Phase 5", "Not Started"];
    const counts = phases.map(p => domains.filter(d => d.phase.startsWith(p) || (p === "Not Started" && d.phase === "Not Started")).length);
    const colors = ['#232F3E', '#37475A', '#147EB4', '#FF9900', '#1B9C3F', '#D5DBDB'];
    const max = Math.max(...counts, 1);

    container.innerHTML = phases.map((p, i) => `
        <div style="display:flex;align-items:center;margin-bottom:0.5rem;">
            <div style="width:100px;font-size:0.8rem;color:#687078;flex-shrink:0;">${p}</div>
            <div style="flex:1;height:20px;background:#E8EAED;border-radius:3px;overflow:hidden;margin-right:0.5rem;">
                <div style="height:100%;width:${(counts[i]/max)*100}%;background:${colors[i]};border-radius:3px;"></div>
            </div>
            <div style="width:20px;font-size:0.8rem;font-weight:600;text-align:right;">${counts[i]}</div>
        </div>
    `).join('');
}

// ============================================================
// DOMAIN CARDS
// ============================================================

function renderDomainGrid(filter) {
    const grid = document.getElementById('domain-grid');
    const filtered = filter === 'all' ? domains : domains.filter(d => d.status === filter);

    grid.innerHTML = filtered.map((d, i) => {
        const capStatus = capStatuses[d.name] || [];
        const capComplete = capStatus.filter(s => s === "Complete").length;
        const capInProgress = capStatus.filter(s => s === "In Progress").length;

        return `
            <div class="domain-card" onclick="showDomainDetail('${d.name}')">
                <div class="domain-card-header">
                    <div class="domain-card-title">${d.name.replace('WWPS - ', '')}</div>
                    ${statusBadge(d.status)}
                </div>
                <div class="domain-card-meta">
                    ${d.gtmLead ? `GTM Lead: ${d.gtmLead}` : 'No GTM Lead assigned'}
                    ${d.builders ? ` · Builders: ${d.builders}` : ''}
                </div>
                <div class="domain-card-phase">${d.phase}</div>
                <div style="margin-top:0.75rem;">
                    ${progressBar(d.completion)}
                </div>
                <div style="margin-top:0.75rem;display:flex;gap:1rem;font-size:0.75rem;color:#687078;">
                    <span>✅ ${capComplete}/15 capabilities</span>
                    <span>🔄 ${capInProgress} in progress</span>
                </div>
            </div>
        `;
    }).join('');
}

document.getElementById('domain-filter').addEventListener('change', (e) => {
    renderDomainGrid(e.target.value);
});

// ============================================================
// DOMAIN DETAIL MODAL
// ============================================================

function showDomainDetail(domainName) {
    const d = domains.find(x => x.name === domainName);
    if (!d) return;

    const caps = capStatuses[domainName] || [];
    const ints = intData[domainName] || [];
    const phases = phaseData[domainName] || [];
    const plays = salesPlays.filter(sp => sp.domain === domainName);
    const phaseLabels = ["Survey", "Roadmap", "Working Session", "30-Day Audit", "QBR"];

    let html = `
        <button class="modal-close" onclick="closeModal()">&times;</button>
        <h2>${d.name.replace('WWPS - ', '')}</h2>
        <div style="display:flex;gap:1rem;flex-wrap:wrap;margin-bottom:1rem;">
            ${statusBadge(d.status)}
            <span style="font-size:0.85rem;color:#687078;">GTM Lead: ${d.gtmLead || '—'}</span>
            <span style="font-size:0.85rem;color:#687078;">Builders: ${d.builders || '—'}</span>
            <span style="font-size:0.85rem;color:#687078;">Started: ${d.startDate || '—'}</span>
        </div>

        <div style="margin-bottom:1.5rem;">
            <strong style="font-size:0.9rem;">Overall Progress: ${d.completion}%</strong>
            ${progressBar(d.completion)}
        </div>

        <h3 style="margin-bottom:0.5rem;">Onboarding Phases</h3>
        <div class="timeline" style="margin-bottom:1.5rem;">
            ${phases.map((p, i) => `
                <div class="timeline-step">
                    ${i < 4 ? `<div class="timeline-line ${p.s === 'Complete' ? 'complete' : 'pending'}"></div>` : ''}
                    <div class="timeline-dot ${p.s === 'Complete' ? 'complete' : p.s === 'In Progress' ? 'in-progress' : 'not-started'}">${i+1}</div>
                    <div class="timeline-label">${phaseLabels[i]}<br>${p.s === 'Complete' ? '✓' : p.s === 'In Progress' ? '⏳' : '—'}</div>
                </div>
            `).join('')}
        </div>

        <h3 style="margin-bottom:0.5rem;">Capability Adoption (${caps.filter(s=>s==='Complete').length}/15 complete)</h3>
        <table style="margin-bottom:1.5rem;">
            <thead><tr><th>Category</th><th>Capability</th><th>Effort</th><th>Status</th></tr></thead>
            <tbody>
                ${capabilityNames.map((c, i) => `
                    <tr>
                        <td>${categoryBadge(c.category)}</td>
                        <td>${c.name}</td>
                        <td>${effortBadge(c.effort)}</td>
                        <td>${statusBadge(caps[i] || 'Not Started')}</td>
                    </tr>
                `).join('')}
            </tbody>
        </table>

        <h3 style="margin-bottom:0.5rem;">Integration Experiences</h3>
        <table style="margin-bottom:1.5rem;">
            <thead><tr><th>Experience</th><th>Priority</th><th>Status</th><th>Progress</th></tr></thead>
            <tbody>
                ${integrationExperiences.map((ie, i) => {
                    const data = ints[i] || {p:'—',s:'Not Started',c:0};
                    const pct = Math.round((data.c / ie.totalActions) * 100);
                    return `
                        <tr>
                            <td>${ie.exp}</td>
                            <td>${data.p}</td>
                            <td>${statusBadge(data.s)}</td>
                            <td>${progressBar(pct)} <span style="font-size:0.75rem;color:#687078;">${data.c}/${ie.totalActions}</span></td>
                        </tr>
                    `;
                }).join('')}
            </tbody>
        </table>
    `;

    if (plays.length > 0) {
        html += `
            <h3 style="margin-bottom:0.5rem;">Sales Plays (${plays.length})</h3>
            <table>
                <thead><tr><th>Sales Play</th><th>Owner</th><th>Status</th><th>Views</th><th>OTE</th></tr></thead>
                <tbody>
                    ${plays.map(sp => `
                        <tr>
                            <td>${sp.name}</td>
                            <td>${sp.owner}</td>
                            <td>${statusBadge(sp.status)}</td>
                            <td>${sp.views.toLocaleString()}</td>
                            <td>${sp.ote === 'Yes' ? '<span class="badge badge-green">Yes</span>' : '<span class="badge badge-gray">No</span>'}</td>
                        </tr>
                    `).join('')}
                </tbody>
            </table>
        `;
    } else {
        html += `<p style="color:#687078;font-size:0.85rem;">No sales plays entered yet.</p>`;
    }

    document.getElementById('modal-content').innerHTML = html;
    document.getElementById('modal-overlay').classList.add('active');
}

function closeModal() {
    document.getElementById('modal-overlay').classList.remove('active');
}

document.getElementById('modal-overlay').addEventListener('click', (e) => {
    if (e.target === e.currentTarget) closeModal();
});

document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape') closeModal();
});

// ============================================================
// CAPABILITIES TABLE
// ============================================================

function populateDomainFilters() {
    const options = '<option value="all">All Domains</option>' + domains.map(d => `<option value="${d.name}">${d.name.replace('WWPS - ', '')}</option>`).join('');
    ['cap-domain-filter', 'int-domain-filter', 'sp-domain-filter'].forEach(id => {
        const el = document.getElementById(id);
        if (el) el.innerHTML = options;
    });
}

function renderCapTable() {
    const domainFilter = document.getElementById('cap-domain-filter').value;
    const catFilter = document.getElementById('cap-category-filter').value;
    const tbody = document.getElementById('cap-table');

    let rows = [];
    const filteredDomains = domainFilter === 'all' ? domains : domains.filter(d => d.name === domainFilter);

    filteredDomains.forEach(d => {
        const caps = capStatuses[d.name] || [];
        capabilityNames.forEach((c, i) => {
            if (catFilter !== 'all' && c.category !== catFilter) return;
            rows.push(`
                <tr>
                    <td>${d.name.replace('WWPS - ', '')}</td>
                    <td>${categoryBadge(c.category)}</td>
                    <td>${c.name}</td>
                    <td>${effortBadge(c.effort)}</td>
                    <td>${statusBadge(caps[i] || 'Not Started')}</td>
                </tr>
            `);
        });
    });

    tbody.innerHTML = rows.join('');
}

document.getElementById('cap-domain-filter').addEventListener('change', renderCapTable);
document.getElementById('cap-category-filter').addEventListener('change', renderCapTable);

// ============================================================
// INTEGRATION EXPERIENCES TABLE
// ============================================================

function renderIntTable() {
    const domainFilter = document.getElementById('int-domain-filter').value;
    const tbody = document.getElementById('int-table');
    const filteredDomains = domainFilter === 'all' ? domains : domains.filter(d => d.name === domainFilter);

    let rows = [];
    filteredDomains.forEach(d => {
        const ints = intData[d.name] || [];
        integrationExperiences.forEach((ie, i) => {
            const data = ints[i] || {p:'—',s:'Not Started',c:0};
            const pct = Math.round((data.c / ie.totalActions) * 100);
            rows.push(`
                <tr>
                    <td>${d.name.replace('WWPS - ', '')}</td>
                    <td>${ie.exp}</td>
                    <td>${data.p}</td>
                    <td>${statusBadge(data.s)}</td>
                    <td>${progressBar(pct)}</td>
                    <td>${data.c}/${ie.totalActions}</td>
                </tr>
            `);
        });
    });

    tbody.innerHTML = rows.join('');
}

document.getElementById('int-domain-filter').addEventListener('change', renderIntTable);

// ============================================================
// PHASES & MILESTONES
// ============================================================

function renderPhases() {
    const container = document.getElementById('phases-content');
    const phaseLabels = ["Phase 1: Survey", "Phase 2: Roadmap", "Phase 3: Working Session", "Phase 4: 30-Day Audit", "Phase 5: QBR"];

    container.innerHTML = domains.map(d => {
        const phases = phaseData[d.name] || [];
        return `
            <div class="card">
                <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:0.75rem;">
                    <h3 style="margin:0;">${d.name.replace('WWPS - ', '')}</h3>
                    ${statusBadge(d.status)}
                </div>
                <div class="timeline">
                    ${phases.map((p, i) => `
                        <div class="timeline-step">
                            ${i < 4 ? `<div class="timeline-line ${p.s === 'Complete' ? 'complete' : 'pending'}"></div>` : ''}
                            <div class="timeline-dot ${p.s === 'Complete' ? 'complete' : p.s === 'In Progress' ? 'in-progress' : 'not-started'}">${i+1}</div>
                            <div class="timeline-label">${phaseLabels[i].replace(/Phase \d: /, '')}<br>
                                <span style="font-size:0.65rem;">${p.s === 'Complete' ? '✓ Done' : p.s === 'In Progress' ? '⏳ Active' : '—'}</span>
                            </div>
                        </div>
                    `).join('')}
                </div>
                ${d.name === 'WWPS - Nonprofit' ? '<div style="font-size:0.8rem;color:#D13212;margin-top:0.5rem;">⚠️ Awaiting domain GTM Lead assignment</div>' : ''}
            </div>
        `;
    }).join('');
}

// ============================================================
// SALES PLAYS TABLE
// ============================================================

function renderSalesPlays() {
    const domainFilter = document.getElementById('sp-domain-filter').value;
    const statusFilter = document.getElementById('sp-status-filter').value;
    const tbody = document.getElementById('sp-table');

    let filtered = salesPlays;
    if (domainFilter !== 'all') filtered = filtered.filter(sp => sp.domain === domainFilter);
    if (statusFilter !== 'all') filtered = filtered.filter(sp => sp.status === statusFilter);

    tbody.innerHTML = filtered.map(sp => `
        <tr>
            <td>${sp.domain.replace('WWPS - ', '')}</td>
            <td><strong>${sp.name}</strong></td>
            <td>${sp.owner}</td>
            <td>${statusBadge(sp.status)}</td>
            <td>${sp.views.toLocaleString()}</td>
            <td>${sp.downloads.toLocaleString()}</td>
            <td>${sp.ote === 'Yes' ? '<span class="badge badge-green">Yes</span>' : '<span class="badge badge-gray">No</span>'}</td>
        </tr>
    `).join('');
}

document.getElementById('sp-domain-filter').addEventListener('change', renderSalesPlays);
document.getElementById('sp-status-filter').addEventListener('change', renderSalesPlays);

// ============================================================
// INITIALIZE
// ============================================================

populateDomainFilters();
renderOverviewTable();
renderCompletionChart();
renderPhaseChart();
renderDomainGrid('all');
renderCapTable();
renderIntTable();
renderPhases();
renderSalesPlays();


// ============================================================
// SURVEY & ROADMAP SYSTEM
// ============================================================

// Integration experience definitions with tasks from AEIM doc
const experienceDefinitions = [
    {
        key: "start",
        name: "Start in Alchemy",
        icon: "🚀",
        color: "#FF9900",
        description: "Establish domain, grant access, and orient your team to the Alchemy platform.",
        required: true,
        tasks: [
            { title: "Establish domain within Alchemy", effort: "Low", detail: "Create or verify your domain entity exists in Alchemy" },
            { title: "Identify key stakeholders and grant access by role", effort: "Low", detail: "Map GTM Lead, Sales Play Builders, BDs, and SA Leads to Alchemy roles" },
            { title: "Complete Alchemy & Use Case Mental Model orientation", effort: "Low", detail: "Watch orientation video and review the Alchemy wiki" }
        ]
    },
    {
        key: "field",
        name: "Field Experiences",
        icon: "📊",
        color: "#147EB4",
        description: "Integrate with AWSentral, DataBook, Highspot, and planned tools like Account Planning and Field Advisor.",
        tasks: [
            { title: "Create and bar raise Solution Area", effort: "Medium", detail: "Define solution areas that map to your domain's GTM motion" },
            { title: "Create and bar raise Use Cases", effort: "Medium", detail: "Build use cases aligned to customer problems your domain solves" },
            { title: "Establish Sales Play entity (standalone or linked to Use Case/Solution Area)", effort: "High", detail: "Create sales plays that the field can discover and act on" },
            { title: "Map relevant solution responses to Use Cases", effort: "Medium", detail: "Connect demos, workshops, and solution responses to use cases" },
            { title: "Create and publish SFDC/OTE tag at Use Case and/or Sales Play level", effort: "AI", detail: "Enable opportunity tagging for field attribution" },
            { title: "Indicate Sales Play as 'Prioritized' in Alchemy", effort: "Low", detail: "Flag key plays for field focus and visibility" },
            { title: "Verify integrations in AWSentral Account & Opportunity Recommendations", effort: "Low", detail: "Confirm your content surfaces in AWSentral recommendations" }
        ]
    },
    {
        key: "customer",
        name: "Customer Experiences",
        icon: "👥",
        color: "#1B9C3F",
        description: "Surface content on AWS.com Marketing Pages, Solutions Library, Console, and Documentation Site.",
        tasks: [
            { title: "Create and bar raise Solution Area", effort: "Medium", detail: "Define solution areas for customer-facing content" },
            { title: "Create and bar raise Use Case", effort: "Medium", detail: "Build use cases that map to customer journeys" },
            { title: "Map solution responses to Use Cases", effort: "Medium", detail: "Connect solution responses for customer discovery" },
            { title: "Request Use Case publication on Solutions Library", effort: "Medium", detail: "Submit for publication on AWS Solutions Library and marketing pages" }
        ]
    },
    {
        key: "partner",
        name: "Partner Experiences",
        icon: "🤝",
        color: "#9C27B0",
        description: "Integrate with Partner Central Listing Experience and AWS Specialization & Competency Program.",
        tasks: [
            { title: "Create and bar raise Solution Area", effort: "Medium", detail: "Define solution areas relevant to partner ecosystem" },
            { title: "Create and bar raise Use Case", effort: "Medium", detail: "Build use cases that partners can align to" },
            { title: "Map partner solution responses to Use Cases", effort: "Medium", detail: "Connect partner solutions to your domain's use cases" }
        ]
    },
    {
        key: "gtm-perf",
        name: "GTM Performance Management",
        icon: "📈",
        color: "#D13212",
        description: "Power VP-level dashboards, OP1 initiative tracking, and Total Addressable Spend attainment.",
        tasks: [
            { title: "Create and publish SFDC/OTE tag at Use Case level", effort: "AI", detail: "Enable opportunity tracking for performance reporting" },
            { title: "Include existing Campaign Codes (Automated Integration)", effort: "AI", detail: "Link campaign codes for attribution in dashboards" },
            { title: "Indicate Sales Play as 'Prioritized' in Alchemy", effort: "Low", detail: "Flag plays for inclusion in performance dashboards" },
            { title: "Reference Dashboard Overview Video", effort: "Low", detail: "Review the Industry Business Performance Dashboard and TAS Attainment views" }
        ]
    },
    {
        key: "cross-domain",
        name: "Cross-Domain Collaboration",
        icon: "🔗",
        color: "#D4A017",
        description: "Enable cross-domain sales play mapping, subscriber model, and agent-assisted recommendations.",
        tasks: [
            { title: "Create Sales Play within Alchemy", effort: "High", detail: "Establish plays that can be mapped across domains" },
            { title: "Use agent to identify matching Use Cases and/or domains", effort: "AI", detail: "Leverage Alchemy Agent to find cross-domain opportunities from a Solution Response or Sales Play" },
            { title: "Request to map Sales Play/Solution Responses to inter-domain Use Cases", effort: "Medium", detail: "Submit mapping requests via feedback tool or agent" }
        ]
    },
    {
        key: "content-auto",
        name: "Content Automation",
        icon: "⚡",
        color: "#37475A",
        description: "Auto-generate BOM, First Call Decks, email templates, and publish to Highspot via KnowledgeMine.",
        tasks: [
            { title: "Establish Sales Play entity within Alchemy", effort: "High", detail: "Create the sales play that BOM will be generated for" },
            { title: "Create and bar raise Use Cases and Solution Areas linked to the Sales Play", effort: "Medium", detail: "Ensure use cases and solution areas are linked for BOM context" },
            { title: "Provide a prompt to the Alchemy Agent to initiate BOM generation", effort: "AI", detail: "Use the agent to auto-generate a complete Bill of Materials" },
            { title: "Review and approve AI-generated BOM and content assets", effort: "Medium", detail: "Quality check generated First Call Decks, email templates, etc." },
            { title: "Publish generated content to KnowledgeMine and Highspot", effort: "Medium", detail: "Push finalized assets to downstream field tools" }
        ]
    }
];

// Store survey submissions (persisted in localStorage)
let surveySubmissions = JSON.parse(localStorage.getItem('alchemySurveys') || '{}');

function initSurvey() {
    // Populate domain dropdown
    const select = document.getElementById('survey-domain');
    domains.forEach(d => {
        const opt = document.createElement('option');
        opt.value = d.name;
        opt.textContent = d.name.replace('WWPS - ', '');
        select.appendChild(opt);
    });

    // Populate roadmap domain filter
    const roadmapFilter = document.getElementById('roadmap-domain-filter');
    roadmapFilter.innerHTML = '<option value="">— Select a domain —</option>';
    domains.forEach(d => {
        const opt = document.createElement('option');
        opt.value = d.name;
        opt.textContent = d.name.replace('WWPS - ', '');
        if (surveySubmissions[d.name]) opt.textContent += ' ✓';
        roadmapFilter.appendChild(opt);
    });

    renderSurveyRankings();
    renderSurveyHistory();
}

function renderSurveyRankings() {
    const container = document.getElementById('survey-rankings');
    container.innerHTML = experienceDefinitions.map((exp, i) => {
        const isLocked = exp.required;
        return `
            <div class="survey-rank-item ${isLocked ? 'locked' : ''}" data-exp-key="${exp.key}">
                <select class="survey-rank-select ${isLocked ? 'locked' : ''}" 
                        id="rank-${exp.key}" 
                        ${isLocked ? 'disabled' : ''}
                        onchange="validateRanks()">
                    ${isLocked 
                        ? '<option value="0">Pre</option>' 
                        : '<option value="">—</option>' + [1,2,3,4,5,6].map(n => `<option value="${n}">${n}</option>`).join('')
                    }
                </select>
                <div class="survey-exp-icon">${exp.icon}</div>
                <div style="flex:1;">
                    <div class="survey-exp-name">${exp.name}</div>
                    <div class="survey-exp-desc">${exp.description}</div>
                </div>
                ${isLocked ? '<span class="badge badge-green">Required</span>' : ''}
            </div>
        `;
    }).join('');
}

function validateRanks() {
    const selects = experienceDefinitions
        .filter(e => !e.required)
        .map(e => document.getElementById(`rank-${e.key}`));
    
    const usedValues = selects.map(s => s.value).filter(v => v !== '');
    const hasDuplicates = usedValues.length !== new Set(usedValues).size;
    
    selects.forEach(s => {
        if (s.value && usedValues.filter(v => v === s.value).length > 1) {
            s.style.borderColor = '#D13212';
        } else {
            s.style.borderColor = '';
        }
    });

    return !hasDuplicates && usedValues.length > 0;
}

function submitSurvey() {
    const domainName = document.getElementById('survey-domain').value;
    const submitterName = document.getElementById('survey-name').value.trim();
    const notes = document.getElementById('survey-notes').value.trim();

    if (!domainName) {
        alert('Please select your domain.');
        return;
    }
    if (!submitterName) {
        alert('Please enter your name or alias.');
        return;
    }

    // Collect rankings
    const rankings = {};
    let hasAtLeastOne = false;
    experienceDefinitions.forEach(exp => {
        if (exp.required) {
            rankings[exp.key] = 0; // prerequisite
        } else {
            const val = document.getElementById(`rank-${exp.key}`).value;
            if (val) {
                rankings[exp.key] = parseInt(val);
                hasAtLeastOne = true;
            }
        }
    });

    if (!hasAtLeastOne) {
        alert('Please rank at least your top priority experience.');
        return;
    }

    if (!validateRanks()) {
        alert('Each rank number can only be used once. Please fix duplicates.');
        return;
    }

    // Save submission
    surveySubmissions[domainName] = {
        submitter: submitterName,
        date: new Date().toISOString().split('T')[0],
        rankings: rankings,
        notes: notes
    };
    localStorage.setItem('alchemySurveys', JSON.stringify(surveySubmissions));

    // Update integration experience priorities in the main data
    applyPrioritiesToDomain(domainName, rankings);

    // Show success
    const status = document.getElementById('survey-status');
    status.style.display = 'inline';
    status.innerHTML = '✓ Submitted! <a href="#" onclick="navigateTo(\'roadmap\');selectRoadmapDomain(\'' + domainName + '\');return false;" style="color:var(--aws-blue);">View your roadmap →</a>';

    renderSurveyHistory();
    renderIntTable();

    // Update roadmap filter to show checkmark
    initSurvey();
}

function applyPrioritiesToDomain(domainName, rankings) {
    // Map experience keys to integration experience indices
    const keyToIndex = {
        "start": 0, "field": 1, "customer": 2, "partner": 3,
        "gtm-perf": 4, "cross-domain": 5, "content-auto": 6
    };

    const domainInts = intData[domainName];
    if (!domainInts) return;

    Object.entries(rankings).forEach(([key, rank]) => {
        const idx = keyToIndex[key];
        if (idx !== undefined && domainInts[idx]) {
            if (rank === 0) {
                domainInts[idx].p = "Required";
            } else if (rank <= 2) {
                domainInts[idx].p = "High";
            } else if (rank <= 4) {
                domainInts[idx].p = "Medium";
            } else {
                domainInts[idx].p = "Low";
            }
        }
    });
}

function renderSurveyHistory() {
    const tbody = document.getElementById('survey-history-table');
    const card = document.getElementById('survey-history-card');
    const entries = Object.entries(surveySubmissions);

    if (entries.length === 0) {
        card.style.display = 'none';
        return;
    }
    card.style.display = 'block';

    tbody.innerHTML = entries.map(([domainName, sub]) => {
        const sorted = Object.entries(sub.rankings)
            .filter(([k, v]) => v > 0)
            .sort((a, b) => a[1] - b[1]);
        const top3 = sorted.slice(0, 3).map(([k, v]) => {
            const exp = experienceDefinitions.find(e => e.key === k);
            return exp ? `${v}. ${exp.name}` : '';
        });

        return `
            <tr>
                <td><strong>${domainName.replace('WWPS - ', '')}</strong></td>
                <td>${sub.submitter}</td>
                <td>${sub.date}</td>
                <td>${top3[0] || '—'}</td>
                <td>${top3[1] || '—'}</td>
                <td>${top3[2] || '—'}</td>
                <td>
                    <button onclick="navigateTo('roadmap');selectRoadmapDomain('${domainName}');" 
                            style="background:var(--aws-blue);color:white;border:none;padding:0.3rem 0.75rem;border-radius:4px;font-size:0.75rem;cursor:pointer;">
                        View Roadmap
                    </button>
                </td>
            </tr>
        `;
    }).join('');
}

// ============================================================
// ROADMAP GENERATION
// ============================================================

function selectRoadmapDomain(domainName) {
    document.getElementById('roadmap-domain-filter').value = domainName;
    renderRoadmap(domainName);
}

document.getElementById('roadmap-domain-filter').addEventListener('change', function() {
    renderRoadmap(this.value);
});

function renderRoadmap(domainName) {
    const emptyState = document.getElementById('roadmap-empty');
    const content = document.getElementById('roadmap-content');

    if (!domainName || !surveySubmissions[domainName]) {
        emptyState.style.display = 'block';
        content.style.display = 'none';
        return;
    }

    emptyState.style.display = 'none';
    content.style.display = 'block';

    const sub = surveySubmissions[domainName];
    const domain = domains.find(d => d.name === domainName);
    const rankings = sub.rankings;

    // Title
    document.getElementById('roadmap-title').textContent = 
        `${domainName.replace('WWPS - ', '')} — Personalized Onboarding Roadmap`;
    document.getElementById('roadmap-subtitle').textContent = 
        `Generated from survey submitted by ${sub.submitter} on ${sub.date}`;

    // Priority badges
    const sorted = Object.entries(rankings)
        .filter(([k, v]) => v > 0)
        .sort((a, b) => a[1] - b[1]);
    
    document.getElementById('roadmap-priority-badges').innerHTML = sorted.map(([key, rank]) => {
        const exp = experienceDefinitions.find(e => e.key === key);
        return `<span class="priority-rank-badge priority-rank-${rank}" title="#${rank} Priority">${rank}</span>
                <span style="font-size:0.8rem;margin-right:0.75rem;">${exp ? exp.name : key}</span>`;
    }).join('');

    // Build roadmap phases: prerequisite first, then by priority rank
    const phasesContainer = document.getElementById('roadmap-phases');
    let html = '';

    // Phase 0: Prerequisites (Start in Alchemy)
    const startExp = experienceDefinitions.find(e => e.key === 'start');
    const domainInts = intData[domainName] || [];
    const startStatus = domainInts[0] || { s: 'Not Started', c: 0 };

    html += buildRoadmapPhaseCard(
        'Prerequisite',
        startExp,
        startStatus,
        0,
        domainName
    );

    // Phases 1-N: By priority rank
    sorted.forEach(([key, rank]) => {
        const exp = experienceDefinitions.find(e => e.key === key);
        if (!exp) return;

        const keyToIndex = {
            "start": 0, "field": 1, "customer": 2, "partner": 3,
            "gtm-perf": 4, "cross-domain": 5, "content-auto": 6
        };
        const idx = keyToIndex[key];
        const intStatus = domainInts[idx] || { s: 'Not Started', c: 0 };

        html += buildRoadmapPhaseCard(
            `Priority #${rank}`,
            exp,
            intStatus,
            rank,
            domainName
        );
    });

    // Unranked experiences
    const rankedKeys = new Set(Object.keys(rankings).filter(k => rankings[k] > 0));
    rankedKeys.add('start');
    const unranked = experienceDefinitions.filter(e => !rankedKeys.has(e.key));
    
    if (unranked.length > 0) {
        html += `<div class="card" style="background:#F7F8F8;">
            <h3 style="color:var(--gray);margin-bottom:0.5rem;">Not Prioritized</h3>
            <p style="font-size:0.8rem;color:var(--gray);margin-bottom:0.75rem;">
                These experiences were not ranked in the survey. They can be added later as your domain's GTM motion evolves.
            </p>
            <div style="display:flex;flex-wrap:wrap;gap:0.5rem;">
                ${unranked.map(e => `
                    <span style="background:white;border:1px solid var(--border);padding:0.3rem 0.75rem;border-radius:4px;font-size:0.8rem;">
                        ${e.icon} ${e.name}
                    </span>
                `).join('')}
            </div>
        </div>`;
    }

    phasesContainer.innerHTML = html;
}

function buildRoadmapPhaseCard(label, exp, intStatus, rank, domainName) {
    const totalTasks = exp.tasks.length;
    const completedTasks = intStatus.c || 0;
    const pct = Math.round((completedTasks / totalTasks) * 100);
    
    const headerBg = rank === 0 ? '#1B9C3F' : 
                     rank <= 2 ? '#D13212' : 
                     rank <= 4 ? '#FF9900' : '#687078';

    const capStatArr = capStatuses[domainName] || [];

    return `
        <div class="roadmap-phase-card">
            <div class="roadmap-phase-header" style="background:${headerBg};color:white;" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'">
                <h3 style="color:white;">
                    ${rank === 0 ? '✓' : `#${rank}`} ${exp.icon} ${label}: ${exp.name}
                </h3>
                <div style="display:flex;align-items:center;gap:0.75rem;">
                    ${statusBadge(intStatus.s === 'Complete' ? 'Complete' : intStatus.c > 0 ? 'In Progress' : 'Not Started')}
                    <span style="font-size:0.8rem;">${completedTasks}/${totalTasks} tasks</span>
                    <span style="font-size:1rem;">▼</span>
                </div>
            </div>
            <div class="roadmap-phase-body">
                <p style="font-size:0.8rem;color:var(--gray);margin-bottom:0.75rem;margin-top:0.75rem;">${exp.description}</p>
                ${progressBar(pct)}
                <div style="margin-top:0.75rem;">
                    ${exp.tasks.map((task, ti) => {
                        const isDone = ti < completedTasks;
                        const isNext = ti === completedTasks;
                        return `
                            <div class="roadmap-task">
                                <div class="roadmap-task-check ${isDone ? 'done' : isNext ? 'in-progress' : ''}">
                                    ${isDone ? '✓' : isNext ? '→' : ''}
                                </div>
                                <div class="roadmap-task-content">
                                    <div class="roadmap-task-title" style="${isDone ? 'text-decoration:line-through;color:var(--gray);' : isNext ? 'color:var(--aws-dark);font-weight:600;' : ''}">${task.title}</div>
                                    <div class="roadmap-task-meta">
                                        ${effortBadge(task.effort)} · ${task.detail}
                                    </div>
                                </div>
                            </div>
                        `;
                    }).join('')}
                </div>
            </div>
        </div>
    `;
}

// ============================================================
// NAVIGATION HELPER
// ============================================================

function navigateTo(sectionId) {
    document.querySelectorAll('.nav button').forEach(b => b.classList.remove('active'));
    document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
    const btn = document.querySelector(`.nav button[data-section="${sectionId}"]`);
    if (btn) btn.classList.add('active');
    document.getElementById(sectionId).classList.add('active');
}

// ============================================================
// INIT SURVEY ON LOAD
// ============================================================

initSurvey();

// Re-apply any saved survey priorities to intData on load
Object.entries(surveySubmissions).forEach(([domainName, sub]) => {
    applyPrioritiesToDomain(domainName, sub.rankings);
});
// Re-render integration table with updated priorities
renderIntTable();
</script>
</body>
</html>
