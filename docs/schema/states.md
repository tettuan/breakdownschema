# State Transition Schema

## Task and Process State Transitions

```mermaid
stateDiagram-v2
direction LR
  state Result <<choice>>

  [*] --> ToDo
  ToDo --> Doing
  Doing --> Result
  Result --> Error: Result False
  Result --> Crash: Stop
  Result --> Done: Result True
  Done --> [*]
  Crash --> [*]
  Error --> Retry
  Retry --> ToDo : Retry <= 5
  Retry --> [*] : Retry > 5
```

## State Descriptions

### ToDo
- Initial state
- Task is defined but not yet started

### Doing
- Execution state
- Task is currently being processed

### Result
- Decision point
- Determines next state based on execution result

### Done
- Normal completion
- Task completed successfully

### Error
- Error state
- Error occurred during task execution

### Crash
- Fatal error
- Process stopped in an unrecoverable state

### Retry
- Retry state
- Re-execute task after error 