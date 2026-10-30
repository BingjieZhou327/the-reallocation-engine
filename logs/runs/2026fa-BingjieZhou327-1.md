# INFO7375 — Setup Exercise: Your Search's Personal Layer

**RUN_LOG — October 29, 2026**  
**Project:** Reallocation Engine · Personal Search Layer  
**Method:** AI-assisted extraction and drafting, followed by my own factual review and corrections

## 1. Purpose and artifacts

I used AI to turn my resume and job-search preferences into structured inputs for the Reallocation Engine, then checked whether those inputs represented my actual experience and constraints. My goal was not simply to produce valid files: it was to distinguish information supported by evidence from plausible but incorrect AI interpretations.

| Artifact | What it contains | Verification performed |
| --- | --- | --- |
| `search/resume.json` | Structured education, three jobs, two projects, skills, and awards | Compared with my source resume and firsthand knowledge; corrected three import errors; set `attested: true` and `attested_date: 2026-10-29` |
| `search/profile.yml` | Target positions, student-status/OPT constraints, preferred locations, industry preferences, and sponsorship rule | Compared the AI draft with my answers to five intake questions; corrected three interpretations |
| `search/gaps.md` | Evidence-based gaps between my record and target UX positions, with plans and closing conditions | Checked cited demand signals, removed misleading rows, and rewrote my highest-priority gap |

I retained the original AI outputs separately to distinguish the initial extraction from my corrected records. The records above are the final working artifacts; original drafts and review notes are audit evidence, not substitutes for them.

## 2. Resume attestation — three AI import errors

**Attestation:** I reviewed the titles, dates, descriptions, quantitative outcomes, and project roles in `search/resume.json`. The file records `attested: true` on October 29, 2026. The three corrections below concern interpretation of information, not cosmetic JSON formatting.

### Error 1 — Method/Metric Misclassification (Technology Company B, `exp-2`)

**Claude's error**

Claude placed `task success rate analysis` in the `methods` field together with `usability testing`, `participant recruitment`, and `SUS`. My original resume mentioned *first-time task success rates* as one of the usability metrics evaluated, so Claude did not invent the metric. The error was in how it classified the information. Usability testing is a research method; recruiting participants is a research activity; the System Usability Scale (SUS) is a standardized measurement instrument; and task success rate and SUS score are measures or outputs. Grouping all of them under `methods` makes unlike qualifications appear equivalent.

**Human correction**

I separated the information according to its actual role in the research process:

| Field in the corrected JSON | Value |
| --- | --- |
| `methods` | `usability testing` |
| `research_activities` | `participant recruitment` |
| `measurement_instruments` | `System Usability Scale (SUS)` |
| `usability_metrics` | `first-time task success rate`, `SUS score` |

I retained the relevant information rather than deleting it; I corrected the structure that described it.

**Why it matters**

A job-matching system may use structured fields to judge whether I have experience conducting a particular research method. If it interprets a performance metric as a separate method, it can overstate my methodological breadth or match me to a requirement on the wrong basis. This case showed me that accurately copying words from a resume does not guarantee that an AI has understood their professional meaning.

### Error 2 — Role Inflation (Trip Sculptor, `proj-2`)

**Claude's error**

Claude assigned `role: UX lead` to my Trip Sculptor project. It appears to have inferred a leadership title from the sentence that I **led the full UX process**, from user research through usability testing. But Trip Sculptor was my **individual project**. I was responsible for the end-to-end work myself; I was not supervising a UX team or holding a formal UX Lead position. Leading a process and leading a team are not interchangeable claims.

**Human correction**

I replaced the unsupported title and made the project arrangement explicit:

```json
"role": "Independent UX Designer",
"project_type": "Individual Project",
"team_size": 1
```

I separately reviewed the TheraVR project and confirmed that its `Team lead` designation was accurate; that title was retained. This distinction matters because I did not simply remove every leadership-related phrase as a precaution—I checked each claim against what I actually did.

**Why it matters**

Role labels influence assessments of seniority, ownership, and people-management experience. Calling a solo project a UX Lead role could create a misleading impression and lead to job recommendations or interview expectations that my experience does not support. My correction preserves the genuine strength of independent, end-to-end UX work without adding an unsupported management claim.

### Error 3 — Tool/Channel Misclassification (Psychological Services Company C, `exp-3`)

**Claude's error**

Claude listed `WeChat official platform` under `tools`. In the source resume, WeChat appeared because psychology-focused articles were **published** through the organization's official account and their reach could be measured there. In this experience, WeChat primarily functioned as a **publication/distribution channel**, not as the main tool I used to produce my work. Claude treated a technology name mentioned in an achievement as if it necessarily represented a professional production tool.

**Human correction**

I removed WeChat from the `tools` field and recorded the actual function of the platform:

```json
"tools": [],
"publication_channels": ["WeChat Official Account"]
```

I deliberately did **not** add Notion or Microsoft Office to individual experience entries just because I have used those applications. The task was to correct Claude's extraction, not to add details that were absent from the source resume without a separate reason for doing so.

**Why it matters**

A publication platform, a measurement channel, and a day-to-day work tool are different kinds of evidence. Conflating them can misrepresent what software I used to perform the work and what capabilities a recruiter should infer. The distinction also protects the Reallocation Engine from rewarding keyword presence over a defensible description of the workflow.

**Attestation summary recorded in `resume.json`:** method/metric misclassification; inflated Trip Sculptor role; WeChat publication-channel misclassification. I preserved the source-file reference structure while replacing its identifying filename in this sharing copy.

## 3. Search profile — my corrections to the first AI draft

I answered the five intake categories (target role; visa and timeline; geography; industry and company preferences; sponsorship), then reviewed the draft for changes to my real eligibility or search boundaries.

| Draft field or interpretation | My correction | Why it changes matching behavior |
| --- | --- | --- |
| `industries.targets` abbreviated `AI / Emerging Technology`, `Healthcare / HealthTech`, and `Education / EdTech` as `AI`, `HealthTech`, and `EdTech` | Restored the full industry labels | Shortened categories could silently narrow the kinds of employers I am willing to consider |
| `geography.metros` listed Boston, New York City, the San Francisco Bay Area, and Seattle without indicating whether they were exclusive | Used `geography.preferred_metros`, plus `other_us_locations_accepted: true` and `search_scope: "United States"` | These four metros are **preferences**, not a whitelist; a good opportunity elsewhere in the US should remain eligible |
| `industries.hard_exclusions` said only `Financial Industry` | Added `hard_exclusion_scope` defining employers whose primary business is financial services, while not automatically excluding nonfinancial employers with payment features | Prevents a keyword-level exclusion from rejecting companies outside the financial sector |

**The most important profile correction was geographic scope.** An AI-generated filter that silently treats preferred cities as the *only* acceptable cities would reject jobs I am genuinely willing to take. The finance boundary was a clarification of my own broad wording, not an invented mistake by Claude.

The final file also preserves these confirmed constraints: entry-level **full-time UX Designer / UX Researcher** roles after Spring 2027 graduation; current **student status (visa category withheld)**; **post-completion OPT not yet applied for**; authorization dates left `null`; STEM eligibility recorded as confirmed on the basis of my reported school verification (document details withheld); a **70-day internal unemployment alert**; no company-size limit; acceptance of on-site, hybrid, and remote work; and **sponsorship preferred rather than required as a hard gate**. I would consider employers that can employ me during valid OPT even if they will not offer longer-term sponsorship.

## 4. Gap analysis — rows removed and priority gap rewritten

I compared the attested record and profile with identifiable job postings and occupational resources. I treated a missing resume detail as **insufficient evidence in that document**, not automatic proof that the underlying skill or artifact does not exist.

### 4.1 Killed row — G1: Published UX portfolio / case-study evidence

**AI's claim:** Because my one-page resume did not include a portfolio URL, I lacked sufficiently reviewable UX case-study evidence.

**My decision:** **Delete G1.** I already have a portfolio and normally send it separately from my resume when applying. The absence of a URL in a short resume is a presentation choice, not evidence that the portfolio is missing. I should still make sure each application includes a working portfolio link, but that is not the same as needing to create a portfolio from scratch.

### 4.2 Other rows removed after review

**G2 — Design-system evidence:** I removed this from the current gap list because the draft relied too heavily on what was not expanded in my resume. I intentionally mention Figma briefly rather than use limited resume space to describe ordinary tool proficiency. **This does not prove that I have design-system experience**; that separate capability should be checked against actual artifacts if a relevant position requires it.

**G4 — Post-research implementation follow-through:** I removed this because I intentionally prioritize concise business outcomes in my resume rather than detailing every intermediate team decision. My record says the research **contributed to** a 17.6% quarterly sales increase and a 23.6% improvement in user satisfaction; the missing process narrative does not prove that no follow-through occurred. Those figures should not be described as effects conclusively caused by my work without additional evidence.

### 4.3 Rewritten priority row — G3: Limited hands-on A/B-style evaluation experience

**Evidence of demand:** A [Lyft UX Researcher listing](https://builtin.com/job/ux-researcher/11735856) references analyzing experimental research; the [Urban Institute Web and Digital Interface Designer National Occupational Framework (2025, p. 10)](https://www.urban.org/sites/default/files/2025-05/Web_and_Digital_Interface_Designer_National_Occupational_Framework.pdf) identifies A/B testing and analysis of design impact as relevant competencies. These are examples of demand, **not universal entry-level requirements**.

**What I actually have:** I have conducted user interviews, surveys, SPSS analysis, and usability testing. But many of my projects were individual or school projects, with limited time, participants, budget, and live user traffic. I have had fewer opportunities to design and conduct a structured comparison of two alternatives. Unlike G1, this is a gap I recognize from my own practice, not merely a phrase missing from my resume.

**My improvement plan:** I will use **Trip Sculptor**, an existing project, to examine one specific UX decision (for example, how a user starts creating a trip). I will make two clickable Figma variants that differ in one key element, define the same task and success measures in advance, and aim to recruit approximately **12–20 consenting participants** if feasible. I will randomly assign participants to a variant, record completion, time/errors, and brief feedback, compare results descriptively, and explain the uncertainty and limits of the small sample. I will describe it as a **controlled prototype comparison**, not pretend that it was a production traffic A/B experiment.

**Closing condition:** A shareable case study documents the research question, variants, protocol, anonymized observations, comparison, limitations, and evidence-supported design decision. The finished case study is then linked from `resume.json`. Merely taking a course or saying I studied A/B testing does not close this gap.

### 4.4 Remaining secondary gap — G5

**Design-to-engineering delivery evidence** remains in `gaps.md`. [O*NET Web and Digital Interface Designers (SOC 15-1255.00)](https://www.onetonline.org/link/summary/15-1255.00) identifies developer collaboration and design documentation as relevant tasks. The closing evidence is a developer-ready handoff package from an existing project, reviewer feedback, and a revision history. This is an evidence gap for design-focused positions, not a claim that every UX researcher must deliver code-ready specifications.

## 5. Step 4 — verification check (explicit answers)

| Required check | Answer | Basis and limitation |
| --- | --- | --- |
| **`resume.json`: Are all job entries traceable and defensible?** | **Yes — based on my personal confirmation.** | I reviewed all three jobs' titles, dates, duties, and reported outcomes and confirmed that each has supporting documents or professional references available. The AI **did not independently inspect** those supporting materials. I corrected the three import errors above and checked for inflated titles, dates, and unsupported skills. |
| **`profile.yml`: Are visa and timeline constraints grounded in actual documents rather than hopes?** | **Yes — based on my reported school verification (document details withheld) and current status.** | I am a student (visa category withheld) and have not applied for post-completion OPT. Authorization dates are `null`, and unemployment days are not yet applicable. Spring 2027 is my planned graduation, not an OPT authorization date. STEM eligibility was reported as verified; the AI did not view the underlying immigration documents. My 70-day threshold is a personal alert, not a legal ceiling. |
| **`gaps.md`: Does every remaining gap cite a real demand signal?** | **Yes — for the cited competencies.** | G3 references the Lyft listing and Urban Institute framework; G5 references the O*NET occupational profile. These sources describe relevant requirements or work activities, **not universal requirements** for every position. My closing plans require observable artifacts, not unsupported assertions. |

## 6. Reflection and AI involvement

The most consequential errors were not spelling mistakes. Claude's structured output was plausible, but plausibility hid distinctions between a **method and a metric**, **process ownership and team leadership**, and **a publication channel and a work tool**. The same problem appeared in search preferences: apparently harmless abbreviations or ambiguous location lists could become real exclusion rules. In the gap analysis, the absence of a detail from a concise resume was initially treated as evidence that an experience or artifact was missing.

The AI accelerated extraction, formatting, comparison, and drafting. **I supplied and verified the facts, challenged the inferences, identified the rows to remove, clarified my actual search constraints, and chose the G3 improvement plan.** Claude produced the initial resume and profile files; ChatGPT assisted with review, drafting the gap table, and compiling this log. My corrections were based on firsthand knowledge, while external links support the market-demand claims. A separate **AI Use Disclosure** is still required under course policy; this run log does not replace it.
