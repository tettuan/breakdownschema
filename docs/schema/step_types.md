# Step Types Schema

Available types for each task step:

|Type Name| Definition| Example|
|---|---|---|
|Execute Command| Execute the specified command| `bundle exec rails test` |
|Check Logs| Check logs to verify there are no issues| `tail -200 log/development.log` |
|Write Application Code| Write code that modifies application behavior| class Edinet ..., def initialize ...|
|Write Test Code| Write test code corresponding to application code||
|Write TDD Test Code| Write tests based on specifications using TDD||
|Git Commit| Commit current changes| `git commit` |
|Git Commit All| Commit all changes including unstaged ones| `git add . ; git commit` |
|Git Push| Push changes to remote| `git push` |

## Step Results

Record the execution results of each step by defining the result
- Messages obtained from the execution
- Evaluate the execution result to determine success/error

Example:
- step_result
  - code (optional) :
  - message:
  - judgment 