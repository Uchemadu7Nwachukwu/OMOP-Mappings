# OMOP CDM Vocabulary Mapping Pipeline

## Project Overview
This repository documents the structural engineering and clinical terminology mapping phase of an ETL pipeline. It transforms localized healthcare concepts (OpenMRS data schema) into the Standardized Vocabularies of the **OMOP Common Data Model (CDM)** to ensure global semantic interoperability for large-scale observational health research.

## Core Technical & Domain Competencies
* **Data Standards:** OMOP CDM v6.0, OpenMRS Data Dictionary
* **Mapping Infrastructure:** OHDSI Usagi Semantic Mapping Engine
* **Target Standard Vocabularies:** SNOMED-CT (Conditions/Diagnoses), RxNorm (Medications), LOINC (Measurements/Vitals)
* **Data Engineering Layouts:** Tabular Schema Mapping (`.csv`), Relational Database Structuring

## Repository Architecture & Artifacts
The files included reflect a production-ready terminology alignment workflow:
* **`usagi_mappings.csv`**: The primary mapping matrix linking source clinical text terms to unique standard OMOP concept IDs using string-similarity scores and manual clinical validation rules.
* **`condition_mappings.csv`**: Focused terminology mapping subsets specifically isolating diagnostic concepts into standard SNOMED-CT clinical targets.
* **`DomainIds.txt` / `ConceptClassIds.txt` / `VocabularyIds.txt`**: Architectural constraint criteria utilized during the Usagi mapping configuration to strictly filter down target code scopes.

## Functional Mapping Strategy
1. **Extraction:** Raw source strings and custom concepts are exported from the source system database engine.
2. **Scoring:** The source concepts are fed into the OHDSI Usagi engine to calculate Jaro-Winkler textual similarity scores against standard target vocabulary dictionaries.
3. **Clinical Validation:** Mapping relationships are filtered by setting precise `relationship_id = 'Maps to'` criteria, resolving unmapped elements, and tracking data versioning through the included `vocabularyVersion.txt`.

---
*This repository serves as a professional portfolio artifact demonstrating expertise in Health Informatics, Clinical Data Architecture, and OHDSI Global Data Standards.*
