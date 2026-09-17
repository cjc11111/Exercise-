Perio Patient Cases

An interactive, AI-simulated patient bank for PERI 5704: Periodontics in General Practice at the Dental College of Georgia, Augusta University. Students interview a simulated patient, review clinical findings, and work up a diagnosis and treatment plan - individually or in groups - without an instructor present.

Live page: add your GitHub Pages URL here once deployed, e.g. https://yourname.github.io/perio-cases/

What this is
index.html is the entire application - a single, self-contained page with no build step. It includes:
15 fictional patient cases across five categories: diagnostic classification, non-surgical therapy, surgical therapy, implant therapy, and oral pathology
An interview tab where students chat with the simulated patient
A clinical findings tab (chart summary, radiographic findings, diagnostic tests) revealed separately from the interview
A group notes tab for a draft diagnosis and treatment plan, saved locally in the browser
Every case is entirely fictional - synthetic names, synthetic histories, built from general periodontics course content. No real patient data is used anywhere in this project.
How it works
Student's browser  --->  Cloudflare Worker  --->  Anthropic API
   (index.html)          (holds the API key)      (plays the patient)

The page itself never holds an API key. It sends the patient's instructions and the conversation so far to a Cloudflare Worker, which attaches the API key (kept as a private secret in Cloudflare) and forwards the request to Anthropic's API. This keeps the key out of the public page - anyone can view this repo's source, but the key itself is never in it.

Setup

Full step-by-step instructions (getting an Anthropic API key, deploying the Cloudflare Worker, publishing this page, and linking it in D2L) are in the deployment manual used to build this project. In short:

Get an Anthropic API key and set a monthly spending limit.
Deploy a Cloudflare Worker that holds the key as a secret and proxies requests to Anthropic.
In index.html, set WORKER_URL (near the top of the <script> block) to your Worker's address.
Push index.html to this repo and enable GitHub Pages (Settings -> Pages -> deploy from main / root).
In D2L, add a Link (not a file upload) pointing to the GitHub Pages URL. D2L strips <script> tags from uploaded HTML files, so the page must be linked, not uploaded directly.
Updating or adding cases

All case data lives in the CASES array near the top of the <script> section in index.html. Each case is one JavaScript object with:

Field	Purpose
section	Which sidebar group it appears under
name, demo, hook	Patient name, one-line demographics, and the teaser shown in the sidebar
opening	The patient's first message when a student opens the case
system	The instructions that keep the AI in character as the patient, including all the history facts it can reveal
findings	The clinical findings shown in the "Clinical findings" tab (as HTML)
radiographs, tests	Rows for the two findings tables

To add a case, copy an existing object, give it a new id, and edit the fields. No other code changes are needed - it will appear automatically in the sidebar under its section.

Privacy and data
All cases are fictional and built from general lecture content - this project does not contain or process any real patient information (no PHI/HIPAA scope).
Group notes are saved only in each student's own browser (localStorage) - not shared across devices and not collected anywhere.
Students and instructors should never enter real patient details into the chat or notes fields; keep it to the fictional cases as written.
Cost

Each student conversation costs a few cents in Anthropic API usage. Check the Usage page in the Anthropic Console periodically, especially after the first class session, and adjust the monthly spending limit if needed.

Maintainer

Celine Joyce Cornelius, Associate Professor, Department of Periodontics, Dental College of Georgia, Augusta University.
