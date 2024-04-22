
## 写入
```python
#!/usr/bin/python3
# -*- coding: utf-8 -*-

# 导入CSV安装包
import csv

# 1. 创建文件对象
f = open('文件名.csv','w',encoding='utf-8')

# 2. 基于文件对象构建 csv写入对象
csv_writer = csv.writer(f)

# 3. 构建列表头
csv_writer.writerow(["姓名","年龄","性别"])

# 4. 写入csv文件内容
csv_writer.writerow(["l",'18','男'])
csv_writer.writerow(["c",'20','男'])
csv_writer.writerow(["w",'22','女'])

# 5. 关闭文件
f.close()

```
## 读取并绘图
```python
# -*- coding: utf-8 -*-
#steps-loss
import csv
from datetime import datetime

import matplotlib.pyplot as plt

filename = './log.csv'
with open(filename) as f:
    reader = csv.reader(f)
    header_row = next(reader)
    per_epoch = 17967
    epoches, steps, lr, loss = [], [], [], []
    now_steps = []
    count=0
    for row in reader:
        now_steps.append(((int(row[2])-1)*per_epoch)+int(row[3]))
        lr.append(float(row[4]))
        loss.append(float(row[5]))
        count=count+1
print(loss[:10])
plt.plot(now_steps, loss, c='red')

plt.title("steps-loss", fontproperties="SimHei", fontsize=24)
plt.xlabel('steps')
plt.ylabel("loss")
# 仅显示少数y轴刻度和标号
for i in range(min(now_steps), max(now_steps), per_epoch):
    plt.axvline(x=i, color='b', linestyle='--')
    #plt.text(i, min(loss), f"第{i//per_epoch + 1}次", rotation=90, verticalalignment='bottom', fontproperties="SimHei", fontsize=10)
    
plt.tight_layout()  # 自动调整布局以确保所有部分适合图像边界

plt.savefig('loss_steps_plot.png')
plt.show()
```
