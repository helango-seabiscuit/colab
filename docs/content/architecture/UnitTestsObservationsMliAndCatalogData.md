### Recommender Service - Unit tests Observations for content version changes

# Data Involved
- MLI
- Catalog

# Tables Referred
- MLI - app-cnt-svcs-rs-mli-datasets-pr7
- Catalog - app-cnt-svcs-rs-catalog-ingestor-pr7

# MLI

Different formats of SK used for testing
- 2023-11-26T18:21:38.050Z - old format
- contentVersion#240120233217#createdTimestamp#2024-01-23T04:30:09.881Z - new format

Unit Testing Findings for MLI Data
- The sorting is happening first based on the number highlighted below, not the date/time at the end:
  - 2024-01-26T12:00:38.050Z
  - contentVersion#**240120233217**#createdTimestamp#2024-01-23T04:30:09.881Z
  - contentVersion#**240123162439**#createdTimestamp#2024-01-03T17:00:08.708Z


    Due to this, irrespective of what the date is in ‘a’, the comparison is always happening only between ‘b’ and ‘c’.
    Though the date in ‘b’ is latest between ‘b’ and ‘c’, still ‘c’ is returned as the highlighted value for ‘c’ is higher than ‘b’

- If the above highlighted value is same for various SK, only then the sorting is happening based on the date and time:
  - 2024-01-26T12:00:38.050Z
  - contentVersion#240123162439#createdTimestamp#2024-01-23T04:30:09.881Z
  - contentVersion#240123162439#createdTimestamp#2024-01-03T17:00:08.708Z


    In this case, ‘b’ is returned based on date




# Catalog

Different formats of SK used for testing
- content-version#null#timestamp#2024-01-11T08:59:07Z - old format
- content-version#nulltimestamp#2024-01-16T18:22:07Z - new format

Unit Testing Findings for Catalog Data
- For SK values with null in them, the sorting is comparing only those SK with no # after the null:
  - content-version#nulltimestamp#2024-01-11T08:59:07Z
  - content-version#null#timestamp#2024-01-16T18:22:07Z
  - content-version#nulltimestamp#2024-01-06T21:36:07Z


    In the above case, ‘b’ is left out and comparison is happening between ‘a’ and ‘c’ and ‘a’ is returned

- If all the SK are of same format and contain null#, then sorting takes place based on date and time:
  - content-version#null#timestamp#2024-01-11T08:59:07Z
  - content-version#null#timestamp#2024-01-16T18:22:07Z
  - content-version#null#timestamp#2024-01-06T21:36:07Z


    In the above case, ‘b’ is returned

- Similarly, if all the SK are of same format and contain null with no #, then also sorting takes place based on date and time:
  - content-version#nulltimestamp#2024-01-11T08:59:07Z
  - content-version#nulltimestamp#2024-01-16T18:22:07Z
  - content-version#nulltimestamp#2024-01-21T21:36:07Z


    In the above case, ‘c’ is returned
