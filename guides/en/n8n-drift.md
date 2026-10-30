# n8n 2.x: what runs is not what the editor shows

[Українська](../n8n-drift.md) | **English**

The editor and direct SQL edits change `workflow_entity`. A published workflow runs from `workflow_history`,
the version `activeVersionId` points to. When the two differ, nothing fails and nothing warns you.

**For:** anyone running their own n8n 2.x on Postgres who has edited a workflow in the database or by script.
**Not for:** you edit only in the editor and publish every time. n8n Cloud gives no database access.

## What it cost me

- An edit only in `workflow_entity`: the post language changed in the database and in every check I ran, while the workflow kept working the old way for days.
- The same with a fix: it sat in the database for a day and never ran. I was checking the same table I had edited, so everything looked fine.

## A 1-minute check

Read only. It lists active workflows where the two tables differ:

```sql
SELECT we.name
FROM workflow_entity we
LEFT JOIN workflow_history wh ON wh."workflowId" = we.id AND wh."versionId" = we."activeVersionId"
WHERE we.active
  AND (wh."versionId" IS NULL OR we.nodes::jsonb <> wh.nodes::jsonb OR we.connections::jsonb <> wh.connections::jsonb);
```

An empty result is not proof yet. Test the query on a planted difference in a temporary table, or you do not know that it can see anything.

## The rule

- Editing the database directly: back up first (`pg_dump` of both tables), change both tables in one transaction, then restart n8n and run a live test.
- Checking an edit: only `workflow_history` at `activeVersionId`, not `workflow_entity`.
- A difference is not always a bug: an unpublished draft in the editor also shows up.

Checked on n8n 2.25 and 2.41. A later version may name the tables differently.
A ready daily script with a canary: [n8n-drift-check](https://github.com/ihorkhamuliak/n8n-drift-check).
