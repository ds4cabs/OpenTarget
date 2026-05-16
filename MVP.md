# OpenTarget — MVP Version (Gemini AI Agent · Dossier Generator)

**Intern:** Xiaoxue Li
**Level:** Master student (University of Chicago), Intermediate Python with Scanpy background
**Timeline:** 2–3 weeks (~50 hours)
**Paradigm:** **Dossier Generator** — CLI tool that takes a target + disease, runs an autonomous Gemini workflow across 6 public databases including single-cell expression context (CELLxGENE), and writes a target-validation dossier with go/no-go verdict.
**Database count:** **6** (expanded from 4 to **restore Xiaoxue's signature single-cell scope** from her original proposal — CELLxGENE Discover — plus ClinVar for clinical-variant relevance).

---

## The Agent

**What the agent does (autonomous workflow):** Given a target + disease, the agent autonomously queries Open Targets, GTEx, ChEMBL, AlphaFold DB, **CELLxGENE Discover** for cell-type-resolved expression, and **ClinVar** for clinically significant variants — writing a target dossier with a go/no-go-style verdict resolved to the cell-type level.

**Input:** Target + disease (CLI: `python agent.py --target PCSK9 --disease hypercholesterolemia`).
**Output:** `dossiers/{target}_{disease}.md` + `.json` — bulk-tissue evidence + **cell-type specificity** + chemistry + structure + **clinical variants** + integrated go/no-go verdict.

**Tools (6 public databases):**

1. `get_target_evidence(target, disease)` — **Open Targets GraphQL**.
2. `get_tissue_expression(target)` — **GTEx Portal API**: bulk-tissue mRNA.
3. `get_target_compounds(target)` — **ChEMBL REST**.
4. `get_alphafold_pocket(target_uniprot)` — **AlphaFold DB**: pLDDT-derived pocket confidence.
5. `get_cellxgene_celltype_expression(gene_symbol, n_cell_types=10)` — **CELLxGENE Discover API**: per-cell-type expression across published single-cell datasets (this is Xiaoxue's original Scanpy strength — restoring the signature scope she put in her original proposal).
6. `get_clinvar_variants(gene_symbol)` — **ClinVar** via NCBI E-utilities: clinically significant variants for target prioritization.

**Example runs (≥3):**

- `python agent.py --target PCSK9 --disease hypercholesterolemia` → dossier: strong genetics (Open Targets) + liver-bulk expression (GTEx) + hepatocyte-specific in single cell (CELLxGENE) + 50+ compounds (ChEMBL) + good structure (AlphaFold) + LDLR-pathway ClinVar variants → strong "Go".
- `python agent.py --target ANGPTL3 --disease hypercholesterolemia` → similar verdict but with narrower cell-type expression (CELLxGENE shows hepatocyte-restricted, less liver-broad than PCSK9).
- `python agent.py --target INHBE --disease MASLD` → dossier including cell-type specificity (CELLxGENE shows hepatocyte expression with limited extrahepatic), ClinVar variants for INHBE, AlphaFold structural confidence.

---

## Week-by-Week

**Week 1 (~14h):** Build 6 tool functions. **CELLxGENE Discover** requires Scanpy / anndata familiarity — Xiaoxue's existing strength, but allow time to wire up the API for agent-tool use (CELLxGENE has a REST and a Census Python API; pick whichever streams cleanly).
**Week 2 (~22h):** Clone dossier-generator sub-template. Wire up Gemini. Build dossier formatter with embedded tissue heatmap + cell-type bar chart.
**Week 3 (~14h):** Generate 5 target dossiers + 2 head-to-head comparisons; tune system prompt for explicit go/no-go reasoning at cell-type resolution; README + demo.

## What's OUT

Human Cell Atlas direct integration (CELLxGENE covers it as the practical access layer), IEU OpenGWAS, DrugBank, ligand-receptor pair analysis, ClinicalTrials.gov competitive-landscape, openFDA FAERS, PubMed citation extraction, n8n.

## Stretch Goals

- 7th tool: `get_active_trials(target)` for competitive landscape.
- Restore ligand-receptor analysis (Xiaoxue's original) as a 8th tool for stretch.

## Realistic CV Entry

*Built OpenTarget, a working Gemini AI dossier-generator agent for target validation integrating 6 public databases spanning genetics, bulk expression, single-cell expression, chemistry, structure, and clinical variant interpretation.*

- Wrapped 6 public databases (Open Targets, GTEx, ChEMBL, AlphaFold DB, **CELLxGENE Discover, ClinVar**) into a Gemini agent producing target-validation dossiers with cell-type-resolved expression and go/no-go-style verdicts.
- Restored single-cell expression context as the project's signature differentiator, building on prior Scanpy experience.
- Generated 5 target dossiers + 2 head-to-head comparisons across cardiometabolic targets.

## Tech Stack

Python, `google-generativeai`, Scanpy, anndata, requests, pandas, matplotlib, Open Targets GraphQL, GTEx Portal API, ChEMBL REST, AlphaFold DB, CELLxGENE Discover API / Census, NCBI E-utilities (ClinVar).

---

## Shared Agent Skeleton (three paradigms, one Gemini primitive)

Every intern's agent uses Gemini's automatic function calling, but the interface layer differs by paradigm. The cohort uses **one starter repo with three sub-templates** that interns clone in week 1:

- **Dossier-generator template** — CLI script: takes structured args, runs the agent workflow autonomously, writes `*.md` + `*.json` to disk. Used by Beyza, Chin Hung, Christina, Shucheng, Xiaoxue.
- **Dashboard template** — Streamlit page with selectors and tables; the agent is invoked on button-click for specific synthesis tasks. Used by Aaron, Jason, Shawn.
- **Computation-engine template** — Streamlit form (or CLI) that takes structured analytical inputs, runs the agent workflow, produces a downloadable analytical report with plots. Used by Reuben, Kening, Natalie.

**Why no chat interfaces?** Scientists need reproducible, shareable artifacts. The agent dimension (Gemini-as-orchestrator, autonomous tool-calling across multiple public databases, synthesis across sources) is preserved in all three paradigms; only the deliverable shape changes.

**Christina** (OpenRepurpose evidence-and-validation module) owns the starter repo with all three sub-templates. The shared repo should also include pre-built wrappers for the most heavily-used databases (ChEMBL, openFDA FAERS, Open Targets, ClinVar) so multiple interns don't redo the same boilerplate.

### Reference snippet — Gemini function calling (same across all three paradigms)

```python
import google.generativeai as genai
import os
genai.configure(api_key=os.environ["GEMINI_API_KEY"])

def my_tool(arg: str) -> dict:
    """One-line docstring Gemini uses to decide when to call this tool."""
    return {"result": ...}

model = genai.GenerativeModel(
    model_name="gemini-2.5-flash",
    tools=[my_tool, other_tool, ...],   # 4-8 tools per agent
    system_instruction=open("system_prompt.md").read(),
)
chat = model.start_chat(enable_automatic_function_calling=True)
response = chat.send_message("structured request — one shot, not a conversation")
```
