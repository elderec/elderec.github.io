---
title: Hyperion SmartView tab disappears from Excel
author: Jonathan
layout: post
permalink: /2011/06/hyperion-smartview-tab-disappears-from-excel/
dsq_thread_id:
  - 3684414637
categories:
  - Hyperion
  - Office
  - Windows
tags:
  - Excel
  - Hyperion
  - Office
  - Oracle
---
Smartview 11, user complains that Hyperion tab in Excel 2003, 2007, or 2010 has disappeared.

  1. Check that the plug is not disabled in Excel.
  2. Close Excel
  3. Create %APPDATA%\Microsoft\Addins
  4. Copy HsAddIn.dll, HsSpread.dll, HyperionSmartTag.dll and HsTbar.xla from c:\Hyperion\SmartView\bin over to %APPDATA%\Microsoft\Addins
  5. Unregister each of the existing DLLs by running 
    <pre>regsvr32 /u c:\Hyperion\Smartview\bin\NAMEOF.dll</pre>
    
    Run this command once for each of the copied DLLs.</li> 
    
      * Register each of the copied DLLs by running 
        <pre>regsvr32 %APPDATA%\Microsoft\Addins\NAMEOF.dll</pre>
    
      * Re-Open Excel and the Hyperion tab should be visible again</ol>