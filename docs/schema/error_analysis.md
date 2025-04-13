# Error Analysis Schema

## Overview

```mermaid
graph TD
    A[Error Analysis] -->|Contains| B[Error Summary]
    A -->|Contains| C[Requirement Analysis]
    A -->|Contains| D[Error Details]
    
    B -->|Has| B1[Trigger Command]
    B -->|Has| B2[Previous Changes]
    B -->|Has| B3[Error Reason]
    
    C -->|Has| C1[Requirement Files]
    C -->|Has| C2[Design Files]
    C -->|Has| C3[Summary]
    
    D -->|Contains| E[Error Entry]
    E -->|Has| E1[File Info]
    E -->|Has| E2[Error Content]
    E -->|Has| E3[Error Analysis]
    E -->|Has| E4[Error Response]
```

## Structure

```mermaid
erDiagram
    ErrorAnalysis ||--|{ ErrorDetails : contains
    ErrorAnalysis {
        object errorSummary
        object requirementAnalysis
        array errorDetails
    }
    
    ErrorDetails {
        string fileName
        integer line
        object errorContent
        object errorAnalysis
        object errorResponse
    }
    
    ErrorDetails ||--|| ErrorContent : has
    ErrorContent {
        string message
        enum errorType
        enum errorSource
    }
    
    ErrorDetails ||--|| ErrorAnalysis : has
    ErrorAnalysis {
        integer occurrenceCount
        array relatedFiles
        boolean triggersOtherErrors
    }
    
    ErrorDetails ||--|| ErrorResponse : has
    ErrorResponse {
        string requirementJudgment
        string designJudgment
        string dependencyJudgment
        integer priority
    }
```

## Error Types

```mermaid
classDiagram
    class ErrorType {
        <<enumeration>>
        code
        data
        syntax
    }
    
    class ErrorSource {
        <<enumeration>>
        typescript
        deno
        application
    }
```

## Error Analysis States

```mermaid
stateDiagram-v2
    [*] --> Analysis
    Analysis --> ErrorCollection
    ErrorCollection --> RequirementCheck
    RequirementCheck --> PriorityAssignment
    PriorityAssignment --> [*]
```

## Attributes

### Error Content
- **message**: Actual error message text
- **errorType**: 
  - `code`: Logic or implementation error
  - `data`: Data structure or format error
  - `syntax`: Language syntax error
- **errorSource**:
  - `typescript`: TypeScript compiler error
  - `deno`: Deno runtime error
  - `application`: Application-specific error

### Error Analysis
- **occurrenceCount**: Total number of times this error has occurred
- **relatedFiles**: List of files related to the error chain
- **triggersOtherErrors**: Whether this error triggers other cascade errors

### Error Response
- **requirementJudgment**: Decision to fix code or update requirements
- **designJudgment**: Decision to fix code or update design
- **dependencyJudgment**: Decision to fix this error directly or address dependencies first
- **priority**: Integer priority ranking (1-5)
  - `5 (Critical)`: Blocks system operation or causes data corruption
  - `4 (High)`: Severely impacts core functionality
  - `3 (Medium)`: Affects non-critical functionality or has workarounds
  - `2 (Low)`: Minor issues with minimal impact
  - `1 (Trivial)`: Cosmetic issues or enhancement requests

```mermaid
classDiagram
    class Priority {
        <<enumeration>>
        5: Critical
        4: High
        3: Medium
        2: Low
        1: Trivial
    }
``` 