# Convert Coordinate in Left-handed System to Right-handed System

UE4 is using left-handed coordinate system which is clockwise when viewing from the top. However the data we use is sampled in right-handed system which is counter-clockwise when viewing from the top. If we use the data directly in UE4, we gets a lying down viewport. So we have to convert the coordinate into left-handed coordinate system at first.

We have already known that $Coordinate\;in\;Screen\;Space = Projection Matrix \times View Matrix \times Model Matrix \times Coordinate\;in\;Local\;Space$. The coordinate conversion happens in *World Space* that we can get from $Model Matrix \times Coordinate\;in\;Local\;Space$. The only thing we need to do is modifying the *Model Matrix* that is equal to $Translation \times Rotation \times Scale$. But we cannot simplely multipy an extra rotation matrix on the left or right side because it will break the construction order of model matrix. So We have to transfer each component of model matrix respectively.

For example, we need to convert a position $(x, y, z)$ to $(-z, x, y)$, the new position is $x' = y, y' = z, z' = -x$. We can get translation component from model matrix directly then shift the three axes.
```
auto translation = modelMatrix[3]
auto newTrans = glm::vec3(translation.y, translation.z, -translation.x)
```
 For rotation, we can convert the rotation matrix without scale into quaternion and shift the three axes as before in quaternion then convert it back to rotation matrix. It is easier to do based on glm library than constructing a rotation matrix.
 ```
 auto newQuat = glm::quat(oldQuat.W, oldQuat.Y, oldQuat.Z, -oldQuat.X);
 auto rotMat = glm::mat3_cast(newQuat);
 ```
For scale, I haven't tried it yet but it would be similar.

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