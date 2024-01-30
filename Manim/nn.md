```python
from manim import *
class FeedForwardLayer():
    def __init__(self, num_nodes,nodesValues=None, node_spacing=0.5):
        self.nodes = [Circle(radius=0.5, fill_color=WHITE, fill_opacity=0.7,color=GRAY_A)\
            for _ in range(num_nodes)]
        self.group = VGroup(*self.nodes).arrange(DOWN, buff=node_spacing)
        
        self.texts=[Tex(f"",color=RED) for _ in range(num_nodes)]
        if nodesValues is not None :
            self.texts=[Tex(f"{value}",color=RED).move_to(self.nodes[i].get_center() )\
                for i,value in enumerate(nodesValues)]
        
        self.groups=VGroup(self.group,*self.texts)
    def get(self):
        return self.groups
    def getNodes(self):
        return self.group
    def GetAll(self):
        return self.group,self.texts
    
    
class ConnLine():
    def __init__(self,start,end,value=1):
        # 连接神经元的蓝色线条
        self.line=Line(start.get_right(),end.get_left(),color=BLUE)
        self.slope=self.line.get_slope()
        self.angle = np.arctan(self.slope)
        # 连线上的数值信息
        self.value=value
        self.tex=Tex(f"{self.value}").next_to(self.line).\
            rotate(self.angle).move_to(self.line.get_center())
        # if self.slope >0 :
        #     self.tex.shift(self.line.get_center()+0.09*UR*self.slope)
        # elif self.slope<0 :
        #     self.tex.shift(self.line.get_center()+0.09*DL*self.slope)
        self.Endpoints=[start,end]
    def get(self):
        return VGroup(self.line,self.tex)
    def get_start(self):
        return self.Endpoints[0]
    def get_end(self):
        return self.Endpoints[1]
    # 重新设置数值
    def setValue(self,value):
        self.value=value
        self.tex.set_text(f"{self.value}")
    # 取得直线参数的数值
    def getValue(self):
        return float(self.tex.get_tex_string())

            
        
        
                
class NeuralNetwork():
    def __init__(self, layer_sizes,nodesValues=None, node_spacing=2):
        # 调用FeedForwardLayer Get方法 获得VGroup(nodes,text)
        self.layers=[]
        # 存储 nodes
        self.pure_layers=[]
        self.layers.append(FeedForwardLayer(layer_sizes[0],\
            nodesValues,node_spacing=node_spacing).get() )
        
        self.pure_layers.append(FeedForwardLayer(layer_sizes[0],\
            nodesValues,node_spacing=node_spacing))
        
        for i,size in enumerate(layer_sizes):
            if i != 0:
                self.layers.append(FeedForwardLayer(size,node_spacing=node_spacing).get())
                self.pure_layers.append(FeedForwardLayer(size,node_spacing=node_spacing))

        self.group = VGroup(*self.layers).arrange(RIGHT, buff=node_spacing)

        #--------连线-------------
        # Add annotations on connections
        # 线条图形及Text
        self.lines = []
        # 线条实例
        self.connlines=[]
        self.para=[0.5 for _ in range(20) ]
        ctl=0
        for i in range(len(self.layers) - 1):
            # 每个node都是circles
            for node1 in self.layers[i][0]:
                for node2 in self.layers[i + 1][0]:
                    ctl=ctl+1
                    # 线条和数值的集合
                    self.lines.append(ConnLine(node1[0],node2[0],value=self.para[ctl]).get())
                    self.connlines.append(ConnLine(node1[0],node2[0]))
                    
        self.groups=VGroup(self.group,*self.lines)
        #self.groups=VGroup(self.lines,self.group)
        # --------------------------------
    
    #def ShowAddInfo(self,values):
        # value[i] node value
        # value[i+1] line value power
    def get_pureLayers(self):
        return self.pure_layers
    
    def get_layers(self):
        return self.layers            
    def get(self):
        return self.groups
    def getLines(self):
        return self.connlines
    

class NeuralNetworkScene(Scene):
    def construct(self):
        #nn=FeedForwardLayer(2).get()
        
        
        self.nn = NeuralNetwork([2, 2, 2, 1],[1,2], node_spacing=1.0)
        nn_=self.nn.get()
        self.add(nn_)
        self.Forword_Feed()
        
        #self.play(Create(nn))
        #self.wait(2)
    def Forword_Feed(self):
        # 每层是一个 Layer
        print(len(self.nn.get_pureLayers()))
        print(type(self.nn.get_pureLayers()))
        for i,layer in enumerate(self.nn.get_pureLayers()):
            # 每个node结点
            if i!=0:
                
                print(i)
                # nodes 为ciecle结点
                # texts 为其值
                nodes,texts =layer.GetAll()
                print(type(nodes))
                #print(type(node1))
                print(type(nodes[0]))
                print(type(nodes[1]))
                print(type(texts))
                
                # 遍历该层的每个结点 寻找其入边
                for j,node in enumerate(nodes):
                    values=[]
                    for lineInfo in self.nn.getLines():
                        if node == lineInfo.get_end():
                            values.append(float(texts[j].get_tex_string()))
                    #proc_text,res_text=self.nn.ShowAddInfo(values)s:
                    num=values[0]*values[1]
                    target_str=str(values[0])+"*"+str(values[1])
                    for i in range(2,len(values),2):
                        num=num+values[i]*values[i+1]
                        target_str=target_str+"+"+str(values[i])+"*"+str(values[i+1])
                    target_str=target_str+"="+str(num)
                    proc_text=Tex(target_str) 
                    res_text=Tex(f"{num}")
                    
                    proc_text.set_color(RED).move_to(3*UP)
                    res_text.move_to(node.get_center())
                    self.play(Write(proc_text))
                    self.wait(0.5)
                    self.play(Transform(proc_text,res_text),run_time=0.5)
                    self.add(res_text)
                    self.wait(0.5)
                    self.play(FadeOut(proc_text))
```