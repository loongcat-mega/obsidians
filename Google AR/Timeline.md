Timeline是Unity2017推出的电影序列工具，有助于设计师们更加方便地编辑影片中的动作、声音、事件、视频等等。Timeline无需编写代码，所有操作仅需通过“拖拽”即可完成，从而让设计师们可以更加专注于剧情与故事讲述，加快制作流程。


## Animation Track

![[Pasted image 20230417210259.png]]

目录内新建timeline

层级内新建空物体（director），将timeline拖拽给director， 用于控制动画的播放

![[Pasted image 20230417210835.png]]

创建空的动画轨道Animation Track，并制定一个有用Animator的物体，并把相应动画拖拽进来

添加多个动画轨道，可以重叠播放

#### 录制动画

把要绑定的摄像机拖拽进来，开始录制之后，记录初始位置和结束位置，并选择是否调节摄像机深度，然后结束

## Audio Track

![[Pasted image 20230417211438.png]]

添加音频轨道Audio Track，并绑定一个有Audio Source的对象，（代表利用这个播放器播放）并把相应的音频拖拽到相应轨道

## Activation Track

控制显示和隐藏摄像机的轨道

![[Pasted image 20230417211716.png]]

将要显示的相机绑定到轨道上，并创建相机显示的区间块