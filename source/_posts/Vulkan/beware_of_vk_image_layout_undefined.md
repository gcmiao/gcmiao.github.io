---
title: Beware of VK_IMAGE_LAYOUT_UNDEFINED
date: 2020/9/11 21:13:23
cover: /img/title_vulkan_500px_dec16.jpg
categories:
- Vulkan
tags:
- Vulkan
---

As a novice of Vulkan, when I first use `VkImageMemoryBarrier`, I intuitively thought that `VK_IMAGE_LAYOUT_UNDEFINED` is a sugar of setting the `oldLayout`. After I did this last time, it left a hidden danger to me.

According to the [official document](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html#synchronization-image-layout-transitions):

> The old layout must either be VK_IMAGE_LAYOUT_UNDEFINED, or match the current layout of the image subresource range.
> If the old layout **matches the current layout** of the image subresource range, the transition **preserves** the contents of that range.
> If the old layout is `VK_IMAGE_LAYOUT_UNDEFINED`, the contents of that range may be **discarded**.

In this way, we'd better **ALWAYS** use a proper old layout unless we really don't care about the existing content of the image.

More information: [Image Layout Transitions](https://www.khronos.org/registry/vulkan/specs/1.2-extensions/html/vkspec.html#synchronization-image-layout-transitions)
