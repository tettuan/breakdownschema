# Issue Fix Schema

This document describes the structure and usage of issue fix files in the Breakdown system.

## Overview

An issue fix file is a JSON document that describes a single issue, its analysis, and solution. Each file follows a strict schema that ensures all necessary information is consistently captured.

## Schema Structure

### Root Properties

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| issueId | Yes | String | Unique identifier (format: YYYYMMDD-description) |
| title | Yes | String | Brief description of the issue |
| gap | Yes | Object | Gap between ideal and current state |
| errorSummary | Yes | Object | Summary of error situation |
| analysis | Yes | Object | Detailed analysis of the issue |
| solution | Yes | Object | Proposed solution and tasks |
| references | Yes | Array | Related files and documents |
| scope | Yes | String | Issue scope: "minimal" or "comprehensive" |
| mingoal | Yes | Object | Minimum functionality, test, and proof requirements |

### Gap Analysis

Describes the gap between ideal and current state:

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| ideal | Yes | String | Description of desired state |
| current | Yes | String | Description of current state |
| impact | Yes | String | Impact of this gap on the project |
| minGoalLink | Yes | String | Link to minimum goal document |

### Error Summary

Contains direct context of the error:

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| command | Yes | String | Command that triggered the error |
| previousChanges | Yes | String | Code changes before the error |
| reason | Yes | String | Overall error description |

### Analysis

Detailed breakdown of the issue:

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| requirements | Yes | String[] | Requirement file paths |
| design | Yes | String[] | Design file paths |
| errors | Yes | Object[] | List of specific errors |

#### Error Details

Each error in the errors array contains:

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| file | Yes | String | Location of error file |
| line | Yes | Integer | Error line number |
| message | Yes | String | Error message |
| type | Yes | Enum | Error type: code/data/syntax |
| source | Yes | Enum | Error source: typescript/deno/application |
| analysis | Yes | Object | Detailed error analysis |
| priority | Yes | Integer | Priority level (1-5) |

##### Error Analysis Properties

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| occurrenceCount | Yes | Integer | Number of times error occurred |
| relatedFiles | Yes | String[] | Files related to this error |
| triggersOtherErrors | Yes | Boolean | Whether error triggers other errors |

### Solution

Proposed fix and implementation plan:

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| approach | Yes | String | Overall solution approach |
| tasks | Yes | Object[] | Implementation tasks |
| minGoalAlignment | Yes | String | How solution aligns with minimum goal |

#### Task Properties

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| id | Yes | String | Task identifier |
| description | Yes | String | Task description |
| type | Yes | Enum | Task type: code/test/config/docs |
| status | Yes | Enum | Status: pending/in-progress/completed |

### References

Links to related files:

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| type | Yes | Enum | Reference type: requirement/design/test/code/mingoal |
| path | Yes | String | Path to reference file |

### Minimum Goal

Defines minimum functionality and its verification:

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| function | Yes | Object | Target functionality definition |
| test | Yes | Object | Test requirements |
| proof | Yes | Object | Solution verification |

#### Function Definition

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| description | Yes | String | Description of minimum functionality |
| requirements | Yes | String[] | List of specific requirements |

#### Test Definition

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| description | Yes | String | What needs to be tested |
| criteria | Yes | String[] | Test success criteria |

#### Proof Definition

| Property | Required | Type | Description |
|----------|----------|------|-------------|
| description | Yes | String | How to prove the solution works |
| verificationMethod | Yes | Enum | Method: test/demo/review |

## Usage Example

```json
{
  "issueId": "20250209-typescript-config",
  "title": "TypeScript Configuration Error in Path Resolution",
  "gap": {
    "ideal": "TypeScript path aliases should resolve correctly for all imports",
    "current": "Path aliases fail to resolve in specific test files",
    "impact": "Prevents running tests and blocks CI pipeline",
  },
  "errorSummary": {
    "command": "deno test",
    "previousChanges": "Updated import map configuration",
    "reason": "Import map configuration conflicts with TypeScript paths"
  },
  "analysis": {
    "requirements": ["draft/20250207-defect.md"],
    "design": ["draft/20250207-design.md"],
    "errors": [
      {
        "file": "src/utils/path.ts",
        "line": 42,
        "message": "Cannot find module '@types/command.ts'",
        "type": "code",
        "source": "typescript",
        "analysis": {
          "occurrenceCount": 3,
          "relatedFiles": ["src/cli/breakdown.ts"],
          "triggersOtherErrors": true
        },
        "priority": 1
      }
    ]
  },
  "solution": {
    "approach": "Update import map configuration to align with TypeScript path aliases",
    "tasks": [
      {
        "id": "FIX-001",
        "description": "Update import map paths",
        "type": "config",
        "status": "pending"
      }
    ],
    "minGoalAlignment": "Directly addresses core functionality requirement of working imports"
  },
  "references": [
    {
      "type": "design",
      "path": "draft/20250207-design.md"
    },
    {
      "type": "mingoal",
      "path": "docs/mingoal.md"
    }
  ],
  "scope": "minimal",
  "mingoal": {
    "function": {
      "description": "TypeScript path alias resolution in test files",
      "requirements": [
        "Must resolve all import aliases in test files",
        "Must maintain compatibility with Deno import maps"
      ]
    },
    "test": {
      "description": "Verify path alias resolution in test environment",
      "criteria": [
        "All test files can import using aliases",
        "No TS2307 errors in test execution"
      ]
    },
    "proof": {
      "description": "Run test suite with path aliases",
      "verificationMethod": "test"
    }
  }
}
```

## Notes

- All file paths should be relative to project root
- Priority levels range from 1 (highest) to 5 (lowest)
- Task IDs should follow the format FIX-NNN
- IssueId dates should use YYYYMMDD format
- Scope values:
  - "minimal": Issue directly addresses minimum goal
  - "comprehensive": Issue addresses broader improvements
- minGoalLink should reference specific sections in docs/mingoal.md 