Triage one Linear support issue and, when it is scoped, accept it into Todo and claim it through CodeFlow's support queue. Ported from the Devin `scope` skill and the work-item standards in rootlyhq/devin-automation, so Soil and Devin triage the same way.

Arguments: the issue key (e.g. OC-1234), and the support label for its team: `oc - automated` for On-Call (OC), `codeflow:ir-support` for Incident Response (IR). These are the labels CodeFlow's support queue watches.

## Preconditions

Stop and report the reason if any fail:
- The issue exists and you can read it.
- It is in Triage, Backlog or Todo.
- Nobody else is already on it: no open PR or lane attached, and no assignee other than the Soil bot who replied in the last day.

## Steps

1. Fetch the issue, its comments, attachments, linked issues and customer needs. Read the relevant code in the repository checkouts as needed to understand feasibility and surface area, and confirm the behaviour is real.
2. Classify it and apply exactly one type label:
   - "Production Bug" for defects in released functionality.
   - "Development Bug" for QA returns or defects behind a feature flag still in development.
   - "Feature" for new behaviour.
   - Chores get no type label.
3. Apply its product area label from the "Product Area" group.
4. Check it belongs to the team it is filed in. On-Call owns everything up to the point an incident is created (alerts and sources, routing, grouping, alert-triggered workflows, heartbeats, schedules, overrides, escalation policies, paging, live call routing, on-call notifications, on-call pay). Incident Response owns incidents after creation and the rest. If it belongs to the other team, comment one line saying so and why, and stop.
5. Rewrite the title and description to the work-item standards below. Preserve the author's intent and every fact, link, screenshot and customer reference already present.
6. Decide:
   - A fix that spans repositories (rootlyhq/rootly and rootlyhq/rootly-notification-system, say) is not a gap: it is scoped like any other. Soil runs it as one effort with a lane per repository, and the lanes keep each other in sync. Never hold work back, or leave it for a person, because it crosses repositories, services or the Rails/notification boundary.
   - No open questions, gaps or ambiguity: it is scoped. Set the 1-5 estimate, apply "Scoped" if the team has that label, and move it to Todo.
   - Otherwise: post one comment with the clarifying questions, addressed to the issue creator unless someone else clearly owns the answer. Do not set an estimate, do not move it, and stop.
7. Claim it: add the support label (keep every other label), assign it to yourself when you start, and move it to In Progress. If the support label does not exist in Linear, do not create it: say so in your report and go on.

## Work-item standards

- The description begins with a one-sentence user story or problem statement, followed by acceptance criteria for a feature, or experienced and expected behaviour (and steps to reproduce) for a bug.
- The title is a succinct version of the user story or problem statement, in the imperative voice if possible.
- Acceptance criteria are a list, each item a complete, easy-to-understand sentence with verifiable, observable output: it answers how someone notices it is true.
- Acceptance criteria include at least one Given-When-Then scenario. For new features, preferably 1 happy scenario, 3-7 edge or alternative scenarios and 2-5 negative, invalid or permission scenarios.
- Bugs use the workspace "Bug report" template and features the "Feature" template; fill every required field (for bugs: experienced behaviour, expected behaviour, steps to reproduce, a Product Area label, the affected Customer) and replace every template placeholder.
- The estimate is 1-5 points from XP's risk framework: Completeness (complete 0, incomplete 1, unknown 2), Volatility (low 0, medium 1, high 2), Complexity (simple 0, standard 1, complex 2). Never put the rationale for the estimate in the description.
- Linear comments are only for clarifying questions and answering questions; do not comment on work you have done.

## Finish

End with exactly one line:
- `OC-123 scoped at N points, claimed: <title> <issue url>`
- `OC-123 questions posted for <name>: <title> <issue url>`
- `OC-123 belongs to <team>: <issue url>`
- `OC-123 skipped, <reason>: <issue url>`
