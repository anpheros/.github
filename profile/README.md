<p align="center">
  <img src="https://anpheros.com/assets/logo-icon.png" alt="Anpheros" width="88" height="88">
</p>

<h1 align="center">Anpheros</h1>

<p align="center">
  <b>Interoperable medical-data infrastructure for building healthcare applications, medical software and AI services.</b><br>
  One patient-controlled HL7&nbsp;FHIR&nbsp;R4 record per person · an API for applications, clinics, laboratories and AI agents · consent, provenance and an audit built in.
</p>

<p align="center">
  <a href="https://developers.anpheros.com/guides/"><b>Developer guides</b></a> ·
  <a href="https://developers.anpheros.com/docs">API reference</a> ·
  <a href="https://github.com/anpheros/anpheros-sdk">SDKs &amp; examples</a> ·
  <a href="https://platform.anpheros.com/">Platform</a> ·
  <a href="https://anpheros.com/">anpheros.com</a>
</p>

<p align="center">
  <a href="https://pub.dev/packages/anpheros_sdk"><img alt="pub.dev" src="https://img.shields.io/pub/v/anpheros_sdk?label=pub.dev%20anpheros_sdk"></a>
  <a href="https://www.npmjs.com/package/@anpheros/sdk"><img alt="npm" src="https://img.shields.io/npm/v/%40anpheros%2Fsdk?label=npm%20%40anpheros%2Fsdk"></a>
  <a href="https://github.com/anpheros/anpheros-sdk/actions/workflows/ci.yml"><img alt="CI" src="https://github.com/anpheros/anpheros-sdk/actions/workflows/ci.yml/badge.svg"></a>
  <img alt="FHIR R4" src="https://img.shields.io/badge/HL7%20FHIR-R4%20(4.0.1)-0e1a33">
  <img alt="License" src="https://img.shields.io/badge/SDK%20license-Apache--2.0-44619e">
</p>

---

## What Anpheros is

Anpheros is the **medical data layer** a healthcare product is built on. Each person has one record made of standard **HL7 FHIR R4** resources — not a proprietary schema — and applications read and write it through an API:

- **for patients** it is a digital medical record they control (the Anpheros Daily app);
- **for developers** it is a managed FHIR data store — a FHIR backend — with patient consent, provenance on every write, an access log the patient can see, and an **AI context API** that prepares the relevant part of a record for the model of your choice.

Anpheros' own patient app is a client of the same public API as everyone else, so what it records is available — with the patient's consent — to other applications in the same FHIR form.

```
  Your application · clinic system · lab system · AI assistant or agent
                         │  HTTPS — API key (your patients) or OAuth token (one consented patient)
                         ▼
  Anpheros API    /fhir/R4 (strict FHIR)   ·   /v1 (simplified REST, same data and ids)   ·   /oauth (consent)
                         │  isolation per application · scopes · provenance · access log
                         ▼
  One HL7 FHIR R4 record per patient — every version kept — shared only with the patient's consent
```

## Why developers use it

| Instead of building… | …you use |
|---|---|
| a medical data model and tables | **26 FHIR R4 resource types**: observations, conditions, medications, documents, immunizations, allergies, encounters, care plans and more |
| export formats for every partner | **FHIR R4** in and out, **International Patient Summary** documents, **SMART on FHIR** for third-party apps |
| a consent system | **OAuth 2.1 + PKCE**, SMART v2 patient scopes, grants of 30–365 days that the patient can revoke at any time |
| an audit trail | **provenance on every write** (patient, practitioner, device, import or AI) and **every read visible to the patient** |
| lab integrations | a **lab connector** for CSV, **HL7 v2 ORU^R01** and JSON reports, coded to **LOINC** |
| retrieval code for an LLM | an **AI context API**: the relevant sections of a record within a token budget, every item labelled with its source |
| polling and sync jobs | **signed, retried webhooks** |
| client libraries | **SDKs for TypeScript and Dart / Flutter**, with idempotent writes, retries, token refresh and the consent flow |

## Quick start

Get a sandbox key instantly: sign in with Google on the [dashboard](https://platform.anpheros.com/dashboard/) and press **Get a sandbox key**. The sandbox is free, and every sandbox project comes with its own 30 synthetic patients.

```bash
npm install @anpheros/sdk          # or: dart pub add anpheros_sdk
```

```ts
import { Anpheros, apiKey } from '@anpheros/sdk';

const anpheros = new Anpheros({ auth: apiKey(process.env.ANPHEROS_KEY!) });   // sandbox key: sk_test_…
const patient = await anpheros.patients.create({ given: 'Ana', family: 'Example' });
await anpheros.observations.create(patient.id, {
  code: '85354-9', display: 'Blood pressure panel', category: 'vital-signs',
  effective_at: new Date().toISOString(), author_type: 'device',
  components: [{ code: '8480-6', value: 128, unit: 'mm[Hg]' }, { code: '8462-4', value: 82, unit: 'mm[Hg]' }],
});
const ctx = await anpheros.context.build({ patient: patient.id, task: 'weekly check-in', budget_tokens: 1500, format: 'text' });
```

Runnable examples — a quickstart in TypeScript and Dart, a consent web app, an LLM example and a FHIR transaction — are in **[anpheros-sdk/examples](https://github.com/anpheros/anpheros-sdk/tree/main/examples)**.

## Building AI healthcare applications and agents

Anpheros is not an AI platform and does not run models. It is the data layer an AI application or agent calls:

- **consented access** — an agent reaches one patient's record only through an OAuth grant, within the scopes the patient chose;
- **context, not raw dumps** — `POST /v1/context` returns the relevant sections within a token budget, labelled with author type and source, and lists what was left out;
- **facts kept apart from AI output** — values written by an AI are recorded as AI-authored and kept in a separate section of later contexts;
- **accountability** — every context request is recorded and visible to the patient.

It works with any model — hosted or local — because the model call is your own code. Guides: [AI healthcare applications](https://developers.anpheros.com/guides/ai-healthcare) · [AI agents and medical data](https://developers.anpheros.com/guides/ai-agents-medical-data) · [LLM applications and healthcare data](https://developers.anpheros.com/guides/llm-healthcare-data) · [Building with AI coding tools](https://developers.anpheros.com/guides/ai-developers)

## Standards

| Standard | In Anpheros |
|---|---|
| HL7 FHIR R4 (4.0.1) | native storage and API; CapabilityStatement at [`/fhir/R4/metadata`](https://platform.anpheros.com/fhir/R4/metadata) |
| International Patient Summary | `Patient/$summary`, validated with the official HL7 validator against the IPS implementation guide (0 errors) |
| SMART App Launch | standalone launch with PKCE and SMART v2 scopes; OpenID Connect `fhirUser` |
| HL7 v2 | `ORU^R01` laboratory results imported as FHIR Observations |
| Terminologies | LOINC, ICD-10, ATC, UCUM |

## Security and privacy, in short

EU data residency · isolation between applications with per-application patient identifiers · consent chosen and revocable by the patient · provenance on every write · an access log the patient can see · documents stored immutable and encrypted with a customer-managed key · a separate sandbox with synthetic patients. Details: [Security, privacy and data residency](https://developers.anpheros.com/guides/security).

## Repositories

| Repository | What it contains |
|---|---|
| [**anpheros-sdk**](https://github.com/anpheros/anpheros-sdk) | the official TypeScript and Dart / Flutter SDKs and runnable examples (Apache-2.0) |

## Status

Anpheros Platform is in **private beta**: EU-hosted, single zone without automatic failover, no contractual SLA yet. The sandbox is free and self-service: sign in with Google on the [dashboard](https://platform.anpheros.com/dashboard/) and get a key instantly, with 30 synthetic patients per sandbox project. Production is for verified organisations with a data processing agreement, and the first month of production is free. Not available yet: SMART EHR launch, a Model Context Protocol (MCP) server.

## For patients

**Anpheros Daily** is the patient and family app built on the same record — symptoms, vital signs, medications, lab results, documents, appointments and an assistant that answers with the person's own data. Android, macOS and the web: [anpheros.com](https://anpheros.com/).

## Contact

General: contact@anpheros.com · Support: support@anpheros.com · Privacy: privacy@anpheros.com · Security reports: see our [security policy](https://github.com/anpheros/.github/blob/main/SECURITY.md).

<sub>For AI assistants and search: [anpheros.com/llms.txt](https://anpheros.com/llms.txt) · [developers.anpheros.com/llms.txt](https://developers.anpheros.com/llms.txt) · [llms-full.txt](https://developers.anpheros.com/llms-full.txt)</sub>
