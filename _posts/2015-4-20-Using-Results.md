---
layout: post
title: Using Results
---

## Results
Within PDI the *results* concept is used to store a set of rows in memory. It allows you to load and transform some
data, then use the *results* to store the transformed data to be obtained by another transformation.

In order to add rows to the results set, you simply use the **Copy rows to results** step:
![Copy rows to results](../images/Pentaho/copy_rows_result.png)

To retrieve the rows in another transformation, you use the **Get rows from results** step:

![Get rows from results](../images/Pentaho/get_rows_results.png)

By leaving all options within this step blank, it will obtain all the rows from results, however you can specify the
specific column(s) you wish to obtain too by name; in the example they are blank like so:
![Get rows options](../images/Pentaho/get_rows_options.png)

## Example

The following is an example of a **Job** containing two **Transformations**.
![Main Job](../images/Pentaho/pass_between_job.png)

The first transformation generates 10 rows of containing the string "Hello World!" and copies those rows to result.
![Generate rows](../images/Pentaho/generate_rows.png)

The second transformation gets the rows from results and outputs them to the log.
![Get rows log](../images/Pentaho/get_rows_log.png)

Here is the final log:
![Log Output](../images/Pentaho/log_output.png)
