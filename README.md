# 代码版圣诞树 · Code Christmas Tree

复刻抖音作品《提前攒一点圣诞的温柔，代码版圣诞树来啦》（作者 **别抢我鸡腿**）
https://www.douyin.com/video/7709364329808346987

一棵由 5 万多颗粒子组成的淡蓝色圣诞树：九圈螺旋灯带缠成圆锥，树顶一小团粉紫爱心，
脚下六圈地面光环，天上撒着 3D 星点，十几朵柔和雪花缓缓落下。

## 在线看

**https://lewis0821.github.io/christmas-tree/**

手机浏览器直接打开即可，无需安装。手指左右拖动旋转，轻点暂停/继续。

## 原作参数

```python
WIDTH, HEIGHT   = 1600, 900
TREE_POINTS     = 50000     # 树更"毛茸茸"
GROUND_POINTS   = 4000
STAR_POINTS     = 1200      # 空中小亮点，跟树/地面一起转
HEART_POINTS    = 1000      # 树顶 3D 爱心的小光点
SNOW2D_POINTS   = 10        # 前景大雪花

CAM_DIST        = 13        # 相机离原点的"前后"距离
CAM_HEIGHT      = 6.0
PITCH           = -0.25     # 负角度 = 俯视
TREE_HEIGHT     = 12.0

loops = 9
spiral_n = int(TREE_POINTS * 0.7)   # 70% 螺旋灯带，其余填充
```

## 实现要点

1. **螺旋灯带**：`h = u**1.25`，`t = u*9*2π`，半径 `base_r*(0.85~1.08)`，
   叠加 `branch_wave = max(0, sin((h*5.8+0.15)*2π))` 做树枝层次。
2. **蓬松填充**：`h = rand**1.9`，半径 `base_r*sqrt(rand)`，造出毛茸茸的轮廓。
3. **树顶爱心**：隐函数 `(x²+y²-1)³ - x²y³ ≤ 0` 拒绝采样，加 z 抖动变"3D 心"。
4. **投影**：`f = (HEIGHT*0.63)/zp`，相机在 `(0, CAM_HEIGHT, CAM_DIST)`，俯视。
5. **合成**：逐通道取最大值（每颗点独立覆盖，密集处不会叠成白色），
   再用最大值膨胀把 1 像素加粗成 3 像素小圆点。

## 文件

| 文件 | 说明 |
| --- | --- |
| `index.html` | 网页版，单文件零依赖，手机/桌面通用 |
| `chris_tree.py` | Python/pygame 桌面版，可实时交互 |
| `export_gif.py` | 导出循环 GIF / MP4 |
| `export_mobile.py` | 导出 9:16 竖屏视频 |

## 本地运行桌面版

```powershell
uv venv .venv --python 3.14
uv pip install --python .venv\Scripts\python.exe pygame-ce numpy pillow
.venv\Scripts\python.exe chris_tree.py
```

---

原作品版权归抖音作者所有，此仓库仅按截图重写代码，供学习使用。