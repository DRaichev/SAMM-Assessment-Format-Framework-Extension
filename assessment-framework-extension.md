# Assessment Framework Extension Specification

## Overview

| Property | Value |
|----------|-------|
| **Extension Name** | Assessment Framework |
| **Version** | 1.0.0 |
| **Purpose** | Identifies the security maturity framework used for the assessment |

## Structure

The extension is added to the `extensions` array within the assessment object:

```json
{
  "formatVersion": "1.0.0",
  "assessment": {
    "version": "1.0.0",
    "organization": "Example Organization",
    "scope": "Development Team",
    "date": "2025-01-15",
    "answers": [...],
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

## Fields

### Extension Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Must be exactly `"Assessment Framework"` |
| `version` | string | Yes | Semantic version of the extension (e.g., `"1.0.0"`) |
| `assessmentFramework` | string | Yes | Name of the framework used for the assessment |

### assessmentFramework Field

The `assessmentFramework` field contains the name of the security maturity or compliance framework under which the assessment was conducted.

**Common values include:**
- `"SAMM"` - OWASP Software Assurance Maturity Model
- `"BSIMM"` - Building Security In Maturity Model
- `"ISO 27001"` - ISO/IEC 27001 Information Security
- `"NIST CSF"` - NIST Cybersecurity Framework
- `"SOC 2"` - Service Organization Control 2

The field accepts any non-empty string, allowing for custom or proprietary frameworks.

## Behavior

### Export

When exporting an assessment to `.samm` format:

1. The extension is automatically included in all exports
2. The `assessmentFramework` value is populated from the assessment's configured framework
3. The extension appears in the `extensions` array alongside other extensions

### Import

When importing a `.samm` file containing this extension:

1. The importing tool reads the `assessmentFramework` value
2. The tool verifies the framework exists and the user has access to it
3. If the framework is unavailable or access is denied, the import fails with an appropriate error
4. If valid, the assessment is created under the specified framework

### Validation

Import tools should validate:

- The `name` field equals `"Assessment Framework"`
- The `version` field is a valid semantic version string
- The `assessmentFramework` field is a non-empty string
- The specified framework is available in the importing system

## Examples

### Example 1: SAMM Assessment

```json
{
  "name": "Assessment Framework",
  "version": "1.0.0",
  "assessmentFramework": "SAMM"
}
```

### Example 2: Combined with Other Extensions

```json
{
  "formatVersion": "1.0.0",
  "assessment": {
    "version": "1.0.0",
    "organization": "Acme Corp",
    "scope": "Security Team",
    "date": "2025-06-15",
    "answers": [
      {"questionCode": "G-SM-A-1", "answerScore": 0.5}
    ],
    "extensions": [
      {
        "name": "Assessment Stream Remarks",
        "version": "1.0.0",
        "assessmentStreamRemarks": {
          "G-SM-A": [
            {"text": "Good progress on governance", "type": "COMMENT"}
          ]
        }
      },
      {
        "name": "Assessment Framework",
        "version": "1.0.0",
        "assessmentFramework": "SAMM"
      }
    ]
  }
}
```

### Example 3: Custom Framework

```json
{
  "name": "Assessment Framework",
  "version": "1.0.0",
  "assessmentFramework": "Internal Security Maturity Model v2"
}
```

## Schema Validation

The extension is validated using JSON Schema (Draft 2020-12). Key validation rules:

1. **Required fields**: `name`, `version`, and `assessmentFramework` must all be present
2. **Name validation**: The `name` field must equal `"Assessment Framework"`
3. **Version format**: The `version` field must be a non-empty string
4. **Framework validation**: The `assessmentFramework` field must be a non-empty string

See [assessment-framework-extension.schema.json](assessment-framework-extension.schema.json) for the complete schema.

## Error Handling

| Scenario | Recommended Action |
|----------|-------------------|
| Missing `assessmentFramework` field | Reject import with validation error |
| Empty `assessmentFramework` value | Reject import with validation error |
| Unknown framework name | Reject import with "framework not found" error |
| Framework access denied | Reject import with "access denied" error |
| Invalid `name` field | Reject import with validation error |

## Future Considerations

Potential enhancements for future versions:

- **Framework version**: Add optional field for framework version (e.g., SAMM 2.0 vs 2.1)
- **Framework URI**: Add optional field linking to framework documentation
- **Mapping information**: Include data about how questions map between frameworks

## Changelog

### Version 1.0.0

- Initial release
- Basic framework identification with `assessmentFramework` field
