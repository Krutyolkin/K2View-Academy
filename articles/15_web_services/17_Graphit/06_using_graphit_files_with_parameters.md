# Graphit Parameters

Graphit allows users to define input parameters so any generated document can be executed with different parameters such as LUIs, fields within LU tables or any other specific parameter. 
Graphit can also takes maps objects as input parameters to enable key-value inputs.
It is also possible to pass a list of arguments as parameters, into which Graphit will be looping through.
You can then invoke your Graphit file from a web-service (java file) in which the parameters will be parsed as a map object in the graphit execution statement 

## Parameters setup when running Graphit from the Graphit file editor
In order for Graphit to generate documents that will be customized according to a specific parameter value.
Parameters are defined using the ${} sign as a generic value to the field requested:

Example: 
Generation of a JSON file that will return the values of the following fields:
- Customer_ID, SSN, first_name, last_name for customer with instance ID = 547
- Notes date and status for case ID = 1394

First lets write the genric select statement to retrieve instance 547:
get Customer.${customer_id}

The corresponding queries for the select statements will be as follow:
select  customer_id,ssn,first_name,last_name From Customer.CUSTOMER where CUSTOMER_ID = ${customer_id}
select * from CASE_NOTE where case_id = ${case_id}

See below the full graphit file:
![](/articles/15_web_services/17_Graphit/images/35_graphit_with_parameters.PNG)

Before running the file, we need to set the parameters. This is usually used for debugging, so you will need to set the Server value to debug in the menu on the left side of the Run command:
![](/articles/15_web_services/17_Graphit/images/36_graphit_with_parameters.png)

Let's now assign a debug value to the input parameters: ${customer_id} and ${case_id}
![](/articles/15_web_services/17_Graphit/images/38_graphit_with_parameters.PNG)

Click Run, and view the result on the right side in the output window:
![](/articles/15_web_services/17_Graphit/images/39_graphit_with_parameters.PNG)


## Parameters setup when calling Graphit as a web service
In order to pass parameters 



