---
layout: post
title: Visual Studio - Toggle Between Leading Tabs or Spaces Per Project
created: 1282141079
section-type: post
---
If you need to toggle between using Tabs or spaces in your Visual Studio projects and files, you can use this simple Macro from within VS to toggle between tabs vs. no tabs:

1. Open the Macro Editor via Tools -> Macros -> Macros IDE (Alt+F11)
2. Paste in the following VBA code:

{% highlight vb.net linenos %}
Public Sub ToggleTabs()
    Dim currentSetting As Boolean = DTE.Properties("TextEditor", "CSharp").Item("InsertTabs").Value
    DTE.Properties("TextEditor", "CSharp").Item("InsertTabs").Value = Not currentSetting    
End Sub
{% endhighlight %}

3. Open the Macro Editor via Tools -> Macros -> Macro Explorer (Alt+F8)
4. Run the "ToggleTabs" macro

For more information on other settings that can be utilized via macros, see the <a href="http://msdn.microsoft.com/en-us/library/ms165641%28vs.71%29.aspx" target="_blank">MSDN page</a>

Or, see how one can use <a href="http://mkdot.net/blogs/lz/archive/2009/03/31/visual-studio-how-to-work-with-different-tab-settings-per-project.aspx" target="_blank">Settings files</a> to achieve the same goal, but I think the Macro is a little easier / faster to switch between them.

<b>EDIT:</b>
Here is a more complete script that I use that will toggle the tabs of C#, SQL, HTML, and javascript, but only when you are editing a file of that language's:

{% highlight vb.net linenos %}
Public Sub ToggleTabs()
  If DTE.ActiveDocument.Language = "CSharp" Then
      Dim currentSetting As Boolean = DTE.Properties("TextEditor", "CSharp").Item("InsertTabs").Value
      DTE.Properties("TextEditor", "CSharp").Item("InsertTabs").Value = Not currentSetting
  End If

  If DTE.ActiveDocument.Language = "SQL" Then
      Dim currentSQLSetting As Boolean = DTE.Properties("TextEditor", "SQL").Item("InsertTabs").Value
      DTE.Properties("TextEditor", "SQL").Item("InsertTabs").Value = Not currentSQLSetting
  End If

  If DTE.ActiveDocument.Language = "HTML" Then
      Dim currentHTMLSetting As Boolean = DTE.Properties("TextEditor", "HTML").Item("InsertTabs").Value
      DTE.Properties("TextEditor", "HTML").Item("InsertTabs").Value = Not currentHTMLSetting
  End If

  If DTE.ActiveDocument.Language = "JScript" Then
      Dim currentJScriptSetting As Boolean = DTE.Properties("TextEditor", "JScript").Item("InsertTabs").Value
      DTE.Properties("TextEditor", "JScript").Item("InsertTabs").Value = Not currentJScriptSetting
  End If
  
End Sub
{% endhighlight %}
