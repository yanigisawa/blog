---
layout: post
title: View MS SQL Server Log Programmatically
created: 1298218161
section-type: post
---
<p>If your user name is a member of the securityadmin SQL server role, you have access to run a magic undocumented stored procedure: xp_readerrorlog. This proc reads the log file from the SQL server and outputs the results in a table within MS Management studio. This allows for scripting access to the file, without needing direct access to the server's file system, or having to open the log tree in SQL Server Management Studio and wait for the UI. 
</p>
<p>
Once we have the output of this proc, we can insert the contents into a table variable, then sort / filter the contents any way we like. We can also take this one step further and put this script into a console C# application and run it on a schedule to search for specific error strings if we like. (in my case, I'm scanning for deadlocks)
</p>
<p>
Below is a SQL script that I run daily to view the log file from within a SQL Server management studio window
</p>

<pre class="brush: sql">
declare @temp table (LogDate DATETIME, ProcessInfo VARCHAR(50), MessageText VARCHAR(MAX))

--exec sp_enumerrorlogs

insert into @Temp
exec xp_readerrorlog
select *
from (
       select top 100 * from @Temp
       order by LogDate desc
) t
order by LogDate
</pre>

<p>
Line 1 of this script simply creates a table variable to hold the log fields stored in the SQL log file.</p>
<p>
Lines 5 and 6 run the proc and output the entire file into the @Temp table variable
</p>
<p>Line 7-12 essentially runs the "tail" (from unix) command on the file. Since the output from the proc is in chronological order, I run the inner query to order the entries in reverse date order to find the 100 most recent entries, then the main query orders the log entries chronologically</p>
<p>If you have a pattern or a specific error you need to search to search on, one can easily add a where clause and filter on any of the fields as desired</p>
