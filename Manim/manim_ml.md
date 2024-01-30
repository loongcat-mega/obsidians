## 基本用法




```bash
manim [ops] <场景类名> <输出动画的文件名>

usage: manim.py [-h] [-p] [-w] [-s] [-l] [-m] [--high_quality] [-g] [-i] [-f]
                [-t] [-q] [-a] [-o FILE_NAME] [-n START_AT_ANIMATION_NUMBER]
                [-r RESOLUTION] [-c COLOR] [--sound] [--leave_progress_bars]
                [--media_dir MEDIA_DIR]
                [--video_dir VIDEO_DIR | --video_output_dir VIDEO_OUTPUT_DIR]
                [--tex_dir TEX_DIR] [--livestream] [--to-twitch]
                [--with-key TWITCH_KEY]
                [file] [scene_names [scene_names ...]]

positional arguments:
  file                  path to file holding the python code for the scene
  scene_names           Name of the Scene class you want to see

optional arguments:
  -h, --help            show this help message and exit
  -p, --preview         Automatically open the saved file once its done
  -w, --write_to_movie  Render the scene as a movie file
  -s, --save_last_frame  Save the last frame
  -l, --low_quality     Render at a low quality (for faster rendering)
  -m, --medium_quality  Render at a medium quality
  -h, --high_quality        Render at a high quality 
  -g, --save_pngs       Save each frame as a png  
  -i, --save_as_gif     Save the video as gif
  -f, --show_file_in_finder Show the output file in finder  
  -t, --transparent     Render to a movie file with an alpha channel  
  -q, --quiet
  -a, --write_all       Write all the scenes from a file
  -o FILE_NAME, --file_name FILE_NAME  Specify the name of the output file, ifit should be
                        different from the scene class name
  -n START_AT_ANIMATION_NUMBER, --start_at_animation_number     START_AT_ANIMATION_NUMBER
                        Start rendering not from the first animation,   butfrom another, specified by its index. If you passin two comma separated values, e.g. "3,6", it will end the rendering at the second value
  -r RESOLUTION, --resolution RESOLUTION Resolution, passed as "height,width"
  -c COLOR, --color COLOR  Background color
  --sound               Play a success/failure sound
  --leave_progress_bars Leave progress bars displayed in terminal
  --media_dir MEDIA_DIR directory to write media 
  --video_dir VIDEO_DIR directory to write file tree for video
  --video_output_dir VIDEO_OUTPUT_DIR  directory to write video
  --tex_dir TEX_DIR     directory to write tex
  --livestream          Run in streaming mode
  --to-twitch           Stream to twitch
  --with-key TWITCH_KEY 	Stream key for twitch
```


## Constants

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231221093902.png)

PI : The ratio of the circumference of a circle to its diameter.

TAU : The ratio of the circumference of a circle to its radius.
## Usage

### Basic Concepts

#### arrange && next_to &&Vgroup

`arrange` 方法是用于排列组内的元素的，它会按照指定的方向和缓冲 (`buff`) 将组内的元素进行排列。并且它们之间的间距为 `buff`

`next_to` 方法是用于将一个对象放置在另一个对象的相邻位置。你需要指定方向 (`direction`) 和缓冲 (`buff`)

`VGroup` 在这里主要用于将对象s组合在一起。`VGroup` 不会自动对其成员进行垂直或水平排列，而是按照它们在列表中的顺序进行组合

```python
from manim import *
class ManimCELogo(Scene):
    def construct(self):
    		# 设置背景颜色
        self.camera.background_color = "#ece6e2"
        logo_green = "#87c2a5"
        logo_blue = "#525893"
        logo_red = "#e07a5f"
        logo_black = "#343434"
        # 设置Latex文本，填充颜色 和缩放
        ds_m = MathTex(r"\mathbb{M}", fill_color=logo_black).scale(7)
        # 向某个方向平移
        ds_m.shift(2.25 * LEFT + 1.5 * UP)
        circle = Circle(color=logo_green, fill_opacity=1).shift(LEFT)
        square = Square(color=logo_blue, fill_opacity=1).shift(UP)
        triangle = Triangle(color=logo_red, fill_opacity=1).shift(RIGHT)
        # 组合起来
        logo = VGroup(triangle, square, circle, ds_m)  # order matters
        logo.move_to(ORIGIN)
        self.add(logo)

```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231220204251.png)

#### Brace  get_tex

`get_tex` 是 `Brace` 对象的一个方法，用于在 `Brace` 对象的中间位置添加 LaTeX 文本

```python

# 大括号
class Brace(Tex):
    def __init__(
        self,
        # 要覆盖的对象
        mobject: Mobject,
        # 括号尖尖方向
        direction: Vect3 = DOWN,
        buff: float = 0.2,
        tex_string: str = R"\underbrace{\qquad}",
        **kwargs
    ):


from manim import *
class BraceAnnotation(Scene):
    def construct(self):
        dot = Dot([-2, -1, 0])
        dot2 = Dot([2, 1, 0])
        # 两点之间的线段
        line = Line(dot.get_center(), dot2.get_center()).set_color(ORANGE)
        # underline
        b1 = Brace(line)
        b1text = b1.get_text("Horizontal distance")
        # get_unit_vector 获取该方向的的单位向量
        b2 = Brace(line, direction=line.copy().rotate(PI / 2).get_unit_vector())
        b2text = b2.get_tex("x-x_1")
        self.add(line, dot, dot2, b1, b2, b1text, b2text)
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231220205323.png)
#### Arrow && NumberPlane && arrow.get_end() && next_to

cartesian 
```python
#Creates a cartesian plane with background lines
class NumberPlane(x_range=(- 7.111111111111111, 7.111111111111111, 1), y_range=(- 4.0, 4.0, 1), x_length=None, y_length=None, background_line_style=None, faded_line_style=None, faded_line_ratio=1, make_smooth_after_applying_functions=True, **kwargs)

from manim import *
class VectorArrow(Scene):
    def construct(self):
        dot = Dot(ORIGIN)
        arrow = Arrow(ORIGIN, [2, 2, 0], buff=0)
        numberplane = NumberPlane()
        origin_text = Text('(0, 0)').next_to(dot, DOWN)
        tip_text = Text('(2, 2)').next_to(arrow.get_end(), RIGHT)
        self.add(numberplane, dot, arrow, origin_text, tip_text)
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231220210358.png)


#### BoolOps && move_to && shift
`move_to`：该方法将对象移动到指定的坐标位置，而不是相对于当前位置的偏移。
`shift`：该方法用于将对象相对于当前位置进行平移，而不是将其移动到一个绝对位置。

```python
from manim import *

class BooleanOperations(Scene):
    def construct(self):
    		# 创建一个椭圆 将其移至LEFT
        ellipse1 = Ellipse(
            width=4.0, height=5.0, fill_opacity=0.5, color=BLUE, stroke_width=10
        ).move_to(LEFT)
        # 复制上述椭圆 移至RIGHT
        ellipse2 = ellipse1.copy().set_color(color=RED).move_to(RIGHT)
        bool_ops_text = MarkupText("<u>Boolean Operation</u>").next_to(ellipse1, UP * 3)
        # 将上面元素打包组合，移至UP*3
        ellipse_group = Group(bool_ops_text, ellipse1, ellipse2).move_to(LEFT * 3)
        self.play(FadeIn(ellipse_group))
        
			# 计算两个元素的交集
        i = Intersection(ellipse1, ellipse2, color=GREEN, fill_opacity=0.5)
        # 移动至特定位置 并且在移动过程中缩放
        self.play(i.animate.scale(0.25).move_to(RIGHT * 5 + UP * 2.5))
        intersection_text = Text("Intersection", font_size=23).next_to(i, UP)
        self.play(FadeIn(intersection_text))

        u = Union(ellipse1, ellipse2, color=ORANGE, fill_opacity=0.5)
        union_text = Text("Union", font_size=23)
        # 移动到i下面 buff 垂直间距
        self.play(u.animate.scale(0.3).next_to(i, DOWN, buff=union_text.height * 3))
        union_text.next_to(u, UP)
        self.play(FadeIn(union_text))

        e = Exclusion(ellipse1, ellipse2, color=YELLOW, fill_opacity=0.5)
        exclusion_text = Text("Exclusion", font_size=23)
        self.play(e.animate.scale(0.3).next_to(u, DOWN, buff=exclusion_text.height * 3.5))
        exclusion_text.next_to(e, UP)
        self.play(FadeIn(exclusion_text))

        d = Difference(ellipse1, ellipse2, color=PINK, fill_opacity=0.5)
        difference_text = Text("Difference", font_size=23)
        self.play(d.animate.scale(0.3).next_to(u, LEFT, buff=difference_text.height * 3.5))
        difference_text.next_to(d, UP)
        self.play(FadeIn(difference_text))
```

#### GrowFromCenter &&  Transform && MoveAlongPath

```python
# Introduce an Mobject by growing it from its center.
class GrowFromCenter(mobject=None, *args, use_override=True, **kwargs)
# A Transform transforms a Mobject into a target Mobject.
class Transform(mobject=None, *args, use_override=True, **kwargs)
# Make one mobject move along the path of another mobject. .. rubric:: Example
class MoveAlongPath(mobject=None, *args, use_override=True, **kwargs)

from manim import *

class PointMovingOnShapes(Scene):
    def construct(self):
        circle = Circle(radius=1, color=BLUE)
        dot = Dot()
        dot2 = dot.copy().shift(RIGHT)
        self.add(dot)

        line = Line([3, 0, 0], [5, 0, 0])
        self.add(line)
			# 从中心生长
        self.play(GrowFromCenter(circle))
        # 从dot到dot2的转变动画
        self.play(Transform(dot, dot2))
        #沿着路径运行
        self.play(MoveAlongPath(dot, circle), run_time=2, rate_func=linear)
        self.play(Rotating(dot, about_point=[2, 0, 0]), run_time=1.5)
        self.wait()
```
annotations
#### ReplacementTransform && SurroundingRectangle

```python
from manim import *
# A rectangle surrounding a Mobject
class SurroundingRectExample(Scene):
    def construct(self):
			# 自带下划线的Title
        title = Title("A Quote from Newton")
        quote = Text(
            "If I have seen further than others, \n"
            "it is by standing upon the shoulders of giants.",
            color=BLUE,
        ).scale(0.75)
        box = SurroundingRectangle(quote, color=YELLOW, buff=MED_LARGE_BUFF)

        t2 = Tex(r"Hello World").scale(1.5)
        box2 = SurroundingRectangle(t2, corner_radius=0.2)
        mobjects = VGroup(VGroup(box, quote), VGroup(t2, box2)).arrange(DOWN)
        self.add(title, mobjects)
```
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231220214910.png)

```python
# Replaces and morphs a mobject into a target mobject.
class ReplacementTransform(mobject=None, *args, use_override=True, **kwargs)

from manim import *

class MovingFrameBox(Scene):
    def construct(self):
        text=MathTex(
            "\\frac{d}{dx}f(x)g(x)=","f(x)\\frac{d}{dx}g(x)","+",
            "g(x)\\frac{d}{dx}f(x)"
        )
        self.play(Write(text))
        framebox1 = SurroundingRectangle(text[1], buff = .1)
        framebox2 = SurroundingRectangle(text[3], buff = .1)
        self.play(
            Create(framebox1),
        )
        self.wait()
        self.play(
            ReplacementTransform(framebox1,framebox2),
        )
        self.wait()
```

#### Special Camera Settings && i2gc &&i2gp
Abbreviation		n 缩写 缩略形式
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/20231221092050.png)

```python
# i2gc
>>> from manim import Axes
>>> ax = Axes()
>>> parabola = ax.plot(lambda x: x**2)
>>> ax.input_to_graph_coords(x=3, graph=parabola)
(3, 9)


# icgp
from manim import *

class InputToGraphPointExample(Scene):
    def construct(self):
        ax = Axes()
        curve = ax.plot(lambda x : np.cos(x))

        # move a square to PI on the cosine curve.
        position = ax.input_to_graph_point(x=PI, graph=curve)
        sq = Square(side_length=1, color=YELLOW).move_to(position)

        self.add(ax, curve, sq)
```
add_updater
```python
from manim import *

class NextToUpdater(Scene):
    def construct(self):
        def dot_position(mobject):
            mobject.set_value(dot.get_center()[0])
            mobject.next_to(dot)

        dot = Dot(RIGHT*3)
        label = DecimalNumber()
        label.add_updater(dot_position)
        self.add(dot, label)

        self.play(Rotating(dot, about_point=ORIGIN, angle=TAU, run_time=TAU, rate_func=linear))
```


```python
from manim import *
# This is a Scene, with special configurations and properties that make it suitable for cases where the camera must be moved around
class FollowingGraphCamera(MovingCameraScene):
    def construct(self):
    		# 保存相机当前状态
        self.camera.frame.save_state()

        # create the axes and the curve
        ax = Axes(x_range=[-1, 10], y_range=[-1, 10])
        # ax 绘制曲线
        graph = ax.plot(lambda x: np.sin(x), color=BLUE, x_range=[0, 3 * PI])
			
        # create dots based on the graph
        # 定义域最小最大值
        moving_dot = Dot(ax.i2gp(graph.t_min, graph), color=ORANGE)
        dot_1 = Dot(ax.i2gp(graph.t_min, graph))
        dot_2 = Dot(ax.i2gp(graph.t_max, graph))
		
        self.add(ax, graph, dot_1, dot_2, moving_dot)![tmp9C9.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/tmp9C9.png)
![tmpF29.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/tmpF29.png)

        self.play(self.camera.frame.animate.scale(0.5).move_to(moving_dot))
			# 跟随moving_dot移动
        def update_curve(mob):
            mob.move_to(moving_dot.get_center())
			# camera添加更新函数
			# Add an update function to this mobject.
        self.camera.frame.add_updater(update_curve)![tmpFFE3.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/tmpFFE3.png)
![tmp2C2.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/tmp2C2.png)
![tmp583.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/tmp583.png)

        # Make one mobject move along the path of another mobject.
        self.play(MoveAlongPath(moving_dot, graph, rate_func=linear))
        self.camera.frame.remove_updater(update_curve)
			# Transforms a mobject to its last saved state 
			# used with save_state()s
        self.play(Restore(self.camera.frame))
```

## nn

### FeedForwardLayer
```python
class FeedForwardLayer(VGroupNeuralNetworkLayer):
    """Handles rendering a layer for a neural network"""

    def __init__(
        self,
        num_nodes,
        # SMALL_BUFF: float = 0.1
        layer_buffer=SMALL_BUFF / 2,
        node_radius=0.08,
        node_color=manim_ml.config.color_scheme.primary_color,
        node_outline_color=manim_ml.config.color_scheme.secondary_color,
        rectangle_color=manim_ml.config.color_scheme.secondary_color,
        node_spacing=0.3,
        rectangle_fill_color=manim_ml.config.color_scheme.background_color,
        node_stroke_width=2.0,
        rectangle_stroke_width=2.0,
        animation_dot_color=manim_ml.config.color_scheme.active_color,
        activation_function=None,
        **kwargs
    ):
```
### NeuralNetwork

```python
class NeuralNetwork(Group):
    """Neural Network Visualization Container Class"""

    def __init__(
        self,
        input_layers,
        layer_spacing=0.2,
        animation_dot_color=manim_ml.config.color_scheme.active_color,
        edge_width=2.5,
        dot_radius=0.03,
        title=" ",
        layout="linear",
        layout_direction="left_to_right",
        debug_mode=False
    ):
```

## 基础动画类Scene

所有的动画均是scene类的子类产生的，因此scene的功能比较少，主要是对一些基础的属性进行配置
```python
# manimlib\scene\scene.py
CONFIG = {
        "camera_class": Camera,
        "camera_config": {},
        "file_writer_config": {},
        "skip_animations": False,
        "always_update_mobjects": False,
        "random_seed": 0,
        "start_at_animation_number": None,
        "end_at_animation_number": None,
        "leave_progress_bars": False,
    }
```
其中 **`camera_config`** 是对视频的处理，由 **`Camera`类** 完成:
```python
# 镜头往右前方拉的效果
 self.play(s.animate.set_color(PURPLE).set_opacity(0.5).shift(2*LEFT).scale(3))
```