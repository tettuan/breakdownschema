# Task Properties Schema

The following properties can be set for tasks:

|Property Name| Definition| Example|
|---|---|---|
|Work Branch| Git branch name to be used for the task. If not specified, the current branch will be used| cursor/edinet-api-20250123|
|Prohibited Files| Application files that are prohibited from editing| `routes.rb`, `migration/*`, `config/*` |

## Detailed Property Definitions

### Work Branch
- **Purpose**: Specifies the Git branch to be used during task execution
- **Format**: String
- **Default**: Current branch
- **Example**: `cursor/edinet-api-20250123`

### Prohibited Files
- **Purpose**: Specifies files that should not be edited
- **Format**: Array of strings or comma-separated string
- **Pattern**: Filename, directory name, or glob pattern
- **Example**: `routes.rb`, `migration/*`, `config/*` 