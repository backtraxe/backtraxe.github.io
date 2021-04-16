# Android 开发教程


<!--more-->

## 文件

项目结构：

- `app > java > com.example.myfirstapp > MainActivity`：这是主 activity。它是应用的入口点。当您构建和运行应用时，系统会启动此 Activity 的实例并加载其布局。
- `app > res > layout > activity_main.xml`：此 XML 文件定义了 activity 界面 (UI) 的布局。
- `app > manifests > AndroidManifest.xml`：清单文件描述应用的基本特性并定义其每个组件。
- `Gradle Scripts > build.gradle`：有两个使用此名称的文件：一个针对项目“Project: My First App”，另一个针对应用模块“Module: app”。每个模块均有自己的 build.gradle 文件，但此项目当前仅有一个模块。使用每个模块的 build.gradle 文件控制 Gradle 插件构建应用的方式。

## 运行应用

### 在真实设备上运行

1. 手机 USB 连接到电脑。
1. 手机打开`开发者选项`。
    - 对于 MIUI，进入`设置 > 全部参数 > MIUI 版本`，连续点击七次，然后进入`设置 > 更多设置 > 开发者选项`中打开`USB 调试`和`USB安装`。
1. 在 Android Studio 中，从顶部的设备下拉菜单中选择你的手机。

### 在模拟器上运行

1. 在 Android Studio 中创建一个 Android 虚拟设备 (AVD)，模拟器可以使用该设备安装和运行您的应用。
1. 在 Android Studio 中，从顶部的设备下拉菜单中选择你的 AVD。

## 界面

### 构建简单的界面

Android 应用的界面 (UI) 以布局和微件的层次结构形式构建而成。布局是 ViewGroup 对象，即控制其子视图在屏幕上的放置方式的容器。微件是 View 对象，即按钮和文本框等界面组件。

Android 提供了 ViewGroup 和 View 类的 XML 词汇表，因此界面的大部分内容都在 XML 文件中定义。布局编辑器会在您拖放视图构建布局时为您编写 XML 代码。

**布局编辑器：**

1. 打开`app > res > layout > activity_main.xml`。
1. 如果您的编辑器显示 XML 源代码，请点击窗口右上角的`Design`。
1. 点击![](https://developer.android.com/studio/images/buttons/layout-editor-design.png)(Select Design Surface)，然后选择`Blueprint`。
1. 点击布局编辑器工具栏中的![](https://developer.android.com/studio/images/buttons/layout-editor-show-constraints.png)(View Options)，并确保选中`Show All Constraints`。
1. 确保 Autoconnect 处于关闭状态。当 Autoconnect 处于关闭状态时，工具栏中的提示会显示![](https://developer.android.com/studio/images/buttons/layout-editor-autoconnect-on.png)(Enable Autoconnection to Parent)。
1. 点击工具栏中的![](https://developer.android.com/studio/images/buttons/default-margins.png)(Default Margins)，然后选择`16`。
1. 点击工具栏中的![](https://developer.android.com/studio/images/buttons/layout-editor-device.png)(Device for Preview)，然后选择`5.5, 1440 × 2560, 560 dpi (Pixel XL)`。

左下方的 Component Tree 面板显示布局的视图层次结构。

`ConstraintLayout`是一种布局，它根据同级视图和父布局的约束条件定义每个视图的位置。这样一来，使用扁平视图层次结构既可以创建简单布局，又可以创建复杂布局。这种布局无需嵌套布局。

**添加文本框：**

1. 在 Component Tree 面板中点击 TextView，然后按 Delete 键。
1. 在 Palette 面板中，点击 Text 以显示可用的文本控件。
1. 将 Plain Text 拖动到设计编辑器中，并将其放在靠近布局顶部的位置。这是一个接受纯文本输入的 EditText 微件。
1. 点击并按住顶边上的锚点，将其向上拖动，直至其贴靠到布局顶部，然后将其释放。这是一个约束条件：它会将视图约束在已设置的默认外边距内。

**更改界面字符串：**

1. 打开`app > res > values > strings.xml`。这是一个字符串资源文件，您可在此文件中指定所有界面字符串。您可以利用该文件在一个位置管理所有界面字符串，使字符串的查找、更新和本地化变得更加容易。
1. 点击窗口顶部的 Open editor。此时将打开 Translations Editor，它提供了一个可以添加和修改默认字符串的简单界面。它还有助于让所有已翻译的字符串井然有序。
1.
