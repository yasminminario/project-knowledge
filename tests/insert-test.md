## Workflow: Database INSERT Test (`/testinsert`)

**Trigger:** This workflow is activated when the user types the exact command `/testinsert`.

**Goal:** To verify that the AI can successfully use the `mysql_query` tool to perform an INSERT operation into the `projects` table. This also tests if the `ALLOW_INSERT_OPERATION` flag is correctly set to "true".

**Step 1: Acknowledge the Command and Prepare the Data**
* **Rationale:** The user has requested an INSERT test. I will use a predefined set of hardcoded data to create a new record in the `projects` table. This is a safe way to test the write capability.
* **Action:** Use the `mysql_query` tool with the following `sql` parameter. The SQL statement will insert a new project with the specified details.

    ```sql
    INSERT INTO projects (name, type, description, image_url, user_id, full_name, birth_date, gender, biography_focus, chapter_sequence, interaction_count, is_locked) VALUES ('biografia6', 'autobiography', NULL, '#6D4C41', 'A3bPtppYiPRAXQwcaLbXhuwHuYu2', 'Yasmin Minário', '2006-05-02', 'ela/dela', 'Histórias sobre mim', '["childhood", "family", "education_learning", "affective_life", "career", "political_causes", "general_themes"]', 0, 0);
    ```

**Step 2: Report the Result**
* **Rationale:** I need to inform the user if the INSERT operation was successful or if it failed. A successful INSERT query usually returns an "affected rows" count of 1.
* **Action (On Success):** If the tool executes without error, respond to the user with the following message:
    `"✅ INSERT test successful! A new project record for 'biografia6' was created in the projects table. You can verify this by checking the database directly."`
* **Action (On Failure):** If the tool returns an error, respond to the user with the following message:
    `"❌ INSERT test failed. The most likely cause is that the 'ALLOW_INSERT_OPERATION' flag is set to 'false' in the config file. Please check the application logs for more details."`
