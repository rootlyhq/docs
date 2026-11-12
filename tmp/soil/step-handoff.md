# Handoff: from-john-dri-rootly-agent-brain-for-the-homepag, step 4/5 "verdict" (verdict: awaiting the human's remediation decision)

This is a FRESH session for this step. The harness wrote this brief from the lane's records; the earlier steps' conversation does not carry over. Open the files under "Read on demand" only when this step needs their detail.

- Workflow: investigate, lane "Create purple Rootly Agent analytics artifact"
- Steps: triage (done) → hypotheses (done) → investigation (done) → verdict (NOW) → handoff (next)
- The run advanced here from "hypotheses".

## Original ask (verbatim; the intent restates it, this is what was asked)
> ok lets start with the data first, lets create an artifact needs to be world class when it comes to visuals/data visualization and rich signal using purple color palete. We should have all the data from metabase and braintrust. go
- the operator (operator) relayed it as: From John (DRI, Rootly Agent "brain" for the homepage redesign): "ok lets start with the data first, lets create an artifact needs to be world class when it comes to visuals/data visualization and rich signal using purple color palette. We should have all the data from metabase and braintrust. go"

Context: in #proj-homepage-redesign (https://rootly-workspace.slack.com/archives/C0BJWDHTDF1/p1794433210058639) Nour (PM) asked John for data to size a 3-tier KR for the Rootly Agent, which is moving out of the incident into a global component (homepage + anywhere in Rootly; John wants Slack too: "1 brain, multiple surfaces"). Tiers: (1) the agent fully resolves the question on its own, (2) it points to help-center docs / self-serve steps, (3) everything else, handed back to the user. PRD: https://app.notion.com/p/rootly/Draft-Homepage-Redesign-4145a77a0aca8024a01bea3babd81498

Pull, as read-only:
A. Braintrust — project rootly-prod-agent (id 8f790d74-eb83-4f65-962d-24527bb4666d), last 30–90 days: volume over time; surface/entry point (Slack mention, in-incident, web, etc.); what people ask (cluster user prompts into intents — e.g. summarize/catch-up, why was I paged, who's on call, handoff/role changes, action items, status updates, metrics questions, config/admin, off-topic); per intent, how often the agent answered vs declined/deflected (guardrail refusals — off-topic and empty-context rules in config/ai/guardrails.yml) vs errored; latency (p50/p95) and any quality scores present. Also check rootly-prod-ai-sre only if it helps the "AI SRE on homepage" question.
B. Metabase — homepage traffic by persona (the PRD cites 27.8k views/30d, 23% of active users), Rootly Agent usage (users, accounts, messages), and the most frequent MANUAL actions users take in the web app (acknowledge alert, request cover, post status update, create/close action item, assign roles, edit schedules, create workflows, etc.) with counts — to estimate which ones the agent could take over.
C. Map the PRD's 11 persona example prompts (Catch me up, What's overdue, Draft a retro, Who's on call, Why was I paged, Cover my shift, Handover brief, Which alerts page me most, Overdue retros by team, Schedule gaps, Set up a SEV1 workflow) to today's demand and today's answer rate.

Deliverable: a claude.ai artifact (HTML) — world-class data visualization, purple palette (light + dark theme), high signal: headline KPIs (deflection rate, answer rate, latency), tier sizing for Nour's KR with a proposed realistic target, intent treemap/bar, deflection by intent, trend lines, surface split, manual-action leaderboard with "agent-takeover candidate" flags, and a clear methodology + data-gaps section. Never invent numbers; if a source is unavailable, say so in the page. No customer names or PII in the page. Publish it and report the link.

## Operator and owner guidance so far (still binding)
- [triage] operator:op-77bd8e664c9c: Note: you were attached to the docs repo. If you need the agent code (e.g. config/ai/guardrails.yml or how refusals/deflections are tagged in logs), attach rootlyhq/rootly. From the operator session, Braintrust queries were blocked by a read-only hook and Metabase wasn't available. If either fails for you too, say so in the artifact's data-gaps section and in your report. Don't estimate the missing numbers.

## Gate results by head
- triage: passed at c823cbd991 (2026-11-12T10:49:50.614Z)
- hypotheses: passed at c823cbd991 (2026-11-12T10:50:13.376Z)
- investigation: passed at c823cbd991 (2026-11-12T10:50:50.528Z)

## Environment
environment: your organization's environment, every included repo's services available (docs is not in it: its checkout and toolchain only); nothing is started: mcp__environment__env_start brings up only the services the task needs (env_services lists them).

## Read on demand
- tmp/soil/hypotheses.json
- tmp/soil/incident.json
- git diff c823cbd991af9bf11ba829ad52076221808564dd...HEAD (the full change)
- tmp/soil/step-handoff.md (this brief)


### What the previous step reported
I wrote five candidates to `tmp/soil/candidates.json`. None has been investigated yet; the next step does that.

- **h1:** Home chat has no write tools for declaring an incident, paging, status page, retros, Slack channel actions or Jira, because `home_write_allowlist.rb` leaves them out. With no tool to call, the agent replies in plain text that it can't.
- **h2:** Web incident chat is read-only on purpose. Its write tools are filtered out and the prompt tells the agent to say so. That would explain why it refuses incident updates, paging and retros 33%, 40% and 60% of the time.
- **h3:** Incident updates are the most-refused action (34 teams). The cause may be that the `enable_ai_assistant_required_fields_v2` flag is off for most paying teams, which keeps the update tool out of the agent's tools.
- **h4:** Many refusals may happen even though the tool exists. Either the user lacks the permission (logged as denied, permission_denied or held), or the confirmation step expires; 1,119 confirmations expired in 90 days.
- **h5:** The guardrails in `config/ai/guardrails.yml` (the off-topic and empty-context rules) may fire on legitimate action requests, so the agent declines even when the right tool is available.

One thing to know: `incident.json` from triage is about why the agent refuses actions users ask for. That is narrower than the original ask, which was the purple analytics artifact sizing Nour's 3-tier KR from Metabase and Braintrust data. These candidates follow `incident.json`, so they don't cover the rest of that brief.