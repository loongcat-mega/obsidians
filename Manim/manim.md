Manim (Mathematical Animation Engine) 是用于创建数学动画的Python库，皆在帮助用户生成高质量的数学动画。Manim最初由3b1b的创始人开发。目前主要有两个版本Manim Community Edition (Manim CE)和Manim GL

- Manim CE 是由Manim 社区维护的版本，是基于原始的Manim 0.7.x开发的，支持的数学绘图功能包括3D场景、图形、曲线、方程式等
- Manim GL是基于Manim CE重构的版本，使用了现代的OpenGL图形库



![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312190912955.png)

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefined202312190914526.png)


- cairo
用于图形渲染的开源图形库，提供了一套用于绘制矢量图形的API，支持输出到多种不同的图形设备和文件格式
主要特点：跨平台，设备无关性(可以渲染到多种输出设备，如屏幕 打印机 pdf等) ，支持多种图形格式，矢量图支持

- Sox
用于处理音频文件的命令行工具和开发库，提供了 录制 转换 编辑 合并 分割 等功能
主要功能：音频格式转换、音频处理和效果(均衡器 压缩器 变速等)、音频编辑 音频分析等



## Installation

requirements：

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/202312191612688.png)

- 虚拟环境 python >= 3.8 
- numpy == 1.24.0
- 配置`c:\users\dlut2102\anaconda3\envs\mainmgl\lib\site-packages\manimlib\default_config.yml` 中的`temporary_storage`
```bash
pip install manimgl
```
验证是否安装成功
```bash
manimgl
```

## config

```yaml
directories:
  mirror_module_path: True
  output: "F:/manim/_3b1b/videos"
  raster_images: "F:/manim/_3b1b/images/raster"
  vector_images: "F:/manim/_3b1b/images/vector"
  sounds: "F:/manim/_3b1b/sounds"
  data: "F:/manim/_3b1b/data"
  temporary_storage: "F:/manim/_3b1b//manim_cache"
universal_import_line: "from manim_imports_ext import *"
```
```yaml
mirror_module_path 表示模块路径将被复制到输出目录中，如果你的场景或脚本位于特定目录结构中，可能需要启用此选项
remove_mirror_prefix  设置复制模块路径是要删除的前缀
output  生成视频文件的输出目录
raster_iamges 存储视频中使用的位图图像的位置
vector_images 存储视频中使用的矢量图像的位置
sounds  存储音频文件的路径
data  存储数据文件的路径
temporary_storage 用于存储临时文件的路径 如缓存文件等
universal_import_line  设置在场景或脚本中的导入语句

```


```
manim/
├── manimlib/
│   ├── animation/
│   ├── ...
│   ├── default_config.yml
│   └── window.py
├── custom_config.yml
└── start.py
```


```python

```
## usage

```bash
manimgl <code>.py <Scene> <flags>
# or
manim-render <code>.py <Scene> <flags>
```
![image_png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/202312191622387.png)


### Axes

```python
class Axes(VGroup, CoordinateSystem):
    default_axis_config: dict = dict()
    default_x_axis_config: dict = dict()
    default_y_axis_config: dict = dict(line_to_number_direction=LEFT)

    def __init__(
        self,
        x_range: RangeSpecifier = DEFAULT_X_RANGE,
        y_range: RangeSpecifier = DEFAULT_Y_RANGE,
        axis_config: dict = dict(),
        x_axis_config: dict = dict(),
        y_axis_config: dict = dict(),
        height: float | None = None,
        width: float | None = None,
        unit_size: float = 1.0,
        **kwargs
    ):

axes = Axes((-3, 10), (-1, 8))
axes.add_coordinate_labels()
axes = Axes(
            x_range=[0, 7, 1],
            y_range=[0, 6, 1],
            axis_config={"color": BLUE},
            x_axis_config={"numbers_to_include": np.arange(0, 7, 1)},
            y_axis_config={"numbers_to_include": np.arange(0, 6, 1)},
        )


```

#### 缩放到角落

```python
self.play(
            axes.animate.scale(0.75).to_corner(UL),
            run_time=2,
        )
```
### Dot

```python
class Dot(Circle):
    def __init__(
        self,
        point: Vect3 = ORIGIN,
        radius: float = DEFAULT_DOT_RADIUS,
        stroke_color: ManimColor = BLACK,
        stroke_width: float = 0.0,
        fill_opacity: float = 1.0,
        fill_color: ManimColor = WHITE,
        **kwargs
    ):


dot = Dot(fill_color=RED)
dot.move_to(axes.c2p(0, 0))
self.play(FadeIn(dot, scale=0.5))
self.play(dot.animate.move_to(axes.c2p(3, 2)))
self.wait()
self.play(dot.animate.move_to(axes.c2p(5, 0.5)))
self.wait()
```


### Arrow

```python
class Arrow(Line):
    def __init__(
        self,
        start: Vect3 | Mobject,
        end: Vect3 | Mobject,
        stroke_color: ManimColor = GREY_A,
        stroke_width: float = 5,
        buff: float = 0.25,
        tip_width_ratio: float = 5,
        width_to_tip_len: float = 0.0075,
        max_tip_length_to_length_ratio: float = 0.3,
        max_width_to_length_ratio: float = 8.0,
        **kwargs,
    ):


arrow = Arrow(start=start_dot, end=end_dot, buff=0.1)
self.play(ShowCreation(arrow))
self.wait(0.5)


```




### Write

```python
class Write(DrawBorderThenFill):
    def __init__(
        self,
        vmobject: VMobject,
        run_time: float = -1,  # If negative, this will be reassigned
        lag_ratio: float = -1,  # If negative, this will be reassigned
        rate_func: Callable[[float], float] = linear,
        stroke_color: ManimColor = WHITE,
        **kwargs
    ):

lag_ratio :  用于控制动画的延迟。表示前后两个字符（或其他元素）之间的时间间隔比例。所有的字符默认同时开始，可以调整每个字符之间的延迟
 self.play(Write(axes, lag_ratio=0.01, run_time=1))
```

### Make an image
start.py:
```python
from manimlib import *

class SquareToCircle(Scene):
    def construct(self):
        circle = Circle()
        circle.set_fill(BLUE, opacity=0.5)
        circle.set_stroke(BLUE_E, width=4)
        self.add(circle)
```

```bash
manimgl start.py SquareToCircle
```
屏幕上会弹出一个窗口
- 滚动鼠标中键可以上下移动屏幕
- z +鼠标中键  ：缩放屏幕
- s+移动鼠标 ： 平移屏幕
- d+移动鼠标 ： 改变三维视角
- q ：quit
 
```python
class SquareToCircle(Scene):  创建子类，它将是你编写和渲染的场景

def construct(self): 决定怎样去创建物体和执行什么操作

circle=Circle()  创建一个Circle实例

circle.set_fill(BULE,opacity=0.5)  设置填充颜色和不透明度opacity

circle.set_stroke(BULR_E,width=4)  设置轮廓颜色和宽度

self.add(circle)  通过add方法将circle添加到输出屏幕上
```
### Add animations


```python
from manimlib import *

class SquareToCircle(Scene):
    def construct(self):
        circle = Circle()
        circle.set_fill(BLUE, opacity=0.5)
        circle.set_stroke(BLUE_E, width=4)
        square = Square()

        self.play(ShowCreation(square))
        self.wait()
        self.play(ReplacementTransform(square, circle))
        self.wait()
```
	

```bash
manimgl start.py SquareToCircle
```
弹出窗口会播放画正方形和变形的动画 它变成一个圆圈

```python
self.play(ShowCreation(square)) 动画通过play()方法来播放 ShowCreation()展示给定物体的创建过程

self.wait() 暂停默认1秒 self.wait(seconds)

ReplacementTransform(A,B)  A变成B
```

### Text
```python
class TextExample(Scene):
    def construct(self):
        # To run this scene properly, you should have "Consolas" font in your computer
        # for full usage, you can see https://github.com/3b1b/manim/pull/680
        text = Text("Here is a text", font="Consolas", font_size=90)
        difference = Text(
            """
            The most important difference between Text and TexText is that\n
            you can change the font more easily, but can't use the LaTeX grammar
            """,
            font="Arial", font_size=24,
            # t2c is a dict that you can choose color for different text
            t2c={"Text": BLUE, "TexText": BLUE, "LaTeX": ORANGE}
        )
        VGroup(text, difference).arrange(DOWN, buff=1)
        self.play(Write(text))
        self.play(FadeIn(difference, UP))
        self.wait(3)

        fonts = Text(
            "And you can also set the font according to different words",
            font="Arial",
            t2f={"font": "Consolas", "words": "Consolas"},
            t2c={"font": BLUE, "words": GREEN}
        )
        fonts.set_width(FRAME_WIDTH - 1)
        slant = Text(
            "And the same as slant and weight",
            font="Consolas",
            t2s={"slant": ITALIC},
            t2w={"weight": BOLD},
            t2c={"slant": ORANGE, "weight": RED}
        )
        VGroup(fonts, slant).arrange(DOWN, buff=0.8)
        self.play(FadeOut(text), FadeOut(difference, shift=DOWN))
        self.play(Write(fonts))
        self.wait()
        self.play(Write(slant))
        self.wait()
```
```text
此场景中的新类为 Text、VGroup、Write、FadeIn 和 FadeOut。
Text可以创建文本、定义字体等，用法在上面的例子中已经体现的很清楚了。
VGroup 可以将多个VMobject 组合在一起作为一个整体。示例中调用.arrange()方法将子对象按顺序向下排列（DOWN），间距为buff.
Write是一个显示类似书写效果的动画。
FadeIn淡入对象，第二个参数表示淡入的方向。
FadeOut淡出对象，第二个参数表示淡出的方向。

```


```python
from manimlib import *
class TexTransformExample(Scene):
    def construct(self):
        to_isolate = ["B", "C", "=", "(", ")"]
        lines = VGroup(
            # Passing in muliple arguments to Tex will result
            # in the same expression as if those arguments had
            # been joined together, except that the submobject
            # hierarchy of the resulting mobject ensure that the
            # Tex mobject has a subject corresponding to
            # each of these strings.  For example, the Tex mobject
            # below will have 5 subjects, corresponding to the
            # expressions [A^2, +, B^2, =, C^2]
            Tex("A^2", "+", "B^2", "=", "C^2"),
            # Likewise here
            Tex("A^2", "=", "C^2", "-", "B^2"),
            # Alternatively, you can pass in the keyword argument
            # "isolate" with a list of strings that should be out as
            # their own submobject.  So the line below is equivalent
            # to the commented out line below it.
            Tex("A^2 = (C + B)(C - B)", isolate=["A^2", *to_isolate]),
            # OldTex("A^2", "=", "(", "C", "+", "B", ")", "(", "C", "-", "B", ")"),
            Tex("A = \\sqrt{(C + B)(C - B)}", isolate=["A", *to_isolate])
        )
        lines.arrange(DOWN, buff=LARGE_BUFF)
        for line in lines:
            line.set_color_by_tex_to_color_map({
                "A": BLUE,
                "B": TEAL,
                "C": GREEN,
            })

        play_kw = {"run_time": 2}
        self.add(lines[0])
        # The animation TransformMatchingTex will line up parts
        # of the source and target which have matching tex strings.
        # Here, giving it a little path_arc makes each part sort of
        # rotate into their final positions, which feels appropriate
        # for the idea of rearranging an equation
        self.play(
            TransformMatchingTex(
                lines[0].copy(), lines[1],
                path_arc=90 * DEGREES,
            ),
            **play_kw
        )
        self.wait()

        # Now, we could try this again on the next line...
        self.play(
            TransformMatchingTex(lines[1].copy(), lines[2]),
            **play_kw
        )
        self.wait()
        # ...and this looks nice enough, but since there's no tex
        # in lines[2] which matches "C^2" or "B^2", those terms fade
        # out to nothing while the C and B terms fade in from nothing.
        # If, however, we want the C^2 to go to C, and B^2 to go to B,
        # we can specify that with a key map.
        self.play(FadeOut(lines[2]))
        self.play(
            TransformMatchingTex(
                lines[1].copy(), lines[2],
                key_map={
                    "C^2": "C",
                    "B^2": "B",
                }
            ),
            **play_kw
        )
        self.wait()

        # And to finish off, a simple TransformMatchingShapes would work
        # just fine.  But perhaps we want that exponent on A^2 to transform into
        # the square root symbol.  At the moment, lines[2] treats the expression
        # A^2 as a unit, so we might create a new version of the same line which
        # separates out just the A.  This way, when TransformMatchingTex lines up
        # all matching parts, the only mismatch will be between the "^2" from
        # new_line2 and the "\sqrt" from the final line.  By passing in,
        # transform_mismatches=True, it will transform this "^2" part into
        # the "\sqrt" part.
        new_line2 = Tex("A^2 = (C + B)(C - B)", isolate=["A", *to_isolate])
        new_line2.replace(lines[2])
        new_line2.match_style(lines[2])

        self.play(
            TransformMatchingTex(
                new_line2, lines[3],
                #transform_mismatches=True,
            ),
            **play_kw
        )
        self.wait(3)
        self.play(FadeOut(lines, RIGHT))

        # Alternatively, if you don't want to think about breaking up
        # the tex strings deliberately, you can TransformMatchingShapes,
        # which will try to line up all pieces of a source mobject with
        # those of a target, regardless of the submobject hierarchy in
        # each one, according to whether those pieces have the same
        # shape (as best it can).
        source = Text("the morse code", height=1)
        target = Text("here come dots", height=1)

        self.play(Write(source))
        self.wait()
        kw = {"run_time": 3, "path_arc": PI / 2}
        self.play(TransformMatchingShapes(source, target, **kw))
        self.wait()
        self.play(TransformMatchingShapes(target, source, **kw))
        self.wait()
```



#### Tex

Tex类用于渲染`LaTex`公式和数学符号，Tex类支持Latex语法
```python
tex = Tex(r"\frac{1}{2} x")
```
![image_jpg](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/202312191718685.png)


#### Text

Text类只使用于普通文本
