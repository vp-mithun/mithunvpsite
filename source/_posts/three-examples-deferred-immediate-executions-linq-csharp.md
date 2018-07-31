---
title: 'THREE examples of Deferred vs. Immediate execution in LINQ using C#'
tags:
  - 'C#'
url: 144.html
id: 144
categories:
  - .NET
date: 2013-11-23 01:35:17
---

In DOT NET world we have **Language-Integrated Query (LINQ)** – it extends powerful query capabilities to the language syntax of C# and Visual Basic. LINQ provider assemblies enable the use of LINQ with .NET Framework collections, SQL Server databases, ADO.NET Datasets, and XML documents. They are part of .NET framework 3.5 and above. Now let’s see three ways to understand what exactly is this deferred and immediate execution all about in LINQ using C#.

DateTime to verify deferred execution of LINQ query
---------------------------------------------------

*   Create  console application with name “_LinqSample_”
*   Create Product class with _ID, Name, InStock_ members in it.
*   Create collection of products, LINQ Query and iterate through it to display time.

[![Creating PRODUCT class and it's sample collection](http://www.mithunvp.com/wp-content/uploads/2013/11/product-class-collection-300x65.png "Creating PRODUCT class and it's sample collection")](http://www.mithunvp.com/wp-content/uploads/2013/11/product-class-collection.png)

*   Now we will be creating two simple LINQ queries which will act as deferred and immediate query

[![Queries to use for deferred and Immediate execution](http://www.mithunvp.com/wp-content/uploads/2013/11/deferred-Immediate-300x171.png)](http://www.mithunvp.com/wp-content/uploads/2013/11/deferred-Immediate.png) We have created list of products, written query for deferred and immediate execution, now let’s loop through them in 3 different foreach loops to check difference in time they are executed. The yellow lines indicate that once the immediate query gets executed, any subsequent looping will fetch same copy. While deferred execution gets executed for every loop showing different time stamp.[![Console output showing time stamp for Deferred & Immediate query execution](http://www.mithunvp.com/wp-content/uploads/2013/11/compare-screenshot-300x177.png)](http://www.mithunvp.com/wp-content/uploads/2013/11/compare-screenshot.png)

Collection Items update to show deferred execution & Immediate execution
------------------------------------------------------------------------

Adding the items to collection in execution and looping them again is also easiest way to check when LINQ query executed. We have to write a query which gets only In Stock products, loop through that query, add one more item to collection and loop through again. Below code snippet image clearly shows this. [![Deferred execution in action](http://www.mithunvp.com/wp-content/uploads/2013/11/codesnippet-deferred-query-300x181.png "codesnippet-deferred-query")](http://www.mithunvp.com/wp-content/uploads/2013/11/codesnippet-deferred-query.png) Code snippet showing additional product gets added to collection after query formed for **Deferred execution**. This code snippet below shows that we are doing same thing except converting result to TOLIST along with query. [![Immediate execution in action](http://www.mithunvp.com/wp-content/uploads/2013/11/codesnippet-immediate-query-300x169.png)](http://www.mithunvp.com/wp-content/uploads/2013/11/codesnippet-immediate-query.png) Code snippet showing additional product gets added to collection after query formed in **Immediate execution**. Now let’s compare both results after running the console application. Milk product added later is shown in deferred execution only. This is shown as yellow lines in screen shot.[![](http://www.mithunvp.com/wp-content/uploads/2013/11/compare-screenshot-collection-300x154.png)](http://www.mithunvp.com/wp-content/uploads/2013/11/compare-screenshot-collection.png).
 **Deferred execution shows that even if collection changes, looping always result in fresh data**.

### Is ToList() only way to force immediate execution?

No, it’s one of many methods that forces immediate execution. Here is the extract from MSDN on Immediate query execution

> In contrast to the **deferred execution** of queries that produce a sequence of values, queries that return a singleton value are executed immediately. Some examples of singleton queries are _Average_, _Count_, _First_, and _Max_. These execute immediately because the query must produce a sequence to calculate the singleton result. You can also force immediate execution. This is useful when you want to cache the results of a query. To force immediate execution of a query that does not produce a singleton value, you can call the _ToList_ method, the _ToDictionary_ method, or the _ToArray_ method on a query or query variable.

Debugging LINQ query to show deferred & Immediate execution
-----------------------------------------------------------

Let’s use the above queries to debug console application. This is animated GIF to show debugging process.[![Debugging LINQ](http://www.mithunvp.com/wp-content/uploads/2013/11/debug-linq-csharp-300x195.gif)](http://www.mithunvp.com/wp-content/uploads/2013/11/debug-linq-csharp.gif) Animated gif file to show debugging steps of LINQ query. Let me summaries what is happening while debugging At first break point

*   Deferred query is formed; it’s not executed as we can see that it only shows _WhereSelectListIterator_.
*   With help of F11 key, am “Step Into” debugging; at first for each loop, the execution pointer going to where condition in query “_where d.InStock == true_”.
*   It does same looping, checking where condition for all items in product list and keeps them displaying on the console.

At second break point

*   Immediate query is formed with ToLIST(), it immediate shows count as 5 before looping into for each loop.
*   Even when it’s iterating through for each loop, execution pointer never goes to where condition in query. It just prints out selected product on console.

In C#, the deferred execution is supported by directly using the **yield keyword** within the related iterator block. Below Table lists the iterators in C# that use the yield keyword to ensure the deferred execution. 