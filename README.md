## Script: Blocking_Chain_With_Root_Blocker.sql

**Description**:
This script identifies active **blocking chains** in SQL Server, highlighting both the **blocker and blocked sessions**, and traces each blocked session back to its **root blocker**. It provides detailed context including login, host, query text, and wait type.

---

### 🔧 How It Works:
- Uses a Common Table Expression (CTE) to gather blocking relationships
- Recursively walks up the blocking chain to find the **root blocker**
- Displays a full blocking tree ordered by root blocker and query start time

---

### 🧩 Key Components:

- `BlockingCTE`: Captures blocking/blocked session pairs, query text, session details
- `RootBlockers`: Recursively resolves the top-level blocker (where `blocking_session_id = 0`)
- Final `SELECT`: Combines both views and presents the blocking hierarchy

---

### 📋 Output Columns:

| Column               | Description                                              |
|----------------------|----------------------------------------------------------|
| `session_id`         | Session ID of the blocked request                        |
| `blocking_session_id`| Session ID of the blocking request (if any)              |
| `root_blocker_session_id` | The original blocking session at the top of the chain |
| `host_name`          | Host machine where the session originated                |
| `DB_Name`            | Database being accessed                                  |
| `login_name`         | SQL or Windows login used                                |
| `status`             | Session status (e.g., suspended, running)                |
| `command`            | SQL command being executed                               |
| `start_time`         | When the query started                                   |
| `cpu_time`           | CPU time used                                            |
| `total_elapsed_time` | Seconds since the query started                          |
| `wait_type`          | Type of wait (e.g., LCK_M_X, PAGELATCH_EX)               |
| `QueryText`          | SQL text being executed                                  |

---

### ✅ Use Case:
- Identify **blocking chains** causing performance issues
- Quickly locate the **root cause (root blocker)** for resolution
- Support proactive monitoring or generate incident diagnostics

---

### ⚠️ Notes:
- Excludes system databases for clarity
- `wait_type IS NOT NULL` filters only waiting sessions
- You can expand this by joining `sys.dm_exec_connections` for IP address/client info


