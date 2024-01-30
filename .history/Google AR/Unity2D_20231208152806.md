
## Palette

新增调色板用于绘制背景
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230721075823.png)

把 Sprites 切割后拖进Tile Palette中
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230721080507.png)

## 物体碰撞后不旋转

冻结z轴
![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230721080734.png)


## 刚体与其他碰撞箱碰撞抖动

```c#
Vector2 position = rbody.position;
position.x += moveX * speed * Time.deltaTime;
position.y += moveY * speed * Time.deltaTime;
rbody.MovePosition(position);
```

