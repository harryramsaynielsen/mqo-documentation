# MQO Documentation

# Table of Contents
- [MQO Documentation](#mqo-documentation)
- [Table of Contents](#table-of-contents)
- [Report Types](#report-types)
- [SQL Queries](#sql-queries)
  * [SQL Code Blocks](#sql-code-blocks)
    + [Version mappings](#version-mappings)
  * [Specific Output fields](#specific-output-fields)
    + [Callsign](#callsign)
    + [Original source](#original-source)
    + [Test of Python syntax](#test-of-python-syntax)
  * [Notes](#notes)

# Report Types

> What types of report are we generally dealing with, and what is the best place to start for each one, in terms of the SQL code required?

- Program reports
- Version mapping reports
- Schedule reports
- Image reports
- Scheme reports
- Channel list reports

# SQL Queries

- SQL code blocks
- SQL code snippets

## SQL Code Blocks
Here, we store predefined code blocks for _types_ of reports we'd want to have.

This is just plain text.
### Version mappings

Retrieving version mappings.
Output fields:

- TMS ID
- Program ID
- Title Text
- Language
- Original source
- Country of production
- Sequence number

Changes

```SQL
SELECT vm.remote_program_id as tms_id,
       v.program_id,
       t.title_text,
	   rfc_5646 as language,
       call_sign_text as orig_source,
	   country_name as prod_country,
       country_of_production_seq as seq
FROM   programs.version_mapping vm
       JOIN programs.version v
         ON vm.version_id = v.version_id
       JOIN programs.titleset t
         ON vm.titleset_id = t.titleset_id
	   JOIN common.language l
	   	 ON t.language_id = l.language_id
       LEFT JOIN programs.original_air_date oad
         ON v.program_id = oad.program_id
       LEFT JOIN sources.source s
         ON oad.source_id = s.source_id
       LEFT JOIN sources.progserv ps
         ON s.source_id = ps.source_id
       LEFT JOIN sources.cur_tms_call_sign cs
         ON ps.progserv_id = cs.progserv_id
	   LEFT JOIN programs.country_of_production cop
	   	 ON v.program_id = cop.program_id
	   LEFT JOIN common.country_name cn
	   	 ON cop.country_id = cn.country_id
WHERE  v.program_id IN ( 14765844, 14765844)
AND mapping_target_id = 2
AND t.language_id in (15,16,21,30,112)
```

## Specific Output fields
This part of the documentation is related to finding specific output fields (for instance, callsign or country of production).
### Callsign

-   The field that we use for callsigns is `call_sign_text`, and we get this using:

```sql
LEFT JOIN sources.cur_tms_call_sign cs ON ps.progserv_id = cs.progserv_id
```

This is a **callsign** if it is pulled from the _schedule record_. If it is pulled from the _version mapping_ table, it is an **original source**.

### Original source

-   The only difference compared to Callsign is that the data is pulled from the program record (i.e. version mapping) and _not_ the schedule. It's the same item being pulled from the same table; however, the thing that changes it is the first line of the query.

```sql
LEFT JOIN sources.cur_tms_call_sign cs ON ps.progserv_id = cs.progserv_id
```

### Test of Python syntax

```python
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

<a name="notes"></a>

## Notes

> **If we had to grow our team, what should the documentation look like?**

-   Two parts:
    -   Domain-specific knowledge (i.e. of GN)
        -   What else?
    -   Technical knowledge (how to write code to query specific tables etc)
        -   What else?

Here's a sentence with a footnote. [^1]

[^1]: This is the footnote.
