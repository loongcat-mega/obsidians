HUAWEI DevEco Device Tool 是OpenHarmony面向智能设备开发者提供的一站式集成开发环境，支持OpenHarmony的组件按需订制，支持代码编辑、编译、烧录和调试等功能，以插件的形式部署在vscode中

DevEco Device Tool工具主要分为如下4个功能区域。
1. 基本功能区 ：DevEco Device Tool菜单栏，提供基本的工程创建、源码导入、工程配置等功能。
2. 开发板任务区：在工程界面，提供开发板相关操作任务，如源码的编译、镜像的烧录、Monitor串口工具等。
3. 代码编辑器：提供代码的查看、编写和调试等开发功能。
4. 输出控制区：提供日志打印、调试指令输入、命令行指令输入等。

![[image-20230103170727660.png]]

## Pegasus WiFi-IoT套件

#### 核心主板

![[2ae1c595aee345cdaafeb782087fed4e.png]]

#### 底板

![[c2cf78d4f6504f17ac3b157ef0a5bcbc.png]]
![[image-20221202095748377.png]]



## 环境搭建

### 初始化配置文件


---------------  这个文件是系统起来时自定义配置IO口功能  -----------------
`hi3861_hdu_iot_application_master/src/device/hisilicon/hispark_pegasus/sdk_liteos/app/wifiiot_app/init/app_io_init.c`

----------  这个文件是开启Hi3861功能文件，如I2C,I2S，PWM等等----------
`hi3861_hdu_iot_application_master/src/device/hisilicon/hispark_pegasus/sdk_liteos/build/config/usr_config.mk`

------------------   提供用于openharmony初始化和启动服务  ---------

`"ohos_init.h"`

-------------------  sources中的所有.c源文件所包含的头文件的路径  ---------
`F:\embedded\hi3861_for_AI_topic\src\base\iot_hardware\peripheral\interfaces\kits`

------------------------- helloworld_demo.c  -----------------------------
```c++
#include <stdio.h>          // c语言的标准库文件
#include "ohos_init.h"		// 提供用于openharmony初始化和启动服务
void HelloWorld(void)       
{
    // 打印一下 helloworld!的字符串
    printf("helloworld! \r\n"); 
}
// APP_FEATURE_INIT()是openharmony封装好的函数入口，实现main函数的功能。
APP_FEATURE_INIT(HelloWorld);
```

---------------------  BUILD.gn  ----------------------------

```c++
static_library("helloworld_demo") {
  sources = [
    "helloworld_demo.c",
  ]
  include_dirs = [
    "//base/iot_hardware/peripheral/interfaces/kits",
    ]
}
# static_library：将sources中的源文件编译后生成``helloworld_demo``库文件
# sources：实现本工程的功能需要编译的所有.c源文件。
# include_dirs：sources中的所有.c源文件所包含的头文件的路径。
```
## IIC SPI