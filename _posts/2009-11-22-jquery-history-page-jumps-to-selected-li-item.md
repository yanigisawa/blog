---
layout: post
title: jQuery History, page “jumps” to selected <li> item
created: 1258898750
---
<p>The situation is this: I’m using jQuery for loading of all sub-pages of my main web page, and I want to maintain the browser history so that a user can simply click the “Back” button to navigate to the previous link. I found <a href="http://www.mikage.to/jquery/jquery_history.html" target="_blank">a jQuery history plug-in</a> that implements all of the history caching with a few lines of JavaScript, and an included .js file. It seems to work really well, but I had one issue: The browser’s scroll position would auto-scroll to the link’s position. It turns out this was developer error, but it took me a couple hours to finally figure out where the problem was, so I decided I should blog about it in case anyone was having the same problem and found this in the Google search results.</p>  <p>In order to make the jQuery history plug-in work,&#160; one needs to format one’s links like so:</p>    <pre>&lt;a href=&quot;#1&quot; rel=&quot;history&quot;&gt;load 1&lt;/a&gt;</pre>



<p>And add the following bit of javascript to the top of the page for processing:</p>

<pre>&lt;script type=&quot;text/javascript&quot;&gt;
        var timeoutValue;
        // PageLoad function
        // This function is called when:
        // 1. after calling $.historyInit();
        // 2. after calling $.historyLoad();
        // 3. after pushing &quot;Go Back&quot; button of a browser
        function pageload(hash) {
            // hash doesn't contain the first # character.
            if (hash) {
                // restore ajax loaded state
                if ($.browser.msie) {
                    // jquery's $.load() function does't work when hash include special characters like aao.
                    hash = encodeURIComponent(hash);
                }
                //timeoutValue = setTimeout(&quot;IsLoading(true);&quot;, 200);
                $(&quot;#pagecontent&quot;).load(hash + &quot;.html&quot;);
            } else {
                // start page
                $(&quot;#pagecontent&quot;).load(&quot;menuItem1.html&quot;);
                hash = &quot;menuItem1&quot;;
            }
            ColorActiveMenu(hash);
        }
        
        function ColorActiveMenu(menuName) {
            var items = $(&quot;#mainMenu &gt; *&quot;);
            items.attr(&quot;class&quot;, &quot;&quot;);
            menuName = &quot;#&quot; + menuName;
            $(menuName).attr(&quot;class&quot;, &quot;current&quot;);
        }

        $(document).ready(function() {
            //alert(&quot;Document ready&quot;);
            // Initialize history plugin.
            // The callback is called at once by present location.hash. 
            $.historyInit(pageload, &quot;menuItem1.html&quot;);

            // set onlick event for buttons
            $(&quot;a[rel='history']&quot;).click(function() {
                // 
                var hash = this.href;
                hash = hash.replace(/^.*#/, '');
                // moves to a new page. 
                // pageload is called at once. 
                // hash don't contain &quot;#&quot;, &quot;?&quot;
                $.historyLoad(hash);
                return false;
            });
            
            if (location.hash == &quot;&quot;) {
                pageload(&quot;menuItem1&quot;);
            }
        });
&lt;/script&gt;</pre>

<p>In the $(document).ready event, the last line we add to the onClick event of each of our links we're adding to the history cache, we use 'return false;' which I thought would counteract any effect of auto-scrolling that would occur due to using the pound (#) mark in the href of the link. It turns out this was incorrect. (at least in the context of IE and Firefox) Consider the following piece of code from a &quot;menu&quot; that I created for a page I developed recently:</p>

<pre>&lt;ul id=&quot;mainMenu&quot; class=&quot;menu&quot;&gt;
        &lt;li id=&quot;menuItem1&quot; class=&quot;current&quot;&gt;&lt;a href=&quot;#menuItem1&quot; rel=&quot;history&quot;&gt;
            Menu Item 1&lt;/a&gt;
        &lt;li id=&quot;menuItem2&quot;&gt;&lt;a href=&quot;#menuItem2&quot; rel=&quot;history&quot;&gt;
            Menu Item 2&lt;/a&gt;
        &lt;li id=&quot;menuItem3&quot;&gt;&lt;a href=&quot;#menuItem3&quot; rel=&quot;history&quot;&gt;
            Menu Item 3&lt;/a&gt;
    &lt;/li&gt;
&lt;/ul&gt;</pre>

<p>Note, I also changed the javascript a little to automatically load the first item. My goal was to have a menu that would make certain CSS changes based upon which page was currently loaded. To do this, I added the ID attribute to each of the &lt;li&gt; tags, in order to identify them when I was ready to mark each item as active. <a href="http://jamesralexander.com/blogExamples/jQueryHistory/example1.html" target="_blank">Here is a live demo</a> of what I was trying to accomplish as well as the bug I encountered / created by using these specific ID names. </p>

<p>The issue seems to be that since I set the ID value to the exact same value as the hash value (e.g. id=”menuItem1” and href=”#menuItem1”) that the browser detected the &lt;li&gt; tag menuItem1 in this case as the position in the page that I wanted to scroll to when this link was clicked. </p>

<p>To work around this behavior, I merely changed the names of the ID attributes I assigned to each &lt;li&gt; tag, like so:</p>

<pre>&lt;ul id=&quot;mainMenu&quot; class=&quot;menu&quot;&gt;
	&lt;li id=&quot;limenuItem1&quot; class=&quot;current&quot;&gt;&lt;a rel=&quot;history&quot; href=&quot;#menuItem1&quot; &gt;
		Menu Item 1&lt;/a&gt;&lt;/li&gt;
	&lt;li id=&quot;limenuItem2&quot;&gt;&lt;a rel=&quot;history&quot; href=&quot;#menuItem2&quot; &gt;
		Menu Item 2&lt;/a&gt;&lt;/li&gt;
	&lt;li id=&quot;limenuItem3&quot;&gt;&lt;a rel=&quot;history&quot; href=&quot;#menuItem3&quot; &gt;
		Menu Item 3&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;</pre>

<p>Note, that I also had to make one minor change to the pageload javascript function to add the “li” prefix to each ID attribute:</p>

<pre>ColorActiveMenu(&quot;li&quot; + hash);</pre>

<p><a href="http://jamesralexander.com/blogExamples/jQueryHistory/example2.html" target="_blank">Here is the full working page.</a>&#160;</p>

<p>Hope this helps.</p>
