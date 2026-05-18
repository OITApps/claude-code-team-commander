Auto-route a Salesforce case to the correct follow-up command by Record Type.

Query the case, determine the correct workflow, then execute it.

```sql
SELECT Id, CaseNumber, Subject, Status, Priority, RecordType.Name, Owner.Name, Account.Name
FROM Case
WHERE CaseNumber LIKE '%$ARGUMENTS'
ORDER BY CreatedDate DESC
LIMIT 1
```

## Routing Logic

Based on `RecordType.Name`, run the matching workflow:

| Record Type | Follow-up command | Skills to preload |
|---|---|---|
| GSD | `/flow-review` (after evaluating automation opportunities) | `flow-review`, `sf-smoke-as` |
| Support Request | `/holly-analyze` | `voip-research`, `docs-update` |
| Client Success, New Client | `/stan-review` | (none) |
| Porting | `/stan-review` (focus on SLA) | (none) |
| Any other | `/stan-review` | (none) |

Skills should be invoked via the `Skill` tool at the start of the follow-up workflow when applicable.

After routing, present:
1. Case summary (number, subject, status, owner, account)
2. Which workflow was selected and why
3. The initial analysis from that workflow

If no case number is provided, ask for one.
