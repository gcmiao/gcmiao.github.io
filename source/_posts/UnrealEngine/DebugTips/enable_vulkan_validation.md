---
title: How to Enable Vulkan Validation Layer
date: 2020/4/26 18:44:39
cover: /img/Vulkan_500px_Dec16.jpg
categories:
- Unreal Engine
- Debug Tips
tags:
- UE4
- Vulkan
---

Vulkan validation layer in UE4 is not enabled by default. We can see the codes in `VulkanLayers.cpp` around line 300:
```C++
const int32 VulkanValidationOption = GValidationCvar.GetValueOnAnyThread();
if (!bVkTrace && VulkanValidationOption > 0)
{
    ...
}
```
The validation layer will be enabled only if `VulkanValidationOption` has a proper non-zero value when creating `VkInstance`. The GValidationCvar is defined at the begining of the same file like this:
```C++
TAutoConsoleVariable<int32> GValidationCvar(
	TEXT("r.Vulkan.EnableValidation"),
	0,
	TEXT("0 to disable validation layers (default)\n")
	TEXT("1 to enable errors\n")
	TEXT("2 to enable errors & warnings\n")
	TEXT("3 to enable errors, warnings & performance warnings\n")
	TEXT("4 to enable errors, warnings, performance & information messages\n")
	TEXT("5 to enable all messages"),
	ECVF_ReadOnly | ECVF_RenderThreadSafe
);
```
We have several options of validation level, and the default value is 0.

Of course we can assign a new value to GValidationCvar. But make sure it is setted before creating 'VKInstance'.
Another safe way to do this is add startup argument 'vulkanvalidation=(xxx)' to your project or UE4 project property. You don't even need to modify any codes by this way.

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