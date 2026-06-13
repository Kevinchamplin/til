# Status `ENUM` truncation fails silently in the producer

Insert or update a column typed `ENUM(...)` with a value that isn't in the allowed set and MySQL (outside strict mode) emits **warning 1265 "Data truncated"** and writes an empty string — it does *not* error by default. In Laravel that surfaces as a `QueryException` only if strict mode catches it; otherwise the row just keeps its old/blank status and the bug hides downstream.

I got burned "unifying" a status name between the code that *writes* status and the code that *reads* it: I renamed the value in the producer without checking the column definition, the new value wasn't in the `ENUM`, and rows quietly stopped advancing.

## Fix / habit

Before introducing any new status string, grep the migration for the actual enum:

```bash
grep -rn "enum(" database/migrations
```

If the value isn't there, it needs an `ALTER TABLE ... MODIFY COLUMN status ENUM(...)` migration — you can't just start writing it.

## Takeaway

`ENUM` is a closed set enforced at the schema, not the app. New value → migration first. And run MySQL in strict mode so truncation throws instead of whispering.
