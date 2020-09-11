---
title: Three Steps to Eliminate Lag of Camera Movement
date: 2020/9/11 15:16:25
cover: /img/title_camera_delay.jpg
categories:
- Unreal Engine
- Development Tips
tags:
- UE4
- Camera
---

In the project of my HiWi job, the lag of camera translation and rotation is highly noticeable even if I move it slowly. It delayed more than 3 frames visually. Here are 4 places need to be check if the same problem happened to you.

1. Don't get the transformation from `AActor::GetActorLocatino()` and `APlayerController::GetControlRotation()`. Instead, get location and rotation from `APlayerCameraManager::GetCameraCachePOV().Location/Rotation`.
2. Change the tick group, which the getter of transformation is called from, to `Post Update Work` instead of `During Physics`
3. Set CVar `r.OneFrameThreadLag` to 0. You can do this in the `BeginPlay` event.

More information:
1. [ETickingGroup](https://docs.unrealengine.com/en-US/API/Runtime/Engine/Engine/ETickingGroup/index.html)
2. [Low Latency Frame Syncing](https://docs.unrealengine.com/en-US/Platforms/LowLatencyFrameSyncing/index.html)