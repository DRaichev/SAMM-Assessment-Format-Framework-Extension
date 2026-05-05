# Assessment Framework Extension

This repository contains the specification and JSON Schema for the **Assessment Framework Extension**, an extension to the [SAMM Assessment Format](https://github.com/DRaichev/SAMM-Assessment-Format) that records which security maturity framework was used for an assessment.

## Overview

The Assessment Framework Extension provides a standardized way to identify the framework (e.g., OWASP SAMM, ISO 27001, NIST CSF) under which an assessment was conducted. This enables:

- **Interoperability**: Tools can identify the framework and apply appropriate processing logic
- **Validation**: Import tools can verify framework compatibility before processing
- **Traceability**: Assessment files are self-documenting about their source framework

## Repository Contents

| File | Description |
|------|-------------|
| [assessment-framework-extension.md](assessment-framework-extension.md) | Complete specification document |
| [assessment-framework-extension.schema.json](assessment-framework-extension.schema.json) | JSON Schema for validation |
| [examples/](examples/) | Valid and invalid example files |

## Quick Start

Add the extension to your `.samm` file's `extensions` array:

```json
{
  "formatVersion": "1.0.0",
  "assessment": {
    "version": "1.0.0",
    "organization": "Example Corp",
    "scope": "Product Team",
    "date": "2025-01-15",
    "answers": [],
    "extensions": [
      {
        "name": "Assessment Framework",
        "version": "1.0.0",
        "assessmentFramework": "SAMM"
      }
    ]
  }
}
```

## Extension Structure

```json
{
  "name": "Assessment Framework",
  "version": "1.0.0",
  "assessmentFramework": "<framework-name>"
}
```
    
### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Must be `"Assessment Framework"` |
| `version` | string | Yes | Extension version (currently `"1.0.0"`) |
| `assessmentFramework` | string | Yes | Name of the framework (e.g., `"SAMM"`, `"ISO 27001"`) |


## Related

- [SAMM Assessment Format](https://github.com/DRaichev/SAMM-Assessment-Format)
- [Assessment Stream Remarks Extension](https://github.com/DRaichev/SAMM-Assessment-Format-Remarks-Extension)

## License

This specification is released under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
