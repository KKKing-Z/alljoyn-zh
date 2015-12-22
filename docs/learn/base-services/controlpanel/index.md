# Control Panel Service 控制面板服务

The AllJoyn&trade; Control Panel Service framework provides a
simple way for applications to render a UI widget set to
interact with remote devices. The framework is composed of
a standard set of interfaces, that when implemented on specific
object paths, allows for dynamic rendering of widgets in a UI
on remote devices. The Control Panel Service abstracts the
details of the AllJoyn Control Panel interface by allowing
the use of higher level APIs and a Code Generator to create
the widget elements. A Control Panel does not dictate how a
UI should look; it provides hints/information about what an
element is capable of, where it should be rendered on a screen,
and synchronization with other applications.
AllJoyn&trade; 控制面板服务架构为应用程序提供了一个便捷的与远程设备进行互动的用户界面小部件。该架构是标准接口的一部分，当被设定了特定的对象路径时，能够动态呈现在远程设备上的用户界面中的小部件。控制面板服务通过允许使用较高级的 API 和代码生成器创建小部件元素，抽象 AllJoyn 控制面板接口的细节。

## Concepts and Terminology 概念和术语

### Controller and Controllee 控制者和受控制者

Two roles exist: 有两个角色：
* Controller. This is the application that renders the control panel. 控制者。操纵控制面板的应用程序。
* Controlee. This is the application that advertises the control panel. 受控制和。广告控制面板的应用程序。

### Control Panel 控制面板

Collection of Widgets that allow a user to interact with a device.
A control panel is defined and announced by a Controllee; and
discovered and displayed by a Controller. A device can have more
than one, and can be defined on a per-language basis.
允许用户与设备进行互动的一组小部件。控制面板由受控制者定义和发布；被控制者发现和显示。一个设备可以有多个控制面板，可以分别定义语言。

### Control Panel Service Code Generator 控制面板服务代码生成器

A tool that aids in the generation of code that will represent a
Control Panel. This tool takes in an XML file that defines
the control panel widgets and generates corresponding code that
implements a specific control panel. Note that XML is not
used as the internal representation of the widgets in the
Control Panel Service, nor is it sent over the wire to the
Controller. 一个用来辅助生成控制面板的工具。这个工具把定义控制面板小部件的 XML 文件生成相应的代码以生成特定的控制面板。需要注意的是，XML 既不是一种小部件在控制面板服务中的内部表现形式，也不会被发送给控制者。

### Types of widgets modules 小部件模块的类型

A Control Panel can be expressed in XML, fed through the Code
Generator, and connected to developer software. The following
UI elements make up a Control Panel:
控制面板以 XML 形式表示，由代码生成器生成，连接到开发者的软件。以下用户界面元素组成了控制面板：

* Container. Container UI element. Allows grouping of widgets
  together. Must contain at least one child element. Container。Container。Container 用户界面元素。它把小部件组合在一起。必须包含至少一个子元素。
* Label. UI element that functions as a read only label of text. Label。用作只读标签的用户界面元素。
* Action. UI element represented by a button that either executes
  code on the Controllee, or opens a Dialog Widget as a confirmation
  before executing. Action。作为受控制者的一个可以执行代码的按钮，或者在执行前打开的一个用于确认的对话框小部件的用户界面元素。
* Dialog. UI dialog element. Has a dialog message and up to 3
  choices of buttons. Dialog. 用户界面对话框元素。有一个对话框消息和 3 个选项按钮。
* Property. UI element used to display a value and possibly edit
  it. This widgets via the hints attribute is used to represent a
  slider, spinner, radio button, etc. Property。用于显示某一值或编辑它的用户界面元素。这个部件通过细微属性，表示滑块、微调框、单选按钮等。

### UI Adaptive Layer UI 自适应层

In order to aid in use and interact with a Control Panel, a helper
library of sorts is available as part of the framework. The purpose
of this software layer is to facilitate the discovery of the widget
elements using AllJoyn Introspection which starts at the object
path of the Control Panel interface found through
[Service Level Discovery][about]. It then parses each child of
this path to determine what each child is and creates the native
platform UI element to interact with each discovered widget.
The native platform elements are the default UI for the given
platform, on Android a button looks like a default Android Button;
on iOS the device will have a native look and feel for and not
look like another OS. Developers have the option to overload the
generation of the platform widgets to provide a custom look and feel.
为了辅助运行控制面板，或与之进行互动，架构中加入了辅助分层。这个软件分层的目的是促进发现使用 AllJoyn 自省机制的小部件元素。自省机制在控制面板接口对象路径中启用，这些接口在 [Service Level Discovery][about] 体现。它解析每一个这个路径的每一个 child，了解 child 的内容，随后建立本地的用户界面元素用来与每一个发现的小部件进行互动。本地平台元素是给定平台的默认用户界面，在 Android 中的按钮属于 Android 风格；相应的，IOS 设备也有自己特定的区别于其它设备的样式。开发者可以定制平台小部件的外观和风格。

## How Does It Work? 运行方式

Under the hood, there are many interfaces that make up the
building blocks to create complex widgets. These interfaces,
when implemented by a BusObject on a specific object path,
determine the language, widget, text, and constraints.
The service, through the APIs and helper classes, allows
for simple use of the AllJoyn framework to register BusObjets
that implement certain widgets and automatically provide hints
on how the widget should render, signals to synchronize between
applications, and default values/constraints.
在内部，有很多接口组成构造块来创建复杂的小部件。这些接口，当被特定对象路径的总线对象使用时，决定了语言，小部件，文本和约束。这项服务，通过 API 和帮助类，允许简单地使用 AllJoyn 架构注册总线对象，执行特定小部件，控制小部件的细节，应用程序间信号同步和默认值／约束。

To implement a Control Panel, the simple and most common way
is to generate an XML file to represent the Control Panel.
This file is then passed into the Code Generator which will take
as input a header file that contains the developer methods to be
connected when widgets are interacted with. The Code Generator
will produce a generated folder that contains software that makes
use of the Control Panel Service APIs and classes to offer the
Contorl Panel.
配置控制面板，最简单的和常用的办法是生成一个代表控制面板的 XML 文件。这个文件被发给代码生成器。它将其当做包含开发者方法的头文件，并与和它发生互动的小部件相连接。代码生成器将建立一个生成文件夹包含了使用控制面板服务 API 的软件和提供控制面板的类。

Once an application discovers a Control Panel, the Control
Panel Service is used to perform AllJoyn Introspection to navigate
the AllJoyn BusObject tree to collect the details needed to render.
This information is passed into the UI Adaptive Layer for processing
to create a root UI element. The UI elements can then be traversed
and the native elements added to a display.
一旦有应用程序发现了一个控制面板，控制面板服务会使用 AllJoyn 自省机制帮助 AllJoyn 总线对象树进行导航，收集需要处理的信息。这些信息被传至 UI 自适应层，使进程能够生成一个根用户界面元素。随后用户界面元素就可以被传输，并且本地元素被加入显示。

When any UI widget is interacted with by a user it causes an
AllJoyn Bus Method call to be made back to the Controlee device.
This then changes the state of the UI accordingly, causes the
device code to execute for the given method, and and sends an
AllJoyn signal to maintain state with other connected applications.
The signal is built into the system and allows for sliders, spinners,
etc. to always be synchronized to avoid confusion when multiple
users are interacting with the same device.
当用户界面小部件与用户发生了互动，就会通过 AllJoyn Bus Method 告知受控设备。这会相应地改变用户界面的状态，使设备代码之行指定的方法，发送 AllJoyn 信号，以保持与其他连接的应用程序的状态。为了避免当多个用户与相同的设备进行交互时发生混淆，信号被植入系统，允许使用滑块和调节按钮等。

## Learn More 了解更多

* [Learn more about the Control Panel Interface Definition][controlpanel-interface]
* [Download the SDK][download], [build][build] and
  [run the sample apps][sample-apps]
* [Learn more about the APIs][api-guide]

[controlpanel-interface]: /learn/base-services/controlpanel/interface
[download]: https://allseenalliance.org/framework/download
[build]: /develop/building
[sample-apps]: /develop/run-sample-apps/controlpanel
[api-guide]: /develop/api-guide/controlpanel
[about]: /learn/core/about-announcement