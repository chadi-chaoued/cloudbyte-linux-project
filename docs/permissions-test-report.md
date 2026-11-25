
# CloudByte Permissions Test Report: Section 2

Date: 2026-11-25

| User  | Action                                              | Expected | Observed |
|-------|-----------------------------------------------------|----------|----------|
| alice | touch in /shared/engineering                        | allow    | allow; file tagged group engineering (setgid) |
| -     | verify team directory ownership and permissions     | correct ownership and mode | passed for all three team directories |
| -     | verify files in team directories                     | all files have the correct team group | all files passed |
| emma  | test write permission in /shared/dropbox             | allow    | allow |
| emma  | ls /shared/dropbox                                  | deny     | deny     |
| -     | verify /shared/dropbox ownership and permissions     | root:admins, sticky bit set | passed |
| -     | verify required file extensions                     | all required extensions present | passed |
| -     | search for ERROR in /shared                         | at least one match | passed |

## Notes

- Setgid (mode `2770`) makes "which files does the engineering team own?" an
  answerable question. Without it, ownership tracks the creator's primary group
  and the team-level view falls apart.
- The sticky bit (mode `1773`) protects submissions from cross-user deletion.
- The dropbox allows writing while preventing users from listing its contents.

## Verification Script

Command executed inside the Lima VM:

```bash
bash /host/verify-permissions.sh
```

Result: 18 passed, 0 failed.
