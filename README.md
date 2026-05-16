# OpenTarget

**Intern:** Xiaoxue Li
**Project Type:** Dossier Generator

## Overview
OpenTarget is a target validation dossier generator that combines genetics, tissue expression, single-cell expression, chemistry, structure, and clinical variant evidence into a go/no-go verdict.

## Deliverable
- CLI dossier generator for target + disease queries
- Markdown dossier with bulk and single-cell expression, target compounds, AlphaFold structure, and ClinVar variant context
- Integrated go/no-go recommendation

## Core Tools
- Open Targets
- GTEx
- ChEMBL
- AlphaFold DB
- CELLxGENE Discover
- ClinVar

## Tech Stack
Python, `google-generativeai`, Scanpy / Anndata, requests, pandas, matplotlib

## Notes
This project restores single-cell expression scope as the signature differentiator for target validation.
