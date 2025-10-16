## 🧬 Problem: DNA Pattern Recognition

**Table Schema**

```sql
Table: Samples
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| sample_id    | int     |
| dna_sequence | varchar |
+--------------+---------+
sample_id is the primary key.
```


### **Problem Statement**

You are given a table `Samples`, where each row represents a DNA sample and its DNA sequence.
Write an SQL query to determine whether each DNA sequence:

1. **Starts with** a start codon `"ATG"`.
2. **Ends with** a stop codon `"TAA"`, `"TAG"`, or `"TGA"`.
3. **Contains** the specific pattern `"ATAT"` anywhere inside the sequence.

Return the result table with columns:

| sample_id | dna_sequence | has_start | has_stop | has_atat |

Where:

* `has_start` = 1 if the DNA sequence **starts with "ATG"**, else 0
* `has_stop` = 1 if the DNA sequence **ends with one of "TAA", "TAG", "TGA"**, else 0
* `has_atat` = 1 if the DNA sequence **contains "ATAT"**, else 0

Return the results **ordered by sample_id**.

---

### **Example Input**

| sample_id | dna_sequence |
| --------- | ------------ |
| 1         | ATGCGTAATAG  |
| 2         | ATGCCCTAA    |
| 3         | CCCATATTT    |
| 4         | TAAATG       |

### **Example Output**

| sample_id | dna_sequence | has_start | has_stop | has_atat |
| --------- | ------------ | --------- | -------- | -------- |
| 1         | ATGCGTAATAG  | 1         | 1        | 0        |
| 2         | ATGCCCTAA    | 1         | 1        | 0        |
| 3         | CCCATATTT    | 0         | 0        | 1        |
| 4         | TAAATG       | 0         | 0        | 0        |

---

**SOLUTION**:
```sql
SELECT *,
    CASE 
        WHEN dna_sequence REGEXP '(^)(ATG)' THEN 1
        ELSE 0
    END AS has_start,
    CASE
        WHEN dna_sequence REGEXP '(TAA|TAG|TGA)($)' THEN 1
        ELSE 0
    END AS has_stop,
    CASE
        WHEN dna_sequence REGEXP '(ATAT)' THEN 1
        ELSE 0
    END AS has_atat,
    CASE
        WHEN dna_sequence REGEXP '(GGG)' THEN 1
        ELSE 0
    END AS has_ggg
FROM Samples
ORDER BY sample_id ASC
;
```





Would you like me to show **the correct MySQL version** (with `REGEXP`) or the **PostgreSQL version** (with `SIMILAR TO`)?
