# How Do I Create a New Broadway Job?

## Purpose
The Fabric Jobs mechanism also enables running a [Broadway flow](/articles/19_Broadway/01_broadway_overview.md).

## Job Type
Set the **Job** type to **broadway_job** and the name of the flow with a list of its arguments.

## Example: 
Using the [startjob](/articles/20_jobs_and_batch_services/07_jobs_commands.md#startjob-jobtype-namename-uiduid-affinityaffinity-argsargs-exec_intervalexecinterval) command:

```
startjob broadway_job name='<lu>.<flow>' [args='{"key":"value"}'];
```

where args consists of a json-type format string containing the parameters to be parsed to broadway: 

``` args='{"first_param":"first_value","second_param":"second_value"}'```
e.g. 
``` startjob broadway_job name='Customer.Flow1' args='A=10, B=20';```




[![Previous](/articles/images/Previous.png)](/articles/20_jobs_and_batch_services/04_create_a_new_process_job.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](/articles/20_jobs_and_batch_services/06_create_a_new_CDC_job.md)
