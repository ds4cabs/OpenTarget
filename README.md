# OpenTarget

[![CABS: ds4cabs](https://img.shields.io/badge/CABS-ds4cabs-1f4b99?logo=github)](https://github.com/ds4cabs)
[![GitHub Pages: live](https://img.shields.io/badge/GitHub_Pages-live-brightgreen?logo=github)](https://ds4cabs.github.io/OpenTarget/)
![CABS: 2026](https://img.shields.io/badge/CABS-2026-6f42c1)
![status: MVP in progress](https://img.shields.io/badge/status-MVP_in_progress-f1c40f)
![type: Dossier Generator](https://img.shields.io/badge/type-Dossier_Generator-1f6feb)
![domain: Target Validation](https://img.shields.io/badge/domain-Target_Validation-0aa)

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
