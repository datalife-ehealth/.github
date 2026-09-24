<div align="center">
  <img src="banner.png" alt="DataLife e-Health Banner" width="460" style="max-width: 100%; height: auto;" />
</div>

<h1 align="center">DataLife e-Health</h1>

<p align="center">
  Patient-centric open architecture for heterogeneous health records.<br>
  Dual-tier storage. Dual-key access. Cryptographic audit inside one administrative boundary.
</p>

<p align="center">
  <a href="https://github.com/datalife-ehealth/datalife-datalake-core">Core engine</a>
  ·
  <a href="../CONTRIBUTING.md">Contributing</a>
  ·
  <a href="../CODE_OF_CONDUCT.md">Code of Conduct</a>
</p>

## Mission

DataLife e-Health separates what identifies a person from the clinical record that clinicians and public-health systems need to keep. Personal identifiers stay on a device the patient controls. The data lake stores immutable exam payloads, device XML, and imaging metadata, and every access is written into a tamper-evident ledger.

The value proposition is practical:

- Patients decide who may read a record, and for how long.
- Clinicians receive a time-bounded token instead of a standing account on the personal store.
- An unconscious patient can still be treated: a verified physician may open an emergency session, which is logged before care and ratified by the patient afterwards.
- Operators can prove that a stored block was not rewritten, without operating a public chain or a miner network.

## System boundaries

This organization publishes reference software. It is local-first, reproducible, and free of paid cloud APIs. It is not a certified electronic health record, not a hospital information system, and not a distributed blockchain. The audit log is a cryptographic tamper-evident ledger: chained Merkle roots over canonical records, verified inside a single organization or a small consortium.

## Architecture & Security Boundary

DataLife e-Health enforces strict zero-trust separation between Patient Identifiable Information (PII) and raw clinical observation payloads. Personal identifiers and medical examination streams never share a unified write path or administrative domain.

### 1. Dual-Tier Decoupled Storage Topology

The system operates across two physically and logically independent tiers to ensure strict compliance with LGPD/GDPR frameworks:

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'fontSize': '14px', 'primaryTextColor': '#0F172A', 'lineColor': '#4A5568', 'edgeColor': '#4A5568'}}}%%
flowchart TB
    subgraph Client["📱 Tier 1: Client Sovereign Device"]
        P["👤 Patient Owner"]
        Vault[("🔒 Local Encrypted PII<br/>(Name, Tax ID, Phone)")]
        OTPGen["🔑 Ephemeral OTP Generator"]
        P --> Vault
        P --> OTPGen
    end

    subgraph Cloud["☁️ Tier 2: Consortium Data Lake"]
        Ingest["⚡ Ingestion API Engine"]
        Lake[("📦 Raw Observation Store<br/>(DICOM Imaging, Lab XML)")]
        Ledger[("🛡️ Relational Merkle Ledger<br/>(Tamper-Proof Audit Chain)")]
        Ingest --> Lake
        Ingest --> Ledger
    end

    OTPGen ==>|"1. Ephemeral Grant Token"| Ingest
    Hospital["🏥 Clinical Modalities / Hospitals"] ==>|"2. Raw Payload Upload"| Ingest

    classDef c1 fill:#EFF6FF,stroke:#3B82F6,stroke-width:1.5px,color:#1E3A8A;
    classDef c2 fill:#F8FAFC,stroke:#64748B,stroke-width:1.5px,color:#0F172A;
    classDef ext fill:#FFFFFF,stroke:#94A3B8,stroke-width:1px,color:#334155;
    class Client c1;
    class Cloud c2;
    class Hospital ext;
```

#### Isolation invariants

- **Zero PII replication.** The cloud relational tier stores pseudonymous entity hashes and payload pointers. A complete compromise of the central database leaks zero decipherable personal names, addresses, or identifiers.
- **Deterministic provenance.** Canonical payloads are indexed against cryptographic Merkle trees. Modification of past observation bytes invalidates the chained root immediately.

### 2. Dual-Key Access & Glass-Break Protocol

Observation access requires mutual validation. Under normal workflows, patients govern disclosure. In acute clinical emergencies, a strict, auditable override protects patient survival without compromising downstream non-repudiation.

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'fontSize': '14px', 'lineColor': '#4A5568', 'edgeColor': '#4A5568'}}}%%
flowchart TD
    Req["🩺 Clinician Requests Record Access"]
    Check{"Patient Conscious & Able to Authorize?"}

    Req --> Check

    %% Routine Care Track
    subgraph StandardTrack["Routine Care Protocol"]
        OTP["📱 Patient Issues Ephemeral OTP"]
        Val["⏳ Validate TTL & Granular Scope"]
        OTP --> Val
    end

    %% Emergency Glass-Break Track
    subgraph EmergencyTrack["Emergency Glass-Break Protocol"]
        Glass["🚨 Verified Physician ID Injected"]
        Audit[("🔒 Pre-Flight Merkle Audit Committed")]
        Sess["⚡ Ephemeral Emergency Session Opened"]
        Glass --> Audit --> Sess
    end

    Check -->|"Yes: Routine Care"| OTP
    Check -->|"No: Acute Emergency"| Glass

    Read["🔓 Access Granted: Decrypted Clinical Payload"]
    Val --> Read
    Sess --> Read

    Dispute["📋 Mandatory Post-Care Patient Dispute Window"]
    Sess -.-> Dispute

    classDef normal fill:#F0FDF4,stroke:#16A34A,stroke-width:1.5px,color:#14532D;
    classDef alert fill:#FEF2F2,stroke:#DC2626,stroke-width:1.5px,color:#7F1D1D;
    classDef neutral fill:#F8FAFC,stroke:#475569,stroke-width:1.5px,color:#0F172A;

    class Req,Check,Read neutral;
    class OTP,Val normal;
    class Glass,Audit,Sess,Dispute alert;
```

#### Protocol guarantees

- **Pre-flight tamper-evident logging.** In a glass-break override, access is written into the cryptographic Merkle chain before observation bytes are returned to the clinical display.
- **Post-incident dispute window.** When patient consciousness is restored, the client agent flags the unratified emergency session, creating an immutable audit dispute log.

## Repository ecosystem

| Repository | Status | Role |
|---|---|---|
| [datalife-datalake-core](https://github.com/datalife-ehealth/datalife-datalake-core) | Active core | Ingestion, OTP and glass-break access, Merkle verification, PostgreSQL schema |
| datalife-web-portal | RFC / Help Wanted | Clinical and teleregulation dashboard |
| datalife-mobile-app | RFC / Help Wanted | Patient application: exam list and OTP issuance |
| datalife-infra-devops | RFC / Help Wanted | Container, cluster, and delivery automation |

`datalife-datalake-core` is the only repository with a maintained implementation today. The other three are reserved names. Open a contribution task in this profile repository before starting one of them, so the boundary with the core engine stays explicit.

## Roadmap

1. **Core, now.** Multi-modal ingest, dual-key access, and Merkle verification in `datalife-datalake-core`.
2. **Patient client.** A mobile application that holds PII locally and mints OTP grants. It must not upload the personal tier.
3. **Clinical portal.** A web console for consented reads and for reviewing glass-break sessions that still need ratification.
4. **Delivery.** Repeatable containers and cluster manifests in `datalife-infra-devops`, after the core API is stable.
5. **Interoperability notes.** Document how DICOM metadata, device XML, and relational extracts land in the same ledger without collapsing them into one schema.

## Contributing

Read [CONTRIBUTING.md](../CONTRIBUTING.md) and the [Code of Conduct](../CODE_OF_CONDUCT.md). Claim work with the contribution-task issue template. Branch names use `feat/`, `fix/`, or `docs/`.

Maintainer: Luchang Jiang ([@FinalSunFlower](https://github.com/FinalSunFlower)).
