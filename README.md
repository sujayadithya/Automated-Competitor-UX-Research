# Automated Competitor UX Research
 
![Automated Competitor UX Research](./automated-competitor-ux-research-thumbnail.jpg)
 
This project runs a competitor review of one product flow at a time. You describe your product once. Then, in Cursor chat, you name a flow. The agent finds competitors, walks the flow on the desktop web, scores what it sees, and writes a report.
 
## Prerequisites
 
Connect these MCP servers in Cursor before you run `/research`. Without them, the command fails partway through instead of finishing a report.
 
- **Playwright** — opens each competitor and steps through the flow
- **Fetcher** — reads public pricing, help, and changelog pages
- **Filesystem** — saves screenshots and the HTML report
- **Memory** — stores scores so a later run of the same flow can say what changed
**Paper** is optional. When it is connected, the same report is also built there. The HTML report is still written if Paper is not connected.
 
## Get started
 
1. Open this folder in Cursor.
2. Fill in [`context.md`](context.md). That file is the product brief the agent reads before every run. If a section is still a placeholder, the agent will ask you one question at a time and write your answers in before it researches anything.
3. Skim [`rubric.md`](rubric.md). Those 1–5 definitions are the only scores the agent is allowed to use. Edit them if a criterion does not match your product. Leave the file as-is to use the defaults: Info Density, Steps-to-Value, Error Clarity, and Edge-Case Handling.
## Run a review
 
In Cursor chat, type:
 
```text
/research add to cart
```
 
Replace `add to cart` with the flow you care about. You can narrow it:
 
```text
/research add to cart, focus on minimum order quantity
```
 
The agent will:
 
- Search for 6–8 competitors in the category from `context.md`, skip anyone listed under Exclude, and always include anyone listed under Include.
- Post that list in chat and continue. It does not wait for you to approve the list.
- Open each product on the desktop web, step through the flow, and save screenshots under `research/[flow]-[date]/[competitor]/`.
- If a site is behind a login, CAPTCHA, or paywall, mark that competitor “not captured” and move on. It will not invent screenshots or scores for a site it could not open.
- Score every captured competitor against `rubric.md`, with a one-line reason for each score.
- Write `research/[flow]-[date]/report.html` and, when Paper is connected, the same report in Paper.
Open the HTML file in a browser. The summary is at the top. The full screenshot set is in a collapsed section at the bottom.
 
## Example output
 
A finished run of `/research add to cart` is saved in [`examples/add-to-cart-2026-10-12/`](examples/add-to-cart-2026-10-12/). Open its `report.html` in a browser to see what a report looks like.
 
## Run it again
 
Run `/research` with the same flow name later. If a previous run of that flow is in Memory, the new report adds a “What changed since last run” section under the summary. Reports stay in the same shape so you can compare them.
 
## What you can change
 
| File                           | What it controls                                                                                            |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| `context.md`                   | Product, users, design principles, constraints, competitors to include or exclude                           |
| `rubric.md`                    | What gets scored, and what a 1, 3, and 5 mean                                                               |
| `output-template.md`           | The sections every report must include                                                                      |
| `.cursor/commands/research.md` | The steps the `/research` command runs                                                                      |
| `.cursorrules`                 | How a run behaves: blocked competitors, screenshot names, progress updates, and when to ask versus continue |
 
Change `context.md` or `rubric.md` before a run when the product or the scorecard changes. Leave `output-template.md` alone if you want reports to stay comparable.