---
layout: post
title: MVP Design Pattern
categories:
- Programming
tags:
- Design Pattern
- Client Side
- GWT
---

## 今天来写写MVP(Model View Presenter)设计模式。

最近正在用GWT(Google Web Toolkit)框架做一个项目的前端代码逻辑，涉及到前端部件在页面上的组合、拼接及逻辑实现，因此随笔记录其中用到的MVP模式。

MVP模式在安卓开发中应用十分广泛。在有用户界面的程序里，模型(Model)和视图(View)的功能不同：模型是用于存储相应业务逻辑的数据，而视图则是用于将不同模型的数据组合在一起，显示它们。为了更好的区分模型和视图对应的功能，MVP模式就诞生了。

### MVP三者的功能分别是什么
> * 视图层(View): 用于展示数据，一般存储相应的小控件(Widget)和相应格式(CSS)的信息，同时保有一个对Presenter的引用;
> * 模型层(Model): 用于存储数据，可以想象其是一些字段(String)和数字(double/float)组成的POJO类;
> * 表现层(Presenter) 用于解藕视图和模型的关系。可用于读取业务逻辑数据，并且把读取的数据更新到相应视图上。或有相应事件发生，去更新/重绘视图层的逻辑。

### 代码举例

在Java代码中，应该如何定义这三者，他们的关系又是什么呢？如下代码给出了示例:

```java
public class Presenter {
  private final View view;
  private Model model;

  public Presenter(View view) {
    // Note in this constructor the view is initialized first
    // Then we will use the view to initialize presenter and bind view with presenter
    this.view = view;
    view.setPresenter(this);
  }

  public View getView() {
    return view;
  }
  
  public void loadModel() {
    model.setData("Katie",25);
  }

  public void updateData() {
    //should process and update the data
  }
}

public class View {
  private Presenter presenter;
  
  public View(){
    //do some construction to the widget within this view 
  }

  @UiHandler("button")
  void click(ClickEvent e) {
    presenter.updateData();
  }
}

public class Model {
  private String name;
  private double age;

  public void setData(String name, double age) {
    this.name = name;
    this.age = age;
  }
}
```

应用举例：
View view = new View();
Presenter presenter = new Presenter(view); // 这里用view去初始化presenter

### 解释说明
上面的代码省略了很多实现细节，但通过将数据和视图分离，展现了MVP模式的一个最基本的实现。一旦某些数据要发生变化时，view会调用presenter中的方法：presenter.updateData()，从而把需要更新的数据传递给view。需要注意的是，view中不包含任何model的信息，所有业务逻辑都由presenter去实现；此外，presenter应该对view中的控件类型和数据结构**一无所知** - 亦即presenter并不需要知道数据在view中的存储结构是什么，只需把模型数据传递给view即可。模型的渲染工作应由view来完成。

用视图去初始化表现层只是一种可能的实现。我曾经看到过用表现层去初始化视图,但还是觉得以上的代码示例更能展现presenter和view的逻辑关系。根据MVP的定义，view应该是无状态的，所有view的活动和更新都应由presenter主导。此外,缓存和数据验证等工作都应由presenter来做，这样就能让view更加专一的完成其职责-通过控件显示数据。

