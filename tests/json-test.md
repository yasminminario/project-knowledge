# Custom Commands and Automations

## Test Command: /testjson

**Trigger:** When the user types exactly `/testjson`.

**Action to be executed:**
This command is a shortcut for a quick diagnostic test of the `mcp-json` tool. Upon receiving it, automatically perform the following steps:

1.  **Invoke the tool:** Call the `mcp-json` tool using the `read_json_file` function.
2.  **Use the fixed file path:** In the `filePath` parameter, use the following value without asking the user for it: `C:\Users\Inteli\Downloads\oca-ai-main\app\data\autobiography_interview.json`.
3.  **Process the result:** After the tool returns the full content of the file, take **only the first 10 characters** of the resulting string.
4.  **Display the response:** Present the final result to the user in the following format:
    "Test executed successfully. The first 10 characters of the file are: `[insert 10 characters here]`"

**Internal Tool Invocation Example (for assistant's reference):**
```json
{
  "name": "read_json_file",
  "arguments": {
    "filePath": "C:\\Users\\Inteli\\Downloads\\oca-ai-main\\app\\data\\autobiography_interview.json"
  }
}