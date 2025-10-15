## Problem: First Letter Capitalization II

**Schema**

```sql
Table: user_content  
+-------------+-----------------+  
| Column Name | Type            |  
+-------------+-----------------+  
| content_id  | INT             |  
| content_text| VARCHAR         |  
+-------------+-----------------+  
content_id is the primary key.
```

### **Problem Statement**

You need to transform the `content_text` of each row according to these rules:

1. **Capitalize the first letter of every word** and **lowercase all other letters** in each word.
2. **Hyphenated words** (words containing `-`) are a special case: each part of the hyphenated word should be treated independently.

   * e.g. `"QUICK-brown"` becomes `"Quick-Brown"`
3. **Preserve other characters, spacing, and punctuation** exactly as in the original string (apart from the letter-case transformations).
4. Return a result that includes:

   * `content_id`
   * `original_text` (same as `content_text`)
   * `converted_text` (the transformed version)

You can return the rows in **any order**.

### **Example**

**Input (user_content table):**

| content_id | content_text                    |
| ---------- | ------------------------------- |
| 1          | hello world of SQL              |
| 2          | the QUICK-brown fox             |
| 3          | modern-day DATA science         |
| 4          | web-based FRONT-end development |

**Expected Output:**

| content_id | original_text                   | converted_text                  |
| ---------- | ------------------------------- | ------------------------------- |
| 1          | hello world of SQL              | Hello World Of Sql              |
| 2          | the QUICK-brown fox             | The Quick-Brown Fox             |
| 3          | modern-day DATA science         | Modern-Day Data Science         |
| 4          | web-based FRONT-end development | Web-Based Front-End Development |

* In row 2, `"QUICK-brown"` → `"Quick-Brown"`
* In row 3, `"modern-day"` → `"Modern-Day"`, and `"DATA"` → `"Data"`
* In row 4, both `"web-based"` and `"FRONT-end"` are handled correctly with hyphens.
If you like, I can send you **hints / the approach** (not full solution) next. Do you want me to share that?

---
**SOLUTION:**
_WRONG_
```sql
SELECT
  content_id,
  content_text AS original_text,
  REGEXP_REPLACE(
    LOWER(content_text),
    '(^| |-)([a-z])',
    CONCAT('\\1', UPPER('\\2'))
  ) AS converted_text
FROM user_content
```

