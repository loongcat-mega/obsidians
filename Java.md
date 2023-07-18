## UI


JFrame是Java Swing库中的一个类，用于创建窗口应用程序的顶级容器。它提供了一个可见的窗口，可以包含其他图形组件（如按钮、标签、文本框等）以及用户界面的各种元素。

JFrame的作用包括：

提供窗口的外观和行为：JFrame充当了应用程序窗口的容器，可以设置窗口的标题、尺寸、位置和关闭行为等。它提供了许多方法来管理窗口的外观和行为，例如setTitle()、setSize()、setLocation()和setDefaultCloseOperation()等。

容纳和布局其他组件：JFrame可以包含其他的Swing组件，如按钮、标签、文本框等。您可以使用不同的布局管理器（如FlowLayout、BorderLayout、GridLayout等）将这些组件放置在JFrame内的适当位置。

响应用户的操作：JFrame可以通过添加事件监听器来响应用户的操作，例如按钮的点击事件、菜单的选择等。您可以在JFrame中注册相应的事件监听器，并编写处理逻辑以响应这些事件。

显示和交互：JFrame提供了方法来设置窗口的可见性，可以显示或隐藏窗口。用户可以通过与窗口上的组件进行交互来与应用程序进行互动。

总之，JFrame是创建图形化用户界面的基础，它提供了窗口容器和一系列管理窗口的方法，使得开发者能够方便地构建和管理窗口应用程序。

## 向文件写入数据

```java

try{
    //创建输出流对象
    fos=new FileOutputStream(path,true);//追加写入
    //创建中转数组
    byte[]words=str.getBytes();
    fos.write(words);
}catch(FileNotFoundExcepetion e){
    e.printStackTrace();
}catch(IOException e){
    e.printStackTrace();
}finally{
    //关流
    try{
        if(fos!=null)
            fos.close();
    }catch(IOException e){
        e.printStackTrace();
    }
    
}


```
java学习的感悟：

java功能强大但体系庞大复杂，在小学期的学习过程中，我逐渐了解了java的基本语法，数据类型，流程控制和函数等。
并且学习了java如何读写文本数据，如何实现与数据库互联，如何打造UI界面等。
加深了对面向对象编程的理解。

项目中的分工：
UI设计