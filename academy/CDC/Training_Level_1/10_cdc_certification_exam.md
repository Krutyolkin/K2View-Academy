# Fabric CDC Certification Exam

<img src="/academy/images/Quiz.png" style="zoom:80%;" /> 

Excellent! You have completed the CDC Training and the Exercises. It's now time to take the following certification exam to see what you have learned. 

The exam consists of a number of multiple-choice questions, each providing a number of possible answers. Click the answer you think is correct. 



#### Question 1: CDC Overview

Fabric CDC capabilities enable you to:


\- A:  Publish Fabric changes to the outside world.


\- B:  Get data changes from the source systems and sync Fabric data accordingly.

\- C:  Run cross instance search.

\- D:  A and C.

(**Solution 1. D: Fabric CDC capabilities enables publishing changes on predefined LU fields to external CDC consumers. One of the CDC consumers is Elasticsearch which enables cross instance search**).



#### Question 2: CDC Messages

CDC messages:


\- A:  Update the CDC Consumers in a synchrony mode.


\- B:  Are sent to Kafka and update the CDC consumers in an asynchrony mode.

\- C:  Are sent to only when a new LUI created in Fabric.

\- D:  None of the above.


(**Solution 2. B:  the CDC Messages are sent to Kafka. Each CDC consumer has its own topic. The CDC consumers can consume the CDC messages from Kafka.**).



#### Question 3: CDC Messages

Which events triggers a publication of CDC Schema message?  

\- A:  Running CDC_REPUBLISH_SCHEMA Fabric command on a selected LU.


\- B:  First deploy of an LU with CDC fields.

\- C:  First get of LUI into Fabric.

\- D:   A and B.


(**Solution 3. D.**).



#### Question 4: CDC Consumers

Which CDC Consumers can get CDC messages from Fabric? 


\- A:  Only Elasticsearch can get CDC messages from Fabric.


\- B:  Only Broadway can get CDC messages from Fabric.

\- C:  Each CDC consumer, defined in the k2Proj file of the Fabric project, and has CDC fields, can get CDC messages.

\- D:  None of the above.


(**Solution 4. C**).



#### Question 5: CDC Implementation Changes

What happens when new CDC fields are added to the LU and the updated LU is redeployed to Fabric? 

\- A:  Fabric sends a CDC Update Schema message to the CDC consumers, but you must re-migrate all LUIs to send the data of the new CDC fields to the CDC consumers.

\- B:  Nothing happens. You must drop and redeploy the LU to send the updated CDC fields to the CDC consumers.

\- C:  Fabric sends a CDC Update Schema message to the CDC consumers and initiates a CDC_REPUBLISH_INSTANCE command on each Fabric LUI to send the CDC data of the new CDC fields to the CDC consumers.

\- D:  None of the above.

(**Solution 4. A**).



#### Question 6: CDC Implementation Changes

You add a new LU table with CDC fields to the LU. How can you update the data of the new LU table into the CDC consumers?


\- A:  The deploy of the updated LU sends an update message to the CDC consumers. No additional activity is needed.


\- B:  You must drop and redeploy the LU.

\- C:  You need to deploy the updated LU and then re-migrate all LUIs.

\- D:  You need to run a batch process to run CDC_REPUBLISH_INSTANCE on all the LUIs.


(**Solution 6. C: The deploy of the LU activates the CDC_REPUBLISH_INSTANCE on all LUIs, but you need to re-migrate all LUIs to bring the data of the new LU table into Fabric. The migrate updates the MicroDBs of the LUIs and send CDC Table Change Info message to the CDC consumers**).





[![img](/articles/images/Previous.png)](22a_broadway_summary_exercise_solution.md)