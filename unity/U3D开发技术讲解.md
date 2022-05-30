1. 向量（Vector）进行加减运算后使用Vector中的方法`normalized`进行**单位化**为方向，相当于y = ax +b中的a，也就是斜率
2. 
```
物体所在点=(鼠标点击的点—物体所在点).normalized*每帧移动速度+物体所在点
```
3. 向量在程序中的应用
- 加 用来移动
- 减 用来确定方向
- 点乘 用于角度的计算、两个向量之间的夹角、一个物体转向另一个物体有一个旋转的角度时使用
- 叉乘 同时垂直于u和v的向量，用于计算方位
4. 点乘（**a·b = b·a**）
- C = `Vector.Dot(a,b)`点积
- `Mathf.Acos(C)`反余弦arc-cos
- `Mathf.Rad2Deg` 弧度转度，弧度到度的转化常量
- 角度 = `Mathf.Acos(C) * Mathf.Rad2Deg`
- 使用`transform.RotateAround(位置，方向，角度)`旋转物体
- ![](https://upload-images.jianshu.io/upload_images/6322775-9c29a7dcbf655679.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. `float.IsNaN()`
- 如果值为NaN返回true
- NaN 实际上就是 Not a Number的简称。0.0f/0.0f的值就是NaN
6. 叉乘（**a x b = - b x a**）
- `Vector3.Cross (a, b)`叉积，返回同时垂直于a和b的向量
- `Vector3.zero`原点
- `Vector3.Distance (a, b)`返回a，b之间的距离
- `Mathf.Asin`反正弦arc-sin
- ![](https://upload-images.jianshu.io/upload_images/6322775-67183314419ceb71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
7. 点乘和叉乘的**区别**：
- 都能计算角度
- **叉乘**能顺带**计算距离**
8. 矩阵相乘进行**平移变换**
```
transform.position = Matrix.translate(x,y,z) * transform.position
```
9. `Transform.localScale` 自身缩放
- `Transform.localScale = Matrix.Scale(倍数f) * transform.localScale`
10. 四元数（Quaternion）
- 控制旋转
11. 平滑旋转、**球形插值Slerp()**
- ![](https://upload-images.jianshu.io/upload_images/6322775-f54fdd1da974006c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)