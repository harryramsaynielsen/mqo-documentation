# 1. MQO-540

<!-- TOC -->

- [1. MQO-540](#1-mqo-540)
    - [1.1. Notes](#11-notes)
        - [1.1.1. Questions](#111-questions)
    - [1.2. Where should you begin?](#12-where-should-you-begin)
    - [1.3. Process](#13-process)
    - [1.4. Required Fields:](#14-required-fields)
        - [1.4.1. Schedule query for MQO-540](#141-schedule-query-for-mqo-540)
        - [1.4.2. Replace True/False with "X" or None](#142-replace-truefalse-with-x-or-none)
    - [1.5. SQL Aggregation](#15-sql-aggregation)

<!-- /TOC -->

## 1.1. Notes

- If they're asking based on _channels_, it's a **schedule** report.
    - The table here is `interface_db.schedule_record_t` (which you can alias with `sr`)
        - `sr_database_key3` refers to TMS IDs.

If you ever want to look up a channel, you can find the progserv ID here:

```sql
select * from sources.progserv p
join sources.source s
```

> (Note: Vijay is using 'source ID' but is referring to a progserv ID.)

```sql
SELECT sr_station_num as progserv_id, sr_air_datetime, sr_database_key3 as tms_id
FROM interface_db.schedule_record_t sr
WHERE sr_station_num = 49613
	AND sr_air_datetime >= '2021-02-22'
	AND sr_air_datetime < '2021-03-08'
	AND sr_deleted = 0
```

### 1.1.1. Questions

- When you say ON Database do you mean the Ned Database?
    - [VJ] Yes NED database, The product feed. The query has come from the CX team we would be interested to look into the data that is shared with clients.
- In the linked google sheet there is a column called `country`. Is that country of production?
    - [VJ] Yes that is correct
- Do you need both program type and subtype? In Ned, program type values are "MV", "SH", "EP", etc. whereas program subtype will be things like Feature Film, TV Movie, Series, Miniseries, etc.
    - [VJ] Yes we need data for both program type and subtype
- I see in the google sheets the data 2/22 - 3/7. Are those the desired start and end dates for the report?
    - [VJ] Yes that is correct
- Are there any image types that need to be filtered out of the report?
    - [VJ] We do not need any filter for Image types

## 1.2. Where should you begin?

If we're going off TMS IDs, begin at `version mapping` - unless they want root IDs, where you want the program table or the version table, depending on the other data.

If you need to go from program to version mapping, you probably need to join on the `version_id` column. If you need to get info about program type, you're going to need the `program` table. If you don't need version-specific stuff, go to programs, because the version one will have multiple versions for the same program ID.

Will need to use the `','join` (etc) for TMS IDs.

------

## 1.3. Process

- Look up by ID instead of name (less room for error)
- However, you should output the **name**, not the ID (For the person who's receiving the report, a description like `Movie` is much clearer than `7`)

----------------

## 1.4. Required Fields:

- [ ] Country
- [ ] progserv id
- [ ] Channel name
- [ ] Schedule start day
- [ ] Schedule end day
- [ ] Schedule time
- [ ] tmsId
- [ ] progType
- [ ] subType
- [ ] title any (Y/N)
- [ ] Image Any (Y/N)

CTE = common table expression, like using WITH in SQL.

Version mapping > version (on version_id) > country of production (on program_id)
Where remote program id = (insert list of TMS here)

This has a `country_id` column, but we need to join to common.country (use `country_abbr1`)

The common database is the link between:
- countries
- languages
- employees, etc.


-------------

### 1.4.1. Schedule query for MQO-540

```sql
SELECT sr_station_num as progserv_id, sr_air_datetime, sr_database_key3 as tms_id
FROM interface_db.schedule_record_t sr
WHERE sr_station_num = 49613
	AND sr_air_datetime >= '2021-02-22'
	AND sr_air_datetime < '2021-03-08'
	AND sr_deleted = 0
```


### 1.4.2. Replace True/False with "X" or None
```Python
import numpy as np
progs_df[['any_banner','any_episodic','any_iconic']] = progs_df[['any_banner','any_episodic','any_iconic']].replace({True: 'X', False: np.nan})
```

--------------  


## 1.5. SQL Aggregation
```SQL
select remote_program_id as tms_id,
    string_agg(country_abbr1, '; ' order by country_of_production_seq) as country

```
- get the schedule IDs
- turn this into a comma-separated string of TMS IDs
- use this:

```SQL
tms_string = "'SH017357350000','EP019038410038','EP019038410039','EP019038410040','EP019038410041','SH019038410000','EP012823810333','EP013065590337','EP012840010316','EP019676450038','EP024457980035','EP016119190110','EP013065590340','EP028466320025','EP015942140118','EP015611490235','EP017357350176','EP020848860021','EP016119190170','EP016571710087','EP028164570021','EP013065590346','EP013065590311','EP019038410018','EP012840010315','EP013065590344','EP017357350217','EP012823810373','EP013065590278','EP012840010323','EP019676450039','EP024457980036','EP016119190122','EP013065590343','EP028466320026','EP015942140116','EP015611490236','EP017357350177','EP020848860022','EP013065590341','EP016119190167','EP016571710088','SH015611490000','SH028466320000','EP015611490337','EP012840010324','EP013065590342','EP012823810374','EP013065590279','EP012840010325','EP019676450040','EP024457980038','EP016119190124','EP028466320027','EP015942140119','EP015611490237','EP017357350178','EP020848860023','EP016119190164','EP016571710089','EP028164570023','EP032668130013','EP019676450066'"
query = f"""
SELECT 	vm.remote_program_id as tms_id,
		program_type_name as prog_type,
		program_subtype_name as prog_subtype,
		tsh.title_text as title,
		tep.title_text as ep_title,
		country
FROM programs.version_mapping vm
	JOIN programs.titleset tsh
		ON vm.titleset_id = tsh.titleset_id
	LEFT JOIN programs.titleset tep
		ON vm.child_titleset_id = tep.titleset_id
	JOIN programs.version v
		ON vm.version_id = v.version_id
	JOIN programs.program p
		ON v.program_id = p.program_id
	JOIN programs.program_subtype ps
		ON p.program_subtype_id = ps.program_subtype_id
	JOIN programs.program_type_name ptn
		ON ps.program_type_id = ptn.program_type_id
	JOIN programs.program_subtype_name psn
		ON ps.program_subtype_id = psn.program_subtype_id
	LEFT JOIN ( 	SELECT 	remote_program_id,
							string_agg(country_abbr1, '; ' order by country_of_production_seq) as country
					FROM programs.version_mapping vm
						JOIN programs.version v
							ON vm.version_id = v.version_id
						JOIN programs.country_of_production cop
							ON v.program_id = cop.program_id
						JOIN common.country c
							ON cop.country_id = c.country_id
					WHERE remote_program_id IN ({tms_string})
					GROUP BY remote_program_id
				) cop
		ON vm.remote_program_id = cop.remote_program_id
WHERE vm.remote_program_id IN ({tms_string})"""
```

Then for **images**:

to get a dataframe of just tms_ids (to pass to the `get_prog_images` function):

```Python
tms_df = pd.DataFrame(progs_df['tms_id'])

img_type_ids = [72,73,74,75,19,76,21,22,77,78,31,33,79,80,48,49,50,52,53,54,55,56,57,58,59,60,61,62,66,67,68,81,82,83]

ned_db.get_prog_images(tms_df, img_type_ids, con, cur)
```
