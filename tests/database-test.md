## Workflow: Database Connection Test

**Trigger:** This workflow is activated when the user types the exact command `/testdb`.

**Goal:** To verify that the AI can successfully use the `mysql_query` tool to communicate with the Okabooks database and receive a valid response.

**Step 1: Acknowledge the Command**
* **Rationale:** The user has requested a database connection test. I need to execute a simple, safe, read-only query to confirm the connection is alive. The safest query is to count the number of users in the `users` table.
* **Action:** Use the `mysql_query` tool with the following `sql` parameter:
    `"SELECT COUNT(*) FROM users;"`

**Step 2: Report the Result**
* **Rationale:** The tool will either return a result or an error. I must inform the user of the outcome.
* **Action (On Success):** If the tool returns a result (it will be a number, like `[{'COUNT(*)': 5}]`), respond to the user with the following message:
    `"✅ Database connection successful! I found [number] records in the users table."`
* **Action (On Failure):** If the tool returns an error, respond to the user with the following message:
    `"❌ Database connection test failed. Please check the application logs for more details."`
