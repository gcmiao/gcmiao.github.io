---
title: How to Debug UnrealBuildTool
date: 2020/4/5 21:43:02
categories:
- Unreal Engine
- Build Tips
tags:
- UE4
---

Steps:
1. Set UnrealBuildTool as startup project.
2. Open UnrealBuildTool Properties page.
3. Add arguments to the *'Command line arguments'* under the section *'Debug'*
   + If you want to debug the phase of generating project file, use `-ProjectFiles`
   + If you want to debug building phase, use parameters from the *'BuildCommandLine'* of UE4 Property Page which is exactly executed when you build the project as usual.

Tips: Sometime the break points will not be triggered without any reason. It often happens after you execute GenerateProjectFiles.bat or other scripts that call UBT from commandline. In this case, you just need to rebuild UBT.

Then you can start your debug. Have fun!

------
<table style="text-align:left">
  <tr>
    <td style="border:none;padding: 0px;">
        <sup>For any problems about this article, feel free to rise an issue to me.</sup>
        <!-- Place this tag in your head or just before your close body tag. -->
        <script async defer src="https://buttons.github.io/buttons.js"></script>
        <!-- Place this tag where you want the button to render. -->
        <a class="github-button" href="https://github.com/gcmiao/gcmiao.github.io/issues" data-icon="octicon-issue-opened" aria-label="Issue gcmiao/gcmiao.github.io on GitHub">Issue</a>
    </td>
  </tr>
  <tr>
    <td style="border:none;padding: 0px;">
        <sup>If this article is usful for you, welcome to follow my github.</sup>
        <!-- Place this tag where you want the button to render. -->
        <a class="github-button" href="https://github.com/gcmiao" aria-label="Follow @gcmiao on GitHub">Follow @gcmiao</a>
    </td>
  </tr>
</table>

[Go back Home](/)