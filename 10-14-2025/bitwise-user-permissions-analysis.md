## Problem: Bitwise User Permissions Analysis

**Schema**

```sql
Table: user_permissions  
+-------------+-------------+  
| Column Name | Type        |  
+-------------+-------------+  
| user_id     | INT         |  
| permissions | INT         |  
+-------------+-------------+  
user_id is the primary key (unique for each row).
```

**Problem Statement**
Each row of the table `user_permissions` consists of a `user_id` and an integer `permissions`. Each bit in the `permissions` value represents whether the user has that specific permission (1 means has the permission, 0 means does not).

Write a SQL query to return a table with two columns: `common_perms` and `any_perms`:

* `common_perms`: The integer whose binary representation has 1 in each bit position where **all users** have that permission.
* `any_perms`: The integer whose binary representation has 1 in each bit position where **at least one user** has that permission.

**Return Format**
The result should be a single-row table:

| common_perms | any_perms |
| ------------ | --------- |

**Example**
Suppose the `user_permissions` table has:

| user_id | permissions |
| ------- | ----------- |
| 1       | 5           |
| 2       | 12          |
| 3       | 7           |
| 4       | 3           |

* Binary forms:

  * 5 = `0101`
  * 12 = `1100`
  * 7 = `0111`
  * 3 = `0011`
* `common_perms` is the bitwise AND of all: `0101 & 1100 & 0111 & 0011 = 0000` → 0
* `any_perms` is the bitwise OR of all: `0101 | 1100 | 0111 | 0011 = 1111` → 15

So the query should return:

| common_perms | any_perms |
| ------------ | --------- |
| 0            | 15        |

---
**SOLUTION**:

```sql
SELECT 
  BIT_AND(permissions) AS common_perms,
  BIT_OR(permissions) AS any_perms
FROM user_permissions
;
```
