# Iterations and Conditions in a Flow

### Using Iterations

A Broadway flow is built in Stages which are executed from left to right. Each Stage can contain one or more Actors that implement logic and data transformation. 

You may occasionally need to iterate the output of an Actor and to handle each iteration. For example, the [Simple Broadway Flow](05_create_broadway_flow.md#example---building-a-simple-broadway-flow) built in the previous lesson selects a list of customers from the database and then builds a JSON object for each customer and writes it into an output file.

Iterations can be added to the flow either by:

- Adding iterations on Stages: setting the [Link Type](/articles/19_Broadway/07_broadway_flow_linking_actors.md#link-object-properties)  to **Iterate** when linking an Actor to another Actor opens a loop. The loop runs on the next Stages in the flow and is closed by the first Stage which is marked as **Iterate Close**. To learn more about the Stage context menu, read about the [Broadway Flow Window](/articles/19_Broadway/18_broadway_flow_window.md).

- Adding a [ForLoop Actor](/articles/19_Broadway/21_iterations.md#forloop-actor) to the flow.

- Iterating data using the code in a [JavaScript Actor](/articles/19_Broadway/actors/01_javascript_actor.md).

Note that some Broadway capabilities like specific built-in Actors or linking between Actors are explained in more detail later in this training.

### Adding Conditions

A Broadway flow can be split into different execution paths based on conditions. More than one Stage can be executed in each fork in the path. If a condition is required in the flow, the flow can be split and a **Stage condition** Actor can be added to one or more Stages that have been created as a result of the split. 

  For example:

  - If (A>3) => Do something.
  - If (B<1) => Do something.
  - Else => Do something else.

To learn about splitting or merging the Stages of a flow and adding conditions to a Stage, read [Broadway Flow - Stages](/articles/19_Broadway/19_broadway_flow_stages.md). 

Continue to the exercise to enhance your first Broadway flow and add a condition to it. 

[![Previous](/articles/images/Previous.png)](05a_create_broadway_flow_example.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">](07_broadway_flow_add_condition_exercise.md)