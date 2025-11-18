# Copilot Instructions for FoundLab Institutional Documents

This repository contains institutional documents, not source code. Your primary role is to assist in creating, editing, and maintaining these Markdown-based documents.

## 1. Core Concepts

To be effective, you must understand the following core concepts:

- **FoundLab:** The company providing the solutions.
- **BTG Pactual:** The client or target audience for these documents.
- **Infraestrutura de Confiança Auditável (ATI) / Auditable Trust Infrastructure:** The core thesis and offering.
- **Umbrella:** The platform for processing documents with auditing.
- **Veritas 2.0:** The protocol for auditability, which includes `DecisionID` and a `WORM` (Write-Once, Read-Many) audit trail.
- **Zero-Persistência / Zero-Persistence:** A key technical principle where sensitive content is not stored.

## 2. Document Structure and Purpose

The documents explain FoundLab's proposal to BTG Pactual. Key documents include:

- `Uma Proposta de Infraestrutura de Confiança Auditável para o BTG Pactual.md`: The main proposal, providing context.
- `Deck Bancário para IA AuditávelV1.md`: The main presentation deck.
- `Veritas_2.0_Whitepaper (1).md`: Technical details on the auditability layer.
- `Simulação ROI FoundLab para BTG Pactual.md`: Return on Investment simulation.

A suggested reading order is provided in the `README.md`. Refer to it to understand the flow of information.

## 3. Key Technical Principles

The technical approach discussed in these documents is based on:

- **Zero-persistence** of sensitive data.
- Generation of a **DecisionID** for each analysis.
- An **append-only (WORM)** audit trail.
- Deployment on **Google Cloud** (Cloud Run, Document AI, Vertex AI, BigQuery).
- Compliance with **LGPD** (Brazilian General Data Protection Law).

## 4. Conventions for Document Updates

When editing or creating documents, adhere to the following:

- **Formal Tone:** Maintain a professional and formal tone suitable for the financial sector.
- **Markdown:** Use Markdown (`.md`) as the primary format.
- **No PII:** Do not include any Personally Identifiable Information or client data.
- **Versioning:** When revising a document that has been shared, append a version suffix (e.g., `...v2.md` or `...2025-11.md`).
- **Descriptive Naming:** New documents should have clear, descriptive filenames.
