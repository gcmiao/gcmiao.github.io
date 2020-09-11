---
title: Get Depth Value of Depth Buffer
date: 2020/9/11 16:51:00
cover: /img/title_depth_value_in_depth_buffer.jpg
categories:
- Unreal Engine
- Development Tips
tags:
- UE4
- Material
---

In the material editor, when we use the node `SceneTexture` with texture id `SceneDepth`, we get the scene depth in clip space. However, we want to get the depth value(called `DeviceZ` in `Common.ush`) that is stored in the depth buffer. We can use the function `float ConvertToDeviceZ(float SceneDepth)` and `float ConvertFromDeviceZ(float DeviceZ)` to convert between each other.

Since there is no dedicated expression node for either of them, we have to create a custom material expression manually.
1. Create a custom material expression with an input node `SceneDepth` and an output type `CMOT Float 1`.
2. Add `ConvertToDeviceZ(SceneDepth)` to the `Code` field.
3. Pass the result of `SceneTexture:SceneDepth` to the custom expression node.
