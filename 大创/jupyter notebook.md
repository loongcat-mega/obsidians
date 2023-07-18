基于网页的用于交互计算的应用程序，可以在网页页面中**直接编写代码**和**运行代码**，代码的**运行结果**也会直接在代码块下显示的程序


获取配置文件所在路径的命令
```
jupyter notebook --generate-config
```
![[Pasted image 20230303211648.png]]
 

进入配置文件后查找关键词`c.NotebookApp.notebook_dir`,修改为目标路径，并取消该行注释

![[Pasted image 20230303212635.png]]
![[Pasted image 20230303212908.png]]



查看当前版本
```python
import sys
print(sys.version)#输出当前版本
print(sys.executable)#输出解释器路径

```

在MinSpore环境中中安装好ipykernel
```python
pip install ipykernel

```
添加内核
```python
python -m ipykernel install --user --name=MindSpore --display-name MindSpore
```
查看当前有哪些内核
```python
jupyter kernelspec list

```
删除name的环境
```python
jupyter kernelspec uninstall MindSpore
```



更改内核名字

![[Pasted image 20230307120253.png]]