# Mega Prompt: Recommended Reading List Skill

## Role

You are a **Skill Architect** specializing in academic curriculum workflows. Generate a production-grade, distributable Claude skill that takes a course syllabus and produces a curated supplementary reading list of recent peer-reviewed research as a professionally formatted Word document.

## Output Target

**Two files:**

- `${SKILLS_DIR}/recommended-reading-list/SKILL.md` (main skill, ~2,000 words)
- `${SKILLS_DIR}/recommended-reading-list/scripts/generate_reading_list.js` (bundled DOCX generator, ~300 lines)

Word budget for SKILL.md: 1,800–2,200. Hard ceiling: 2,500.

## Skill Purpose

For an instructor or student with a course syllabus, produce a professional supplementary reading list as `.docx`:

1. Parse syllabus to extract topics + learning outcomes
1. Group related topics into 6-12 sections
1. Search Consensus for recent peer-reviewed papers per section
1. Select 1-3 papers per section (15-25 total)
1. Write plain-language summaries + discussion questions tied to learning outcomes
1. Generate styled DOCX with clickable Consensus links

Output is a ready-to-distribute Word document that supplements the textbook with current research.

## Architectural Pattern: Bundled Script

This skill uses a **bundled JavaScript helper script** for DOCX generation. Rationale:

- DOCX generation logic is reusable and complex (300+ lines)
- Better separation of concerns: skill = orchestration + intelligence; script = mechanical document assembly
- Token-efficient: the skill doesn’t need to re-derive DOCX layout each run
- Easier to maintain and version

The mega prompt produces BOTH the skill file AND the script.

## Required Capabilities

The skill must specify how to:

1. **Parse syllabus** — Handle PDF / DOCX / text / pasted content / image (use appropriate reader)
1. **Extract topics + learning outcomes** — If outcomes missing, infer 3-5 from description + topic list
1. **Group topics** — Aim for 6-12 sections; closely related topics merge
1. **Confirm grouping with user** — Show proposed grouping before searching
1. **Run targeted Consensus searches** — 1-2 per section, sequential, with applied-domain angle
1. **Select papers** — Priority: relevance > reviews/meta-analyses > citation count > applied-domain connection
1. **Write summaries + discussion questions** — Plain language for summaries; learning-outcome-tied for questions
1. **Generate DOCX via bundled script** — Pass JSON data, get .docx out
1. **Validate output** — Run validation script after generation
1. **Surface audit summary** — In chat alongside file delivery

## Workflow Structure

The generated skill must follow this structure:

```
1. Overview + value proposition
2. Data Integrity Principles
3. Phase 1: Parse the Syllabus
4. Phase 2: Search Consensus for Each Section (with rate limit + failure handling)
5. Phase 3: Write Summaries and Discussion Questions
6. Phase 4: Generate the .docx Document (via bundled script)
7. Phase 5: Deliver to User (file + audit summary)
8. Important Notes (year range, tier, languages, file types)
```

## Critical Improvements Over Naive Implementation

The skill MUST address these concerns:

1. **Applied-domain weaving** — Critical insight: don’t just search “enzyme kinetics” — search “enzyme kinetics food processing applications”. Document this pattern with concrete examples per discipline. Boosts paper relevance dramatically.
1. **Sequential execution discipline** — 1 query/sec rate limit. Sleep 1 between calls. Confirm result before next call.
1. **Plan-tier awareness** — Free tier = 3 papers/search. With ~10 sections × 1-2 queries = 30-60 candidates. Pro tier doubles this. Surface to user.
1. **Source discipline** — Hard rule: only cite Consensus papers from this session. Training knowledge labeled and excluded.
1. **Three-count tracking** — Queries sent / papers received / papers cited. Surface in chat audit summary.
1. **Group-and-confirm before searching** — Present proposed section grouping to user. Wait for confirmation. This prevents wasted searches.
1. **Summary quality bar** — Plain language for undergrads. Define jargon. Make student think “I want to read this”.
1. **Discussion question quality bar** — Beyond recall (apply / analyze / evaluate). Tied to a specific learning outcome.
1. **Topic grouping intelligence** — 6-12 sections is the sweet spot. Closely-related topics merge.
1. **Year range** — Default `year_min: current_year - 1` for recency. Configurable.

## Bundled Script Specification

The mega prompt must also produce `scripts/generate_reading_list.js`. The script:

- Accepts JSON input file path + output DOCX path as CLI args
- Uses `docx` npm package (require pattern with multi-location fallback for robustness)
- Produces a clean professional document with:
  - Title page section (course name, subtitle, date)
  - Introduction with clickable consensus.app link
  - “Course Learning Outcomes” boxed section
  - Numbered papers under section headings, each with:
    - Clickable hyperlinked title to Consensus URL
    - Author / journal / year in italic gray
    - “Summary:” line in plain language
    - “Discussion Question:” line in blue accent color
  - Footer with generation metadata
- Handles input validation (missing fields → graceful error)
- Uses ExternalHyperlink with full Consensus URL (never truncated)
- Uses LevelFormat.BULLET for any lists (not unicode bullets)

Document the JSON input schema explicitly:

```json
{
  "courseTitle": "string",
  "courseSubtitle": "string",
  "generatedDate": "string",
  "yearRange": "string",
  "introText": "string",
  "learningOutcomes": ["string", ...],
  "sections": [
    {
      "heading": "string",
      "papers": [
        {
          "title": "string",
          "authors": "string",
          "journal": "string",
          "year": number,
          "url": "string",
          "summary": "string",
          "question": "string"
        }
      ]
    }
  ],
  "auditLog": {
    "totalQueriesSent": number,
    "totalPapersReceived": number,
    "totalPapersCited": number,
    "toolConstraints": "string",
    "searchDetails": [
      {
        "section": "string",
        "query": "string",
        "papersReturned": number,
        "papersSelected": number,
        "status": "string"
      }
    ],
    "failures": []
  }
}
```

## Source Discipline Rules (Must Be Stated)

The skill must include an explicit “Data Integrity Principles” block:

- **Only use what Consensus returns** — Every paper title, author, journal, year, URL must come from this session’s tool calls. Training-knowledge papers labeled `[Not from Consensus — model knowledge]` and excluded.
- **Confirm before moving on** — A search isn’t complete until response received and inspected.
- **Track three counts** — Queries sent / papers received / papers cited. Surface in audit summary.
- **Surface gaps, don’t fill them** — Section with one paper + note about limited results > section padded with fabrications.

## Trigger Phrases (for frontmatter description)

- “find papers for my course”
- “create a reading list from this syllabus”
- “recent research for my class”
- “supplementary readings”
- “find journal articles for these topics”
- “what recent papers cover this material”
- “any new research on these course topics”
- “update my syllabus with recent papers”
- Casual mentions when syllabus is attached

## Quality Bars (Must Be Documented With Examples)

### Summary Quality

- ✅ Good: “This review maps how different diets — Mediterranean, Nordic, and vegetarian — reshape the types of fat molecules circulating in your blood, with implications for heart disease risk.”
- ❌ Bad: “This paper reviews lipidomic profiles across dietary interventions and their cardiometabolic implications.” (Too jargon-heavy)

### Discussion Question Quality

- ✅ Good: “If dietary fat quality can reshape your lipoprotein lipidome, what does this suggest about the biochemical basis for dietary guidelines recommending unsaturated over saturated fats?”
- ❌ Bad: “What did the authors find?” (Just recall)

## Error Handling Requirements

|Failure                             |Behavior                                                                |
|------------------------------------|------------------------------------------------------------------------|
|Consensus rate-limit hit            |Wait 3s, retry once, log                                                |
|Search returns 0 for a section      |Note section as “limited results — consider manual supplementation”     |
|3 consecutive failures              |Stop, alert user, share collected so far, ask how to proceed            |
|`docx` package not installed        |Script attempts `npm install`; if still failing, fail with clear message|
|DOCX validation fails               |Unpack XML, log issue, ask user to retry                                |
|Syllabus format unsupported         |List supported formats, ask user to convert                             |
|Learning outcomes can’t be extracted|Infer 3-5 from course description; mark as inferred in document         |

## Portability Requirements

Document at top:

> **Portability:** Requires a Consensus MCP connection, Node.js with `docx` package, and file reading capability for the syllabus. Works in Claude Code CLI natively. In Claude.ai with Consensus MCP + Code Execution + file upload, the workflow is supported.

## Dependencies

- **Consensus MCP** — Required for literature search
- **`docx` Node.js library** — Required (`npm install docx`)
- **Bundled script** — `scripts/generate_reading_list.js` (shipped with skill)
- **File reading** — Tools appropriate to syllabus format (PDF reader, DOCX parser via pandoc, vision for images)

## Frontmatter Spec

```yaml
---
name: recommended-reading-list
description: "Generates a curated supplementary reading list from any course syllabus using Consensus academic search. Parses the syllabus to extract topics and learning outcomes, searches Consensus for recent peer-reviewed papers per topic, and produces a professionally formatted .docx with clickable Consensus links, plain-language summaries, and discussion questions tied to course learning goals. Triggers whenever a user uploads a syllabus, course outline, or curriculum document and wants supplementary readings. Also triggers on: 'find papers for my course', 'create a reading list from this syllabus', 'recent research for my class', 'supplementary readings', 'find journal articles for these topics', 'what recent papers cover this material', 'any new research on these course topics', 'update my syllabus with recent papers'. Even casual mentions when a syllabus is attached should trigger this skill."
---
```

## Anti-Patterns To Reject

- Parallelizing Consensus calls (rate limit)
- Searching topics without applied-domain angle (poor relevance)
- Padding sections with fabricated entries when Consensus returns thin
- Generic discussion questions (“What did the authors find?”)
- Jargon-heavy summaries unsuitable for the course’s audience level
- Skipping the group-and-confirm step (wastes searches)
- Truncating Consensus URLs in hyperlinks
- Inlining 300 lines of docx-generation JavaScript in the skill body (use bundled script)

## Validation Checklist (Run Before Delivery)

- [ ] SKILL.md frontmatter parses as YAML
- [ ] SKILL.md word count 1,800–2,500
- [ ] Data Integrity Principles block present
- [ ] Applied-domain weaving documented with examples
- [ ] Sequential execution + 1 query/sec rate limit stated
- [ ] Plan-tier awareness (3/search free, more for Pro) documented
- [ ] Bundled script produced at `scripts/generate_reading_list.js`
- [ ] Script accepts JSON input + output path CLI args
- [ ] Script handles `docx` require from multiple locations
- [ ] JSON schema documented in skill
- [ ] Summary + discussion question quality bars with examples
- [ ] Audit summary in chat documented as part of delivery
- [ ] 6+ failure modes documented
