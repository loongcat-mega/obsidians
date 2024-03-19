```CMakeLists
cmake_minimum_required(VERSION 3.24)  
project(untitled)  
  
set(CMAKE_CXX_STANDARD 20)  
foreach(FILE_PATH ${DEMO_RESOURCE})  
    add_executable(${FILE_NAME} ${FILE_PATH} )  
endforeach()
```

![image.png](https://yaaame-1317851743.cos.ap-beijing.myqcloud.com/undefinedPasted%20image%2020230131160833.png)
