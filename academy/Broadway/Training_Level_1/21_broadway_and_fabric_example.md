# Broadway and Fabric - Examples

### ![info](/academy/images/example.png)Example 1 - Running a Flow using a Broadway Command

In this example you can either use the **CheckNegative** Broadway flow you created in [Example 2 - Inner Flow and Error Handling](16_broadway_addl_features_ex2.md) or create a new flow.

1. Deploy an LU that includes the flow definitions. 
   * If the flow is created under Shared Objects, deploy any LU, for example CRM LU.
2. Open the Fabric Console and run the **Broadway** command twice using different input.

~~~
fabric>broadway CRM.CheckNegative a = "-10";
Flow: CheckNegative Level: 1 Stage: Stage 1 Actor: ErrorMsg  class jdk.nashorn.internal.runtime.ECMAException Cause: A Can't be negative!

fabric>broadway CRM.CheckNegative a = 10;
|column|value                  |
+------+-----------------------+
|date  |2020-09-22 12:08:12.620|
~~~

3. Check that the flow's results are as expected.



[![Previous](/articles/images/Previous.png)](20_broadway_and_fabric.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](22_broadway_summary_exercise.md)