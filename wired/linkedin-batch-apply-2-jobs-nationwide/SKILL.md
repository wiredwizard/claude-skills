---
name: linkedin-batch-apply-2-jobs-nationwide
description: |
  Automate applying to 2 remote LinkedIn jobs in one go nationwide. Use this skill whenever the user wants to apply to multiple LinkedIn positions at once—especially for n8n, AI Automation, workflow automation, or similar remote roles. The skill navigates LinkedIn's Easy Apply flow, auto-fills all form fields with resume data and standard answers, handles validation errors, creates appropriate headlines from resume content, uploads the profile picture when needed, and completes all 2 applications in sequence. Perfect for job search batching and saving hours on repetitive form filling. Always applies only to fully remote positions nationwide and avoids Resolv Global. Triggers on phrases like "apply to jobs", "batch apply", "apply to 2 positions", "find and apply to n8n jobs", "automation engineer jobs".
compatibility: Chrome browser with LinkedIn access, Claude with form interaction tools
---

## Overview

This skill automates the process of finding and applying to 2 LinkedIn remote job positions nationwide. It handles the entire Easy Apply workflow including form filling, validation, multi-step navigation, and submission. The skill uses embedded standard answers and adapts them intelligently based on job requirements extracted from the job posting.

## How It Works

### Step 1: Navigate to Job Search
- Open LinkedIn job search with filters for:
  - Keywords: **N8N** or **AI Automation Engineer** (or user-specified keywords)
  - Location: Remote only (nationwide, not limited by geography)
  - Easy Apply enabled
  - Salary range: $95,000+ (based on standard expectations)
- Exclude: Resolv Global positions and hybrid/on-site roles

### Step 2: Identify 2 Qualifying Jobs
- Scroll through results
- Check each listing for:
  - ✅ Remote designation (not hybrid, not on-site)
  - ✅ Easy Apply available
  - ✅ Not from Resolv Global
  - ✅ Matches n8n/AI Automation criteria
- Stop when 2 qualifying positions found

### Step 3: Apply to Each Job
For each position, complete the Easy Apply form:

#### Basic Information (Auto-filled)
- **Name:** Morgan Todd
- **Email:** prplwiredwizard@gmail.com
- **Phone:** 919-590-0954
- **Location:** Harrisburg, Pennsylvania
- **LinkedIn:** https://www.linkedin.com/in/wiredwizard/
- **Job Title/Headline:** Auto-generated from resume based on job requirements (e.g., "AI Prompt Engineer | Generative AI | Workflow Automation")

#### Work History
- Auto-populated from LinkedIn profile (10 positions from current back to 1996)
- Most recent: Lewistown Liquidators LLC (Oct 2024–Present)

#### Education
- 3 entries auto-populated from LinkedIn (MIT, Vanderbilt, Center for Media Arts NY)

#### Skills & Experience

**For positions requiring AI/ML skills:**
- AI/Automation experience: **5+ years**
- Familiarity with Claude, GPT, LLM-based tools
- Answer to "AI tools usage": *"I am an advanced AI user and have directly used AI to completely systemize actions and workflows."*

**For positions requiring programming skills:**
- Python: **15-20 years** (standard answer)
- SQL: **15-20 years**
- JavaScript, other languages: **15-20 years**

**For work authorization questions:**
- Legally authorized to work: **Yes**
- Require sponsorship: **No**
- Eligible without visa: **Yes**
- Can hold secondary employment: **No** (due to conflict with primary employer)

#### Demographic Questions
- Age 18+: **Yes**
- Work authorization: **Yes**
- Visa sponsorship needed: **No**
- Disability: **No**
- Race/Ethnicity: **White (Not Hispanic or Latino)** (auto-selected if available)
- Gender: **Male** (auto-selected if available)
- Sexual Orientation: **Heterosexual** (if asked)

#### Salary & Compensation
- Annual salary expectation: **$95,000**
- Willing to relocate: **No** (remote position)

#### Profile Picture
- Upload **MorgaIT.png** when form requests profile image

### Step 4: Adaptive Answering for Custom Questions

When a form contains questions not in the standard set:

1. **Extract job requirements** from the job posting (required skills, technologies, years of experience)
2. **Match to resume content** — identify relevant experience from work history
3. **Formulate best answer** that:
   - Honestly reflects resume experience
   - Demonstrates fit for the specific role
   - Emphasizes automation, n8n, AI, workflow optimization, or similar relevant skills
4. **If no strong match exists** for a critical question:
   - Provide generic professional answer (e.g., "Passionate about building scalable automation solutions")
   - If still unclear, move to next job (don't submit incomplete/weak application)

**Examples of adaptive answers:**

*Question: "Describe your experience with workflow automation"*
- **Answer:** "I've designed and deployed n8n workflows to automate repetitive business processes across multiple systems. Experience includes data transformation, API integrations, scheduled tasks, and conditional logic. Recent projects focus on AI-powered automation using Claude and GPT models."

*Question: "What's your experience with Salesforce?"*
- **Answer:** "Implemented Salesforce automations including flows, validation rules, and custom field logic. Integrated with third-party tools via webhooks and APIs."

*Question: "Tell us about your AI/automation background"*
- **Answer:** "Advanced AI user with 5+ years in workflow automation and AI implementation. Direct experience systemizing actions across multiple platforms using Claude, GPT, and workflow automation tools. Focus on ROI-driven automation that reduces manual effort and improves efficiency."

### Step 5: Form Validation & Error Recovery

- **GitHub URL required:** If field appears and no URL provided, fill with: `https://github.com/wiredwizard`
- **Headline required:** Auto-generate: "AI Prompt Engineer | Generative AI | Workflow Automation"
- **Custom required fields:** Attempt intelligent answer based on job description, resume, and context
- **If form is malformed or unclear:** Skip to next job

### Step 6: Multi-Step Form Navigation

- Click through all form sections (contact → resume → work history → education → demographics → additional questions)
- At each step, verify data is correct before proceeding
- Click final "Review" or "Submit" button to complete application
- Capture confirmation (e.g., "Application submitted 1 day ago")

### Step 7: Complete All 2 Applications

- Repeat Steps 3–6 for each of the 2 qualifying jobs
- Track which jobs were applied to
- Report final status: names of companies, positions, and submission confirmation

---

## Embedded Standard Answers Reference

These answers are used throughout the applications:

```
PERSONAL INFO
- Name: Morgan Todd
- Email: prplwiredwizard@gmail.com
- Phone: 919-590-0954
- City: Harrisburg, Pennsylvania
- LinkedIn: https://www.linkedin.com/in/wiredwizard/
- GitHub: https://github.com/wiredwizard
- Profile Image: MorgaIT.png

WORK AUTHORIZATION
- Legally authorized: Yes
- Require sponsorship: No
- Eligible without visa: Yes
- Secondary employment: No (reason: conflict with duties/working hours)

EXPERIENCE
- AI/Automation: 5+ years
- Python: 15-20 years
- SQL: 15-20 years
- JavaScript: 15-20 years
- All programming languages: 15-20 years

SALARY & BENEFITS
- Annual expectation: $95,000
- Willing to relocate: No

HEADLINE/SUMMARY (ADAPTIVE)
- Default: "AI Prompt Engineer | Generative AI | Workflow Automation"
- Adapt based on job title/focus if needed

AI TOOLS STATEMENT
"I am an advanced AI user and have directly used AI to completely systemize actions and workflows."

DEMOGRAPHIC (AUTO-SELECT IF AVAILABLE)
- Age 18+: Yes
- Race/Ethnicity: White (Not Hispanic or Latino)
- Gender: Male
- Sexual Orientation: Heterosexual
- Disability: No
```

---

## Usage Example

**User says:**
> "Apply to 2 AI automation jobs on LinkedIn for me"

**Skill does:**
1. Opens LinkedIn job search → filters for "AI Automation" + Remote nationwide + Easy Apply
2. Finds 2 qualifying remote positions
3. For each job:
   - Fills all standard fields from this skill's embedded answers
   - Generates appropriate headline from resume
   - Adapts answers to job-specific questions
   - Uploads MorgaIT.png if requested
   - Completes form and submits
4. Reports: "✅ Applied to [Company 1], [Company 2]"

---

## Key Behaviors

- **Remote only nationwide:** Skips any position marked Hybrid, On-site, or requiring travel. Searches all remote opportunities nationwide.
- **No Resolv Global:** Automatically rejects all Resolv Global positions
- **Location set to Harrisburg, PA:** Personal location profile shows Harrisburg, PA but applies to remote jobs nationwide
- **Smart adaptation:** Uses job description to inform custom question answers
- **Profile image included:** Always uploads MorgaIT.png when form has image field
- **Graceful degradation:** If a job's form is unusual, moves to next rather than guessing
- **LinkedIn profile auto-fill:** Leverages LinkedIn's auto-population for work history, education, and basic info
- **Experience honesty:** Reports 15-20 years for programming skills, 5+ for AI/automation, matching actual background

---

## What You Need

- Active LinkedIn account logged in
- Chrome browser with LinkedIn access
- MorgaIT.png profile picture in project folder
- Morgan Todd AI.docx resume in project folder (reference for headlines/summaries)

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Form won't load | Refresh LinkedIn page and try again; may be temporary server issue |
| "Applied" button unchanged | Application may have submitted silently; check LinkedIn notifications/application status |
| Custom question unclear | Skill will attempt best answer from resume; if still problematic, skips to next job |
| GitHub URL required but empty | Auto-fills: https://github.com/wiredwizard |
| Headline required but empty | Auto-fills: "AI Prompt Engineer \| Generative AI \| Workflow Automation" |
| Profile image won't upload | Verify MorgaIT.png exists in project folder; some forms may not support image upload |

---

## Success Criteria

✅ Applied to 2 remote job positions nationwide  
✅ All standard answers filled correctly  
✅ Location profile set to Harrisburg, PA  
✅ No applications to Resolv Global or non-remote roles  
✅ Custom questions answered intelligently based on resume  
✅ Profile image uploaded (if form allowed)  
✅ All 2 applications successfully submitted
