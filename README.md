# Semantic Harmonization Pipeline: OpenMRS to OMOP CDM Standardized Vocabulary Mapping

## 1. Executive Summary & Project Purpose
In modern clinical data analytics, the lack of standardized health terminologies presents a massive barrier to multi-center observational research and federated data networks (such as OHDSI, EHDEN, and PCORnet). Source Electronic Health Record (EHR) platforms often utilize localized, non-standard custom concept dictionaries that fail to achieve semantic interoperability out of the box.

This repository serves as an enterprise portfolio showcase, documenting the technical frameworks, metadata configurations, and terminology mapping artifacts required to align an **OpenMRS source clinical database** with the **OMOP Common Data Model (v6.0)**. By translating localized source terminologies into global standard concepts (**SNOMED-CT, RxNorm, and LOINC**), this pipeline prepares unstructured or semi-structured healthcare datasets for massive-scale observational health studies and predictive analytics.

---

## 2. Core Architectural & Domain Competencies
This project demonstrates production-level proficiency across a specialized clinical data engineering stack:
* **Data Standards Architecture:** Expert handling of relational EHR schemas (OpenMRS Core) and observational model designs (OMOP CDM v6.0).
* **Semantic Mapping Frameworks:** Enterprise deployment of the OHDSI tooling suite, specifically utilizing **OHDSI Usagi** for vocabulary alignment.
* **Controlled Medical Vocabularies:** In-depth execution of cross-vocabulary transformations mapping to:
  * **SNOMED-CT** (Systematized Nomenclature of Medicine -- Clinical Terms) for clinical conditions, symptoms, and diagnoses.
  * **RxNorm** for granular medication configurations, ingredient groupings, and clinical drug formulations.
  * **LOINC** (Logical Observation Identifiers Names and Codes) for physical measurements, laboratory values, and vital signs.
* **Data Quality & Governance:** Implementation of structural mapping rules, source-to-concept tracking, and rigorous handling of data version control.

---

## 3. Repository Architecture & Artifact Layout
The files contained within this repository represent the configuration outputs, structural matrices, and vocabulary boundary definitions generated during the mapping design cycle:

```text
├── .gitignore               # Excludes bulky system files, databases, and localized metadata caches
├── ConceptClassIds.txt       # Scope constraints isolating acceptable OMOP Concept Classes (e.g., Clinical Drug, Ingredient, Diagnosis)
├── DomainIds.txt            # Operational target boundary definitions restricting targets to specific domains (Condition, Drug, Measurement)
├── VocabularyIds.txt        # Hard constraints narrowing search scopes to standard source vocabularies (SNOMED, RxNorm, LOINC)
├── vocabularyVersion.txt    # Records the explicit version code of the Athena OMOP Standardized Vocabularies utilized
├── usagi_mappings.csv       # The master terminology mapping engine containing lexical similarities and clinical validations
└── condition_mappings.csv   # Target subset mapping file isolating and processing diagnostic code entities
4. End-to-End Mapping MethodologyThe data transformation pipeline follows a strict, repeatable framework to maintain high data fidelity and protect clinical context:Plaintext  [Source EHR Database] ──> Extract Raw Concept Strings 
                                    │
                                    ▼
  [OMOP Athena Portal]  ──> Download Target Vocabulary Vocab v2026
                                    │
                                    ▼
  [OHDSI Usagi Engine]  ──> Execute Jaro-Winkler Matching & Scoring
                                    │
                                    ▼
  [Clinical Validation] ──> Review Matches & Hardcode 'Maps to' Status
                                    │
                                    ▼
  [Target OMOP tables]  ──> Ready for DRUG_EXPOSURE / CONDITION_OCCURRENCE
Step 1: Source Concept ExtractionRaw alphanumeric concepts, text descriptors, and unique internal IDs are extracted directly from the source concept_name and orders layers of the local database.Step 2: Algorithmic Lexical MatchingExtracted concepts are ingested by OHDSI Usagi. The engine processes string similarity matching using a customized Jaro-Winkler textual distance algorithm against the downloaded standard OMOP vocabulary indices, generating an automated match score between $0.0$ and $1.0$.Step 3: Vocabulary & Domain ConstrainingTo prevent false-positive matches (e.g., mapping a drug name to a condition or a physical measurement), strict mapping filters are configured using the accompanying metadata parameters:Domain Restrictions (DomainIds.txt): Ensures medication strings are strictly evaluated against the Drug domain, and diagnostic strings against the Condition domain.Concept Class Constraints (ConceptClassIds.txt): Limits target structures to valid relational classes, such as mapping individual items to active pharmaceutical ingredients or exact clinical drug formulations.Step 4: Clinical Review & VerificationAutomated mappings are manually audited to resolve lexical ambiguities, handle edge cases, and manually map orphan codes. Valid associations are stamped with a standard relationship_id = 'Maps to', transforming the file into a structural blueprint for the final ETL execution.5. Downstream Target Model IntegrationOnce public and fully validated, the .csv mapping matrices generated in this phase serve as the lookup tables inside SQL or PySpark ETL scripts to load real patient profiles.usagi_mappings.csv drives the transition of drug concepts from the source tables directly into the OMOP standard DRUG_EXPOSURE data structure.condition_mappings.csv drives the schema mapping pipeline that converts source clinician notes and diagnostic codes into the standardized CONDITION_OCCURRENCE medical timeline table.
Contact Information: Developed by Uchemadu Nwachukwu, Clinical Data and Health Informatics Scientist.
