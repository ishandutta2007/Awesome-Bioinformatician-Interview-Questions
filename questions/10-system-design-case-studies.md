# 10. System Design / Case Studies

Open-ended, senior-level scenarios synthesizing themes across the entire repo. As with the format established across this series, these are typically extended discussions rather than questions with one correct answer — evaluate reasoning quality, appropriate skepticism toward messy real-world data, and whether the candidate integrates biological, statistical, and software engineering rigor together rather than treating them as separate silos.

---

### 10.1 🔴 Design the end-to-end pipeline and analysis approach you'd propose for a genomics core facility that needs to support many different research groups' varied sequencing projects (germline variant calling, somatic variant calling, and RNA-seq) using a shared, standardized infrastructure.

**What a strong answer covers:**
- Should propose a modular pipeline architecture using an appropriate workflow management system (category 3.11) and containerization (category 3.12), recognizing that different project types (germline vs. somatic vs. RNA-seq, per categories 3.15 and 4.1) require genuinely different downstream analytical branches even if they share common early steps (QC, adapter trimming, alignment).
- Should propose validating each distinct pipeline branch against an appropriate reference truth set (category 3.17) before offering it as production infrastructure to research groups.
- Should address reference genome and annotation version standardization (category 6.7) as a deliberate, documented, and centrally-managed policy, given how many different research groups would be relying on this shared infrastructure and the confusion that inconsistent versioning across projects would create.
- Should propose a metadata standardization and intake process (echoing category 6.6's discussion of the common SRA metadata reconciliation problem) to prevent exactly this kind of downstream headache from occurring in the first place across many different research groups' varied submission practices.
- Should address computational scalability (category 7.10) and cloud infrastructure considerations (category 7.11) given the genuinely bursty, variable demand a shared core facility would need to support across many different projects and research groups.
- A strong answer explicitly discusses how the facility would handle pipeline versioning and validate/communicate the impact of any future pipeline updates on already-processed projects, echoing the pipeline version transition discussion found in adjacent precision medicine bioinformatics literature.

---

### 10.2 🟡 A wet-lab collaborator hands you an RNA-seq differential expression result they generated themselves using a popular online analysis tool, and asks for your help interpreting an unexpectedly large number of significant genes before they submit for publication. How would you approach reviewing their work?

**What a strong answer covers:**
- Should systematically work through the specific diagnostic questions developed in category 4.12 — checking experimental design for batch confounding (category 4.8, 5.3), examining effect sizes alongside significance (category 5.9), and verifying the normalization and multiple-testing correction methodology was actually applied correctly (categories 4.4-4.7).
- Should specifically ask about and verify the replicate structure used — checking for the pseudoreplication risk discussed in category 5.1, which is a genuinely common error for researchers using an online tool without a bioinformatics background to catch on their own.
- Should approach this collaboratively and educationally rather than simply delivering a verdict — since the collaborator will need to understand and defend this methodology during peer review, not just receive a corrected number.
- Should propose gene set enrichment analysis (category 4.9) as an additional check on whether the flagged genes show a biologically coherent pattern, providing useful corroborating (or contradicting) evidence beyond the raw statistical output alone.

---

### 10.3 🔴 You're reviewing a colleague's pull request adding a new variant annotation step to a shared production pipeline. The change works correctly in their local testing but you notice it silently drops variants with certain unusual formatting rather than raising an error. How do you handle this code review?

**What a strong answer covers:**
- Should directly apply the "fail fast" principle discussed in category 7.14 — flagging the silent-drop behavior as a specific, serious concern rather than a minor style preference, since silently discarding data is exactly the kind of failure mode that can produce a subtly incorrect downstream result without any visible warning sign.
- Should ask specifically what proportion and category of real-world variants this formatting edge case actually represents, rather than assuming it's negligible — echoing the defensive programming discussion in category 7.6 regarding real-world data messiness being the norm rather than a rare edge case in bioinformatics specifically.
- Should propose a specific, constructive fix — either handling the edge case correctly, or explicitly flagging/logging any dropped variants with a clear warning so this behavior is visible and auditable rather than silent, consistent with the reproducibility and traceability themes discussed throughout category 7.
- Should raise this in a collegial, specific, and evidence-based way (echoing the code review discussion in category 7.8), pointing to the exact behavior and its potential consequence rather than a vague, general concern.

---

### 10.4 🟡 Two team members disagree about pipeline architecture: one wants to adopt a fully cloud-native, elastically-scaled infrastructure immediately; the other is concerned about cost predictability and wants to continue relying primarily on the team's existing, owned computing cluster. How would you help resolve this disagreement?

**What a strong answer covers:**
- Should take both positions seriously, echoing the disagreement-navigation approach modeled throughout this series.
- The cloud-native position's legitimate case: per category 7.11, genomics workloads are often genuinely bursty, and elastic scaling can better match actual resource consumption to this variable demand pattern, potentially improving both cost efficiency and turnaround time during peak demand periods.
- The owned-infrastructure position's legitimate case: cost predictability is a genuine, practical organizational planning concern, and a poorly-managed cloud migration can produce unexpectedly high or volatile costs if usage isn't carefully monitored and controlled, plus there may be genuine data governance or existing sunk-cost infrastructure considerations relevant to a specific organization's situation.
- A strong answer proposes resolving this with actual, specific usage data rather than abstract preference — characterizing the team's real historical computational demand pattern (how bursty is it actually, in practice) and running a genuine cost comparison grounded in that real usage pattern, potentially proposing a hybrid approach (using owned infrastructure for baseline, predictable demand and cloud infrastructure specifically for burst capacity) rather than treating this as a binary, all-or-nothing choice.

---
