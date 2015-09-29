---
layout: post
title: What’s in a Title?
created: 1335018668
---
<p>Whenever I’m asked the question: “What do you do for a living?” – I hesitate. It’s not that it’s a complicated question, or I don’t know the answer. I know exactly what I do for a living. However, only a limited subset of the population of people I will meet actually will understand the term “Software Developer”, and even fewer will understand “Systems Engineer”, which is <a href="http://contacts.leafsoftwaresolutions.com/jamesalexander" target="_blank">my actual title</a>.</p>  <p>The best title I’ve come up with so far is “Computer Programmer”. This at least has a word everyone is familiar with, and another word that vaguely means that I know something about the previous word. On rare occasions I’ll simply say “I work with computers”, or “I sit at a computer all day long”.</p>  <p>The problem with all of these titles, and indeed my job itself, is that it’s terribly vague and dependent upon the problem I’m solving that day. If I were to describe in general terms what my job is, I’d have to say that I organize information into an easy and/or conveniently consumable form.</p>  <p>All of this vagueness just doesn’t help most people, and the first thing I always have to do is provide an example. To help me, I’ll just use an example from the <a href="https://developers.google.com/chart/interactive/docs/examples" target="_blank">Google chart examples</a>. </p>    

<?php
drupal_add_js('http://www.google.com/jsapi', 'external');
?>
<script type="text/javascript">
function drawSort() {
  var data = new google.visualization.DataTable();
  data.addColumn('string', 'Name');
  data.addColumn('number', 'Salary');
  data.addColumn('boolean', 'Full Time');
  data.addRows(5);
  data.setCell(0, 0, 'John');
  data.setCell(0, 1, 10000);
  data.setCell(0, 2, true);
  data.setCell(1, 0, 'Mary');
  data.setCell(1, 1, 25000);
  data.setCell(1, 2, true);
  data.setCell(2, 0, 'Steve');
  data.setCell(2, 1, 8000);
  data.setCell(2, 2, false);
  data.setCell(3, 0, 'Ellen');
  data.setCell(3, 1, 20000);
  data.setCell(3, 2, true);
  data.setCell(4, 0, 'Mike');
  data.setCell(4, 1, 12000);
  data.setCell(4, 2, false);

  var formatter = new google.visualization.NumberFormat({prefix: '$'});
  formatter.format(data, 1); // Apply formatter to second column

  var view = new google.visualization.DataView(data);
  view.setColumns([0, 1]);

  var table = new google.visualization.Table(document.getElementById('table_sort_div'));
  table.draw(view);

  var chart = new google.visualization.BarChart(document.getElementById('chart_sort_div'));
  chart.draw(view);

  google.visualization.events.addListener(table, 'sort',
      function(event) {
        data.sort([{column: event.column, desc: !event.ascending}]);
        chart.draw(view);
      });
}
</script>




<p>In the table above is a list of five salaries, and a bar chart beside it. Now, depending on what one is doing with this list of salaries depends on how this information is to be presented. If one wants to simply view all salaries, then the table on the left would likely be preferred. This table format also lends itself to being sorted alphabetically, or by salary so that whomever is viewing this table can analyze the data and make decisions on it. If however, one is presenting this data in a presentation, and wants to visual represent it, then the bar chart clearly is a better medium for the data.</p>  <p>The decisions for how the data is presented is a collaboration between me and the person for whom I’m presenting the data. In this case, it’s likely a manager or HR person that is viewing the salary distributions. So, the HR person would come to be with a problem like: “How do I find out who makes the least money?” At which point, I would provide an interface to these salaries in a table, and the HR person could sort by salary to find that information. Conversely, a manager may need to present this data to higher level managers to show the salary distribution within his department, in which case the bar chart would be a better representation.</p>  <p>As I mentioned before, the decision around which form the data is presented is a collaboration between the client and me. After we know whether we need a table or a chart, I can decide how best to render that table or chart, using the tools I have available to me. For the example in this blog, I used a web page. I could have easily created a windows client, or a plugin for power point or excel to do the same job. It all depends on what tools the client has already, and what other costs the client is willing to spend in getting the data. (including time and money in buying other products)</p>  <p>So, while Systems Engineer and Software Developer are fancier sounding titles, Information Organizer (or Information Engineer if you must) are slightly more accurate descriptions of what I actually do. Of course, neither of these will actually work when I’m introducing myself at a party anyway, so maybe I’ll just stick with Computer Programmer, so that at least the other person knows I work on a computer. </p>
