---
layout: post
title: Bash Reference Part 1
created: 1317513017
---
<strong>Variables:</strong> 
One can use a variable simply by pre-pending a dollar sign ($) in front of the variable. If the variable is to be concatenated with another fixed string, one can use curly braces around the variable to define the variable itself, and then to concatenate the variable with other text. For example:

<pre class="brush: bash;">
$ str="this is a string"
$ echo $str
this is a string
$ echo ${str}
this is a string
$ echo ${str}andmore
this is a stringandmore
$ 
</pre>

By default, variables are only scoped to their current shell. Meaning, if you execute a shell script, or start a new sub shell, variable declared in parent shells don't by default translate into the sub shell. If you want variables to be set in the sub shell, use the export command:

<pre class="brush: bash;">
$ echo $VAR

$ VAR=5
$ export VAR
$ bash
bash-3.2$ echo $VAR
5
bash-3.2$ 
</pre>

If you want a variable to have a specific value just for the duration of a command's execution, you can assign the variable inline with the script invocation:

<pre class="brush: bash;">
$ ./showit

$ cat showit
echo $VAR

$ VAR=17
$ ./showit

$ VAR=29 ./showit
29
$ echo $VAR
17
$ 

</pre>
