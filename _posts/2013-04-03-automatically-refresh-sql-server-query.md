---
layout: post
title: Automatically Refresh SQL Server Query
created: 1365019842
---
As I found on <a href="http://stackoverflow.com/questions/5434192/automatically-refresh-a-query-in-ms-sql-server-management-studio" target="_blank">stack over flow,</a> there is a clever way to automatically refresh your current query N times. This is ideal if you're checking a running query, or trying to get a feel for how long a specific query is going to take, and you want to monitor it along the way.

<pre class="brush: sql">

SELECT GETDATE()              --your query to run
raiserror('',0,1) with nowait --to flush the buffer
waitfor delay '00:00:10'      --pause for 10 seconds
GO 5                          --loop 5 times

</pre>
