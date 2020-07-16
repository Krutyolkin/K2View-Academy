# Graphit - Code Examples
### Simple Example of a Customer Info Web Service that Brings Data for a Given LUI

The following Graphit file gets an input LUI which extracts customer data from the CUSTOMER LU, calculates its balance and sets its status accordingly. Output data is returned in JSON structure and adds information on whether the customer is a VIP member (total balance of over USD 10,000), or a Gold member (total balance of over USD 1,000). 

![](/articles/15_web_services/17_Graphit/images/58_graphit_examples.PNG)

After deploying and invoking the Graphit file directly as a Web Service:
![](/articles/15_web_services/17_Graphit/images/59_graphit_examples.PNG)


###  Example of a wsGraphitExample2 Web Service that Invokes the Relevant Graphit File Depending on a Specific Condition    
The following Web Service gets an input LUI for the CUSTOMER LU and returns a response stating whether a customer has a Bronze, Gold or Platinum status in their subscription lines.

Code:

```java
String val_brz="Bronze";
String val_gld="Gold";
String val_plt="Platinum";

String CUST_STATUS = "SELECT count(*) FROM SUBSCRIBER where SUBSCRIBER.VIP_STATUS=?";
String cnt_brz = ludb("Customer", i_id).fetch(CUST_STATUS,val_brz).firstValue().toString();
String cnt_gld = ludb("Customer", i_id).fetch(CUST_STATUS,val_gld).firstValue().toString();
String cnt_plt = ludb("Customer", i_id).fetch(CUST_STATUS,val_plt).firstValue().toString();


if ((Integer.parseInt(cnt_brz)==0)&&((Integer.parseInt(cnt_gld) !=0)||(Integer.parseInt(cnt_plt) !=0))){
	return graphit("grSqlGldPlus.graphit",i_id);}
else{
	return graphit("grSqlBrz.graphit",i_id);}
```

The first Graphit file displays the customer's basic information and their subscriber lines with a Gold or Platinum VIP status while the second Graphit file displays the customer's basic information and corresponding subscriber lines in Bronze VIP status.

Graphit file 1: grSQLGldPlus.graphit
![](/articles/15_web_services/17_Graphit/images/60_graphit_examples.PNG)


Graphit file 2: grSQLBrz.graphit
![](/articles/15_web_services/17_Graphit/images/62_graphit_examples.PNG)


Output from Swagger GUI for grSQLGldPlus.graphit with Customer Instance ID = 1234:
![](/articles/15_web_services/17_Graphit/images/59_graphit_examples.PNG)

Output from Swagger GUI for grSQLBrz.graphit with Customer Instance ID = 1000:
![](/articles/15_web_services/17_Graphit/images/59a_graphit_examples.PNG)

### Simple Example with CSV Generated from JOIN Queries Between Subscriber and Invoice Tables From the Billing_DB External Database
This example displays how to retrieve data from multiple tables in the Billing_DB database and use Graphit to prepare a CSV-formatted response:

Graphit file: grCSV.graphit:
![](/articles/15_web_services/17_Graphit/images/63_graphit_examples.PNG)
![](/articles/15_web_services/17_Graphit/images/64_graphit_examples.PNG)

Running the Graphit file in Debug mode with 2 and 3 as consecutive values for the SubscriberID:
![](/articles/15_web_services/17_Graphit/images/65_graphit_examples.PNG)

Note the different tags used and their effect on the output CSV document:
e.g. csvHeader: false -> removes fields headers from the response







[![Previous](/articles/images/Previous.png)](/articles/15_web_services/17_Graphit/09_invoke_graphit_from_outside_studio.md)