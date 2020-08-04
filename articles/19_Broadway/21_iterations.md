# Iterations
Iterations can be used to to repeatedly perform a section of a flow in a  data set. Iterations are similar to a **for...each loop** in the sense that a logic is repeatedly run until no data remains to act upon.
The most common iteration use cases are iterating over a database result set or an array of data returned by an API.


## Iterable Line Type

To start the iterable logic, select the line at the beginning of the loop and change the [Link Type](20_broadway_flow_linking_actors.md#link-object-properties) to Iterable. The line is now double-dotted, the background of the loop's scope is highlighted grey and a thick divider line is displayed at the starting point. 

In the following image, Stage 2 runs for each data entry returned by the the first Actor.
 
<div align="center"><img src="images/iterate_simple.png" height="130px"/></div>


## Scope of Iteration

The scope of the interation's logic starts immediately after the Actor and the Iterate line type's origin and continues until the end of the flow or until the **Iterate Close** Stage. To mark an iteration Stage as Closed, click ![image](images/99_19_dots.PNG) to open the [Stage context menu](18_broadway_flow_window.md#stage-context-menu) >  **Iterate Close**. 

The following image displays an iteration loop that starts in Stage 2 and runs until Stage 3 on each entry. After the data is traversed, the loop is complete and Stage 4 is executed.

<div align="center"><img src="images/iterate_scope.png" height="160px"/></div>


## Complex Objects and Paths

Path connections work well when combined with an iteration loop. The following displays an iterate connection selecting a field in a database result set and iterating over it.

<div align="center"><img src="images/iterate_path.png" height="160px"/></div>



## Nested Arrays

Looping an item in an inner array of a data set with nested arrays generates an interation over the entire data set. For example, in a data set of map names holding an array of maps with a Broadway field name, Broadway traverses all **name** values.

<div align="center"><img src="images/iterate_nested_array.png" height="160px"/></div>


## Nested Iterations

Iterations can also be nested. For example, a value in an iteration can be used as an input in another iteration. The depth of the iteration is highlighted in shades of grey. To limit the loop's scope using **Iterate Close**, add a closing Stage on each level of the loop.
There are no limitations on the iteration nesting level. However, to make a flow more readable, consider using inner flows for 3 or 4 nesting levels.


In the following image, the first name is an input to a query that gets a list of relevant phone numbers. Stage 2 is run on every entry in Stage 1 and Stage 3 on every entry in Stage 2.

<div align="center"><img src="images/iterate_nested_iterations.png" height="160px"/></div>



## Split Iterations

A Stage can have more than one collection. A common case is a JSON data structure that contains more than one array.
Using the Stage Split functionality you can split the flow and manage several loops over the same data structure.

<div align="center"><img src="images/iterate_split.png" height="180px"/></div>

## ForLoop Actor

The ForLoop Actor can be used to create a virtual data set of integers in a given range. This enables creating a loop that runs N times over a synthetic data set which is a useful way to repeat iterations when there is no data set to traverse.


## Programmatic Control

The Broadway Context object enables an Actor to programmatically access and control the loop it resides in via the Loop interface. 
A Loop Context object can be accessed via the **Context.loop()** method in Java or using the **contextLoop** instance in JavaScript. 

The following methods are supported:
* **Loop.stop()**, stops the current loop and continues execution after the loop. All Actors in the same Stage as the calling Actor are called.

* **Loop.skip()**, skips the current loop iteration and continues to the next data entry. All Actors in the current Stage are called.

* **Loop.index()**, returns the current loop index. The index of the first iteration = 0.

In a nested loop, you can only access the inner-most (deepest) loop that is running in the current Stage. 

For more information about programmatic control and the JavaScript Actor, refer to the iterate-for-each.flow Broadway example.

[![Previous](/articles/images/Previous.png)](20_broadway_flow_linking_actors.md)[<img align="right" width="60" height="54" src="/articles/images/Next.png">]()
