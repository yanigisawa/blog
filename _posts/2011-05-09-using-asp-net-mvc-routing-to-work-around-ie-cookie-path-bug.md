---
layout: post
title: Using ASP.NET MVC Routing to work around IE cookie path bug
created: 1304959487
section-type: post
---
As seen from <a href="http://stackoverflow.com/questions/2156399/restful-cookie-path-fails-in-ie-without-trailing-slash" target="_blank">this stack overflow question</a> all versions of IE appear to have a "bug" that will not persist or retrieve cookies when set on a specific path that does not include a trailing slash. For example, if using the jquery cookie plugin:

<pre class="brush: jscript">
$.cookie("CookieId", "StoredCookieValue", { path: "/MyController/" });
</pre>

However, by default, ASP.NET MVC doesn't add the trailing slash, so if I'm requesting the Index action of my controller, (e.g. http://somedomain.com/MyController) I don't get the trailing slash, and IE won't retrieve the cookies stored at "/MyController/".

To work around this issue, I found <a href="http://stackoverflow.com/questions/1385265/add-a-trailing-slash-at-the-end-of-each-url" target="_blank">another stack over flow question</a> that provides functionality to automatically add trailing slashes to the virtual route paths created by MVC. As seen in the question, one can map routes with the trailing slash for a specific controller:
<pre class="brush: csharp">
routes.MapRouteTrailingSlash("MyControllerRoute", "MyController/{action}/{id}", new { controller = "MyController", action = "Index", id = "" });
</pre>

 or change the default MapRoute to MapRouteTrailingSlash:

<pre class="brush: csharp">
routes.MapRouteTrailingSlash(
  "Default",                                              // Route name
  "{controller}/{action}/{id}",                           // URL with parameters
  new { controller = "MyController", action = "Index", id = "" }  // Parameter defaults
);
</pre>
