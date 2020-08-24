# How Do I Create a New CDC Job?

## Purpose
Fabric can execute [CDC](/articles/18_cdc_and_search/02_cdc_messages.md) Jobs (Change Data Capture) to notify external systems about any data changes occuring in Fabric DB. 

Jobs can also execute cross-instance searches using Fabric's Search capability - (Elastic Search command syntax should be used).

## Job Type
Set the **Job** type to **cdc_republish_instance_job**

## Example:
``` startjob cdc_republish_instance_job Customer.1000 tables='customer,address'```



[![Previous](/articles/images/Previous.png)](/articles/20_jobs_and_batch_services/05_create_a_new_broadway_job.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](/articles/20_jobs_and_batch_services/07_jobs_commands.md)
