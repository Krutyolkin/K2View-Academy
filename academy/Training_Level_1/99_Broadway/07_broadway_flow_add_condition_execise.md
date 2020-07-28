# ![](/academy/images/Exercise.png) Exercise – Adding a Condition to a Broadway Flow 

You've just created and tested your first Broadway flow that selects data from a DB table and creates a JSON file based on the selected DB records. You've also practiced adding Stages and Actors to a Broadway flow and then adding a loop into the flow. 

Now, let's practice an additional Broadway flow feature - adding conditions to a flow. 

A Broadway flow can be split into different execution paths based on conditions so that more than one Stage can be executed in each fork in the path.

In this exercise you will add the following condition to your flow that does the following:

- Check if the number of selected customers >= 3:  

  - If this condition is fulfilled, then add a message to the log file.

  - Else, add an error message to the log file.

    

Before you start, please read [Broadway Flow - Stages](/articles/99_Broadway/19_broadway_flow_stages.md) to learn about spliting and merging the Stages in a flow and about adding conditions to a Stage. 

#### Step 1 - Open the Broadway Flow

1. Go to **Project Tree** > **Shared Objects** > **Broadway** and click the flow you created in the [previous lesson](/academy/Training_Level_1/99_Broadway/05_create_broadway_flow.md).


#### Step 2 - Add Counting of Selected Customers

1. Add the **Count** Actor to **Stage2**. This Actor returns the number of times an Actor is called. If you add this Actor in the loop on selected customers, it counts the number of the selected customers.

#### Step 3 - Add a Condition to the Flow

1. Add a new Stage to the flow, click ![three dots](/academy/Training_Level_1/99_Broadway/images/three_dots_icon.png) and select **Stage Condition**. A new popup window opens where you can add an Actor to the Stage. Select the **GreaterThanEquals** Actor and click  **SUBMIT**.

2. The **GreaterThanEquals** Actor returns true if the value of the **a** parameter is greater than or equals the value of the **b** parameter. This Actor is marked in grey since it is added in a condition.

   Read [Broadway Actors](/articles/99_Broadway/03_broadway_actor.md) to learn more about Broadway Actors.

3. Link the output of the **Count** condition to the **GreaterThanEquals** **a** input parameter to set the **b** parameter to be a **Const** and set its value to 3:

   ![image](/academy/Training_Level_1/99_Broadway/images/MyFirstFlow_GreaterThanEqual_Actor.png)

   

4. Split **Stage4**:

  Click ![three dots](/academy/Training_Level_1/99_Broadway/images/three_dots_icon.png) on the newly created **Stage5** and select the **Else** option.

#### Step 4 - Add the Next Stages

1. Add a Stage to the flow and split it to **Stage6** and **Stage7**.

2. Add the **Logger** Actors to the Stage6 and Stage7.

3. Set the **Logger** Actor parameters of **Stage6**:

   - Set the **message** input parameter as **Const**.

   - Set the value of **message** input parameter to:

     - There are ${0} customers in the list 

     The **${0}** is set for the first parameter of the **params** input parameter.

   - Set the **level** input parameter to **info**.

4. Link the **Count** Actor to the **Logger** Actor or to **Stage6**.

5. Set the **Logger** Actor parameters of **Stage7**:

   - Set the **message** input parameter as **Const**.
   - Set the value of the **message** input parameter to:
     - Error- there are not enough customers in the list
   - Set the **level** input parameter to **error**.

#### Step 5 - Debug the Updated Flow

1. Click ![Debug Step](/academy/Training_Level_1/99_Broadway/images/debug_step_icon.png) to execute the flow steps in Debug mode:

   <ul>
   <pre><code>
   1. What is the output value of the <strong>Count</strong> Actor? 
   2. What is the output value of <strong>GreaterThanEquals</strong> Actor? 
   3. Which Stage has been executed - <strong>Stage6</strong> or <strong>Stage7</strong>? Why?
   4. Check the Output of the <a href="/articles/13_LUDB_viewer_and_studio_debug_capabilities/02_fabric_studio_log_files.md">Fabric Log File</a>. Which message is given by the <strong>Logger</strong> Actor? What is the level of the message? 
   </code></pre>
   </ul>

2. Update the **GreaterThanEquals** Actor: set the **b** parameter to 10 instead of 3.

3. Rerun the Debug on the flow: 

   <ul>
   <pre><code>
   1. What is the output value of <strong>GreaterThanEquals</strong> Actor? Is it different now? Why? 
   2. Which Stage has been executed - <strong>Stage6</strong> or <strong>Stage7</strong>? Why?
   3. Check the output of the <a href="/articles/13_LUDB_viewer_and_studio_debug_capabilities/02_fabric_studio_log_files.md">Fabric Log File</a>. Which message is given by the <strong>Logger</strong> Actor? What is the level of the message? Is it different now? Why? 
   </code></pre>
   </ul>



### ![](/academy/images/Solution.png)Solution - Add a Condition to a Broadway Flow Exercise 

 <ul>
 <pre><code> 
 1. The output value of the <strong>Count</strong> Actor is 3. This is the number of the records returned by the <strong></strong>DbCommand</strong> Actor.
 2. The output value of <strong>GreaterThanEquals</strong> Actor is <strong>true</strong>.
 3. <strong>Stage6</strong> was executed since the condition returned <strong>true</strong>.
 4. The following messsage was given by the Logger Actor of Stage6: 
 <strong>INFO: The are 3 customers in the list</strong>
 </code></pre>
 </ul>

<ul>
<pre><code>
 3.1 The output value of <strong>GreaterThanEquals</strong> Actor is <strong>false</strong>.
 3.2 <strong>Stage7</strong> was executed, since the condition returned <strong>false</strong>.
 3.3 The following messsage was given by the Logger Actor of Stage7: 
     <strong>ERROR: Error- there are not enough customers in the list</strong>. 
     The Logger Actors of Stage6 and Stage7 set a different level of message, since each one of them has a different value of the the level input parameter.
 </code></pre>
 </ul>



[![Previous](/articles/images/Previous.png)](/academy/Training_Level_1/99_Broadway/06_broadway_flow_adding_loops_and_conditions.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">]()
