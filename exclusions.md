# Documentation Exclusions: Image Coverage Reports

> For standard Image Coverage reports, it is common to exclude certain programmes by default. These exclusions are made in order to ensure that the report output matches up with the product file, which also excludes these programmes by default.
> 
> Clients may, on a per-report basis, request exceptions to these exclusions.


## Standard Exclusions

-   **Exclude AO Programming (Adults Only Target Audience)**
    - Any programmes with a target audience of "Adults Only" should be excluded by default.
-   **Exclude Paid Programs**
    - Paid programs (for example, infomercials) should be excluded by default.
-   **Exclude Local Programming (Source Type = Local)**
    - Anything considered as "local programming" (any small, regional programs) with a source type of `Local` should be excluded by default.


> NOTE: We use **subtypes**, not types, to differentiate.

| **Program Type Buckets** | **DB Program Subtype**                          | **Genre**                                                                              |
| ------------------------ | ----------------------------------------------- | -------------------------------------------------------------------------------------- |
| Feature Film             | Feature Film                                    |                                                                                        |
| TV Movies/Short Film     | TV Movie OR Short Film                          |                                                                                        |
| Series                   | Series OR Miniseries                            | Exclude Genre of  "News" OR "Talk"                                                     |
|                          | Series OR Miniseries                            | Exclude Genre of "News" OR "Talk Show" OR "Soap Opera" OR "Educational" OR "Game Show" |
|                          | TMSId begins "EP"                               |                                                                                        |
| Special                  | Special                                         |                                                                                        |
| Sport                    | Sport Event OR Sports event OR Sports non-event |                                                                                        |
