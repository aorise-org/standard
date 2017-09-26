# 安卓开发规范(updating)

## 摘要

- [前言](#前言)
- [AS规范](#AS规范)
- [命名规范](#命名规范)
- [资源文件规范](#资源文件规范)
- [版本统一规范](#版本统一规范)
- [第三方库规范](#第三方库规范)
- [注释规范](#注释规范)
- [测试规范](#测试规范)
- [Ionic规范](#Ionic规范)
- [其他规范](#其他规范)


## 前言

为了利于项目维护以及规范开发，促进成员之间Code Review的效率，故提出以下开发规范，如有更好的建议，欢迎提出。   
本文档的预期读者包括：Android开发人员及测试人员。

## AS规范

工欲善其事，必先利其器。   
- 尽量使用最新版的IDE进行开发(暂定Android Studio2.3.1)；
- 编码格式统一为 **UTF-8**；
- 编辑完`.java`、 `.xml`等文件后一定要**格式化**（基本格式方面使用 AS 默认模板即可）；
- 删除多余的import，减少警告出现，可利用AS的`Optimize Imports`(Settings → Keymap → Optimize Imports)快捷键；

## 命名规范

代码中的命名严禁使用拼音与英文混合的方式,更不允许直接使用中文的方式。正确的英文拼写和语法可以让阅读者易于理解,避免歧义。

> 注意：即使纯拼音命名方式也要避免采用。但`alibaba`、`taobao`、`youku`、`hangzhou`等国际通用的名称,可视同英文。

### 包名

包名都以`域名反写+项目名称+模块名称`风格编写。  

包名全部小写，连续的单词只是简单地连接起来，不使用下划线，采用反域名命名规则，全部使用小写字母。一级包名是顶级域名，通常为`com`,`edu`,`gov`,`net`,`org`,`cn`等，二级包名为公司名，三级包名根据应用进行命名，后面就是对包名的划分了。  

例如，奥昇平台项目的Config模块包名 `cn.aorise.platform.config`.

关于包名的划分，其结构如下所示，很值得学习，参考Google I/O 2015的代码结构，按功能分包具体可以这样做：

```
src
└─com
    └─domain
        └─app
            │  AppApplication.java  定义Application类
            │  Config.java          定义配置数据（常量）
            │
            ├─framework
            │      定义interface以及相关基类
            │
            ├─io
            │      数据定义（model）、数据操作（比如json解析，但不包括db操作）
            │
            ├─model
            │      定义model（数据结构以及getter/setter、compareTo、equals等等，不含复杂操作）
            │      以及modelHelper（提供便于操作model的api）
            │
            ├─provider
            │      实现ContentProvider，及其依赖的db操作
            │
            ├─receiver
            │      实现Receiver
            │
            ├─service
            │      实现Service（比如IntentService），用于在独立线程中异步do stuff
            │
            ├─ui
            │      实现BaseActivity，以及自定义view和widget，相关的Adapter也放这里
            │
            ├─util
            │      实现工具类，提供静态方法
            │
            ├─feature1
            │      Item.java                定义model
            │      ItemHelper.java          实现modelHelper
            │      feature1Activity.java    定义UI
            │      feature1DAO.java         私有db操作
            │      feature1Utils.java       私有工具函数
            │      ...其它私有class
            │
            ├─...其它feature
```

### 类名

类名都以`UpperCamelCase`风格编写。

类名通常是名词或名词短语，接口名称有时可能是形容词或形容词短语。现在还没有特定的规则或行之有效的约定来命名注解类型。

名词，采用大驼峰命名法，尽量避免缩写，除非该缩写是众所周知的， 比如`HTML`, `URL`，如果类名称中包含单词缩写，则单词缩写的每个字母均应大写。

| 类                  | 描述                                       | 例如                                       |
| :----------------- | :--------------------------------------- | :--------------------------------------- |
| Activity 类         | Activity为后缀标识                            | 欢迎页面类 WelcomeActivity                     |
| Adapter类           | Adapter 为后缀标识                            | 新闻详情适配器 NewDetailAdapter                 |
| 解析类                | Parser为后缀标识                              | 首页解析类 HomePosterParser                    |
| 工具方法类              | Utils或Manager为后缀标识（与系统或第三方的Utils区分）或功能+Utils | 线程池管理类：ThreadPoolManager日志工具类：LogUtils（Logger也可）打印工具类：PrinterUtils |
| 数据库类               | 以DBHelper后缀标识                            | 新闻数据库：NewDBHelper                        |
| Service类           | 以Service为后缀标识                            | 时间服务 TimeService                          |
| BroadcastReceiver类 | 以Receiver为后缀标识                           | 推送接收 JPushReceiver                        |
| ContentProvider类   | 以Provider为后缀标识                           | ShareProvider                            |
| 自定义的共享基础类          | 以Base开头                                  | BaseActivity,BaseFragment                |

测试类的命名以它要测试的类的名称开始，以Test结束。例如：`HashTest`或`HashIntegrationTest`。

~~接口（interface）：命名规则与类一样采用大驼峰命名法，多以able或ible结尾，如
`interface Runnable`、`interface Accessible`。~~

接口（interface）：命名规则与类一样采用大驼峰命名法，接口都以I为前缀，不加后缀，如
`interface ICycle`、`interface IRequest`。

> 注意：如果项目采用MVP，所有Model、View、Presenter的接口都以I为前缀，不加后缀，其他的接口采用上述命名规则。


### 方法名

方法名都以`lowerCamelCase`风格编写。方法名通常是动词或动词短语。

| 方法                     | 说明                                   |
| :--------------------- | ------------------------------------ |
| initXX()               | 初始化相关方法,使用init为前缀标识，如初始化布局initView() |
| isXX() checkXX()       | 方法返回值为boolean型的请使用is或check为前缀标识      |
| getXX()                | 返回某个值的方法，使用get为前缀标识                  |
| setXX()                | 设置某个属性值                              |
| handleXX()/processXX() | 对数据进行处理的方法                           |
| displayXX()/showXX()   | 弹出提示框和提示信息，使用display/show为前缀标识       |
| updateXX()             | 更新数据                                 |
| saveXX()               | 保存数据                                 |
| resetXX()              | 重置数据                                 |
| clearXX()              | 清除数据                                 |
| removeXX()             | 移除数据或者视图等，如removeView();             |
| drawXX()               | 绘制数据或效果相关的，使用draw前缀标识                |

### 常量名

常量名命名模式为`CONSTANT_CASE`，全部字母大写，用下划线分隔单词。 
 
那，到底什么算是一个常量？  
每个常量都是一个静态`final`字段，但不是所有静态`final`字段都是常量。在决定一个字段是否是一个常量时，考虑它是否真的感觉像是一个常量。例如，如果任何一个该实例的观测状态是可变的，则它几乎肯定不会是一个常量。只是永远不打算改变对象一般是不够的，它要真的一直不变才能将它示为常量。

```java
// Constants
static final int NUMBER = 5;
static final ImmutableListNAMES = ImmutableList.of("Ed", "Ann");
static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
static final SomeMutableType[] EMPTY_ARRAY = {};
enum SomeEnum { ENUM_CONSTANT }

// Not constants
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final SetmutableCollection = new HashSet();
static final ImmutableSetmutableElements = ImmutableSet.of(mutable);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};
```


### 非常量字段名

非常量字段名以`lowerCamelCase`风格的基础上改造为如下风格：基本结构为`scopeVariableNameType`。

**scope：范围**

- 非公有，非静态字段命名以m开头
- 静态字段命名以s开头
- 公有非静态字段命名以p开头
- 公有静态字段（全局变量）命名以g开头

例子：

```java
public class MyClass {
      int mPackagePrivate;  
      private int mPrivate;  
      protected int mProtected;
      private static MyClass sSingleton;  
      public int pField;
      public static int gField;
}
```

使用1个字符前缀来表示作用范围，1个字符的前缀必须小写，前缀后面是由表意性强的一个单词或多个单词组成的名字，而且每个单词的首写字母大写，其它字母小写，这样保证了对变量名能够进行正确的断句。

**Type：类型**

考虑到Android中使用很多UI控件，为避免控件和普通成员变量混淆以及更好达意，所有用来表示控件的成员变量统一加上控件缩写作为后缀（文末附有缩写表）。

对于普通变量一般不添加类型后缀，如果统一添加类型后缀，请参考文末的缩写表[UI控件缩写表](#UI控件缩写表)。

用统一的量词通过在结尾处放置一个量词，就可创建更加统一的变量，它们更容易理解，也更容易搜索。

> 注意：如果项目中使用`ButterKnife`，则不添加m前缀，以`lowerCamelCase`风格命名。

例如，请使用`mCustomerStrFirst`和`mCustomerStrLast`，而不要使用`mFirstCustomerStr`和`mLastCustomerStr`。

| 量词列表  | 量词后缀说明      |
| ----- | ----------- |
| First | 一组变量中的第一个   |
| Last  | 一组变量中的最后一个  |
| Next  | 一组变量中的下一个变量 |
| Prev  | 一组变量中的上一个   |
| Cur   | 一组变量中的当前变量  |

说明：

集合添加如下后缀：List、Map、Set
数组添加如下后缀：Arr

> 注意：所有的实体类、VO（值对象）统一采用标准的lowerCamelCase风格编写，所有的DTO（数据传输对象）就按照接口文档中定义的字段名编写。


### 参数名

参数名以`lowerCamelCase`风格编写。参数应该避免用单个字符命名。


### 局部变量名

局部变量名以`lowerCamelCase`风格编写。  
  
比起其它类型的名称，局部变量名可以有更为宽松的缩写。虽然缩写更宽松，但还是要避免用单字符进行命名，除了临时变量和循环变量。   
即使局部变量是final和不可改变的，也不应该把它示为常量，自然也不能用常量的规则去命名它。


### 临时变量

临时变量通常被取名为`i`、`j`、`k`、`m`和`n`，它们一般用于整型；`c`、`d`、`e`，它们一般用于字符型。      
如：`for (int i = 0; i < len ; i++)`。


### 类型变量名

类型变量可用以下两种风格之一进行命名：  

- 单个的大写字母，后面可以跟一个数字(如：`E`, `T`, `X`, `T2`)
- 以类命名方式，后面加个大写的`T`(如：`RequestT`, `FooBarT`)


## 资源文件规范

### 资源布局文件（XML文件（layout布局文件））

全部小写，采用下划线命名法。

#### ContentView命名

必须以全部单词小写，单词间以下划线分割，使用名词或名词词组。   

所有Activity或Fragment的contentView必须与其类名对应，对应规则为：将所有字母都转为小写，将类型和功能调换（也就是后缀变前缀）。   

规则：`activity_描述.xml`    
例如：`activity_main.xml`

规则：`fragment_描述.xml`    
例如：`fragment_chat.xml`


#### Dialog命名

规则：`dialog_描述.xml`  
例如：`dialog_hint.xml`   


#### PopupWindow命名

规则：`ppw_描述.xml`  
例如：`ppw_info.xml`


#### 列表命名

规则：`list_描述.xml`   
例如：`list_city.xml`


#### 列表项命名

规则：`item_描述.xml`   
例如：`item_city.xml`


#### 包含项命名

规则：`项目名称缩写_模块_(位置)描述.xml`    
例如：`grid_activity_main_head.xml`、`grid_activity_main_bottom.xml`  

具体项目名字见附录[项目名字统计表](#项目名字统计表)


### 资源文件（图片drawable文件夹下）

全部小写，采用下划线命名法，加前缀区分。    

命名模式：可加后缀 `_small` 表示小图， `_big` 表示大图，逻辑名称可由多个单词加下划线组成，采用以下规则：      

- `用途_模块名_逻辑名称`
- `用途_模块名_颜色`
- `用途_逻辑名称`
- `用途_颜色`

说明：用途也指控件类型，具体见附录[UI控件缩写表](#UI控件缩写表)

例如：   

| 名称                      | 说明                      |
| ----------------------- | ----------------------- |
| btn_main_home.png       | 按键`用途_模块名_逻辑名称`         |
| divider_maket_white.png | 分割线`用途_模块名_颜色`          |
| ic_edit.png             | 图标`用途_逻辑名称`             |
| bg_main.png             | 背景`用途_逻辑名称`             |
| btn_red.png             | 红色按键`用途_颜色`             |
| btn_red_big.png         | 红色大按键`用途_颜色`            |
| ic_head_small.png       | 小头像`用途_逻辑名称`            |
| bg_input.png            | 输入框背景`用途_逻辑名称`          |
| divider_white.png       | 白色分割线`用途_颜色`            |
| bg_main_head            | 主模块头部背景图片`用途_模块名_逻辑名称`  |
| def_search_cell         | 默认搜索界面单元图片`用途_模块名_逻辑名称` |
| ic_more_help            | 更多帮助图标`用途_逻辑名称`         |
| divider_list_line       | 列表分割线`用途_逻辑名称`          |
| sel_search_ok           | 搜索界面确认选择器`用途_模块名_逻辑名称`  |
| shape_music_ring        | 音乐界面环形形状`用途_模块名_逻辑名称`   |

如果有多种形态，如按钮选择器：`sel_btn_xxx.xml`  

| 名称                   | 说明                           |
| -------------------- | ---------------------------- |
| sel_btn_xx           | 按钮图片使用`btn_整体效果`（selector）   |
| btn_xx_normal        | 按钮图片使用`btn_正常情况效果`           |
| btn_xx_pressed       | 按钮图片使用`btn_点击时候效果`           |
| btn_xx_focused       | `state_focused`聚焦效果          |
| btn_xx_disabled      | `state_enabled` (false)不可用效果 |
| btn_xx_checked       | `state_checked`选中效果          |
| btn_xx_selected      | `state_selected`选中效果         |
| btn_xx_hovered       | `state_hovered`悬停效果          |
| btn_xx_checkable     | `state_checkable`可选效果        |
| btn_xx_activated     | `state_activated`激活的         |
| btn_xx_windowfocused | `state_window_focused`       |

> 注意：使用AndroidStudio的插件`SelectorChapek`可以快速生成selector，前提是命名要规范。


### 动画文件（anim文件夹下）

全部小写，采用下划线命名法，加前缀区分。  

具体动画采用以下规则：`模块名_逻辑名称`。     
例如：`refresh_progress.xml`、`market_cart_add.xml`、`market_cart_remove.xml`。  

普通的tween动画采用如下表格中的命名方式：`动画类型_方向`   

| 名称                | 说明      |
| ----------------- | ------- |
| fade_in           | 淡入      |
| fade_out          | 淡出      |
| push_down_in      | 从下方推入   |
| push_down_out     | 从下方推出   |
| push_left         | 推向左方    |
| slide_in_from_top | 从头部滑动进入 |
| zoom_enter        | 变形进入    |
| slide_in          | 滑动进入    |
| shrink_to_middle  | 中间缩小    |


### values中name命名

#### colors.xml

`colors.xml`的`name`命名使用下划线命名法，在你的`colors.xml`文件中应该只是映射颜色的名称一个ARGB值，而没有其它的。不要使用它为不同的按钮来定义ARGB值。   

**不要这样做**

```xml
  <resources>
      <color name="button_foreground">#FFFFFF</color>
      <color name="button_background">#2A91BD</color>
      <color name="comment_background_inactive">#5F5F5F</color>
      <color name="comment_background_active">#939393</color>
      <color name="comment_foreground">#FFFFFF</color>
      <color name="comment_foreground_important">#FF9D2F</color>
      ...
      <color name="comment_shadow">#323232</color>
```

使用这种格式，你会非常容易的开始重复定义ARGB值，这使如果需要改变基本色变的很复杂。同时，这些定义是跟一些环境关联起来的，如`button`或者`comment`, 应该放到一个按钮风格中，而不是在`colors.xml`文件中。   

**相反，这样做**

```xml
  <resources>

      <!-- grayscale -->
      <color name="white"     >#FFFFFF</color>
      <color name="gray_light">#DBDBDB</color>
      <color name="gray"      >#939393</color>
      <color name="gray_dark" >#5F5F5F</color>
      <color name="black"     >#323232</color>

      <!-- basic colors -->
      <color name="green">#27D34D</color>
      <color name="blue">#2A91BD</color>
      <color name="orange">#FF9D2F</color>
      <color name="red">#FF432F</color>

  </resources>
```

向应用设计者那里要这个调色板，名称不需要跟`"green"`、`"blue"`等等相同。`"brand_primary"`、`"brand_secondary"`、`"brand_negative"`这样的名字也是完全可以接受的。 像这样规范的颜色很容易修改或重构，会使应用一共使用了多少种不同的颜色变得非常清晰。 通常一个具有审美价值的UI来说，减少使用颜色的种类是非常重要的。

> 注意：如果某些颜色和主题有关，那就单独写一个`colors_theme.xml`。


#### dimens.xml

像对待`colors.xml`一样对待`dimens.xml`文件 与定义颜色调色板一样，你同时也应该定义一个空隙间隔和字体大小的“调色板”。 一个好的例子，如下所示：  

```xml
<resources>

    <!-- font sizes -->
    <dimen name="aorise_font_size_micro">12sp</dimen>
    <dimen name="aorise_font_size_small">14sp</dimen>
    <dimen name="aorise_font_size_middle">16sp</dimen>
    <dimen name="aorise_font_size_medium">18sp</dimen>
    <dimen name="aorise_font_size_large">22sp</dimen>
    <dimen name="aorise_font_size_xlarge">24sp</dimen>
    <dimen name="aorise_font_size_huge">36sp</dimen>

    <!-- typical spacing between two views -->
    <dimen name="aorise_margin_huge">48dp</dimen>
    <dimen name="aorise_margin_big">24dp</dimen>
    <dimen name="aorise_margin_middle">16dp</dimen>
    <dimen name="aorise_margin_normal">8dp</dimen>
    <dimen name="aorise_margin_micro">4dp</dimen>
    <dimen name="aorise_margin_nano">2dp</dimen>

    <dimen name="aorise_padding_huge">48dp</dimen>
    <dimen name="aorise_padding_big">24dp</dimen>
    <dimen name="aorise_padding_middle">16dp</dimen>
    <dimen name="aorise_padding_normal">8dp</dimen>
    <dimen name="aorise_padding_micro">4dp</dimen>
    <dimen name="aorise_padding_nano">2dp</dimen>

    <!-- typical sizes of views -->
    <dimen name="aorise_width_huge">240dp</dimen>
    <dimen name="aorise_width_xlarge">180dp</dimen>
    <dimen name="aorise_width_large">120dp</dimen>
    <dimen name="aorise_width_big">60dp</dimen>
    <dimen name="aorise_width_middle">48dp</dimen>
    <dimen name="aorise_width_normal">32dp</dimen>
    <dimen name="aorise_width_small">16dp</dimen>

    <dimen name="aorise_height_huge">240dp</dimen>
    <dimen name="aorise_height_xlarge">180dp</dimen>
    <dimen name="aorise_height_large">120dp</dimen>
    <dimen name="aorise_height_big">60dp</dimen>
    <dimen name="aorise_height_middle">48dp</dimen>
    <dimen name="aorise_height_normal">32dp</dimen>
    <dimen name="aorise_height_small">16dp</dimen>

</resources>

```

布局时在写`margins`和`paddings`时，你应该使用`aorise_xx`尺寸格式来布局，而不是像对待`string`字符串一样直接写值，像这样规范的尺寸很容易修改或重构，会使应用所有用到的尺寸一目了然。 这样写会非常有感觉，会使组织和改变风格或布局是非常容易。  


#### strings.xml

`strings`的`name`命名使用下划线命名法，采用以下规则：`模块名(Activity模块)+逻辑名称`，这样方便同一个界面的所有string都放到一起，方便查找。  

| 名称                | 说明      |
| ----------------- | ------- |
| main_menu_about   | 主菜单按键文字 |
| friend_title      | 好友模块标题栏 |
| friend_dialog_del | 好友删除提示  |
| login_check_email | 登录验证    |
| dialog_title      | 弹出框标题   |
| button_ok         | 确认键     |
| loading           | 加载文字    |


#### styles.xml

`style`的`name`命名使用大驼峰命名法，几乎每个项目都需要适当的使用`style`文件，因为对于一个视图来说有一个重复的外观是很常见的，将所有的外观细节属性（`colors`、`padding`、`font`）放在`style`文件中。 在应用中对于大多数文本内容，最起码你应该有一个通用的`style`文件，例如：   

```
<style name="ContentText">
    <item name="android:textSize">@dimen/font_normal</item>
    <item name="android:textColor">@color/basic_black</item>
</style>
```

应用到`TextView`中:   

```
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/price"
    style="@style/ContentText" />
```

你或许需要为按钮控件做同样的事情，不要停止在那里。将一组相关的和重复`android:****`的属性放到一个通用的`style`中。  

**将一个大的`styles.xml`文件分割成多个文件**， 你可以有多个`styles.xml` 文件。    
Android SDK支持其它文件，`styles.xml`这个文件名称并没有作用，起作用的是在文件里的`<style>`标签。因此你可以有多个style文件，如`styles.xml`、`styles_home.xml`、`styles_item_details.xml`、`styles_forms.xml`。   

不同于资源文件路径需要为系统构建起的有意义，在`res/values`目录下的文件可以任意命名。  


### layout中的id命名

命名模式为：`view缩写_模块名_逻辑名`，比如`btn_main_search` `txt_header_title`
使用`AndroidStudio`的插件`ButterKnife Zelezny`，生成注解非常方便，原生的话可以使用`Android Code Generator`插件。


## 版本统一规范

版本统一规范在项目根目录：`config/config.gradle`   

Android开发存在着众多版本的不同，比如`compileSdkVersion`、`minSdkVersion`、`targetSdkVersion`以及项目中依赖第三方库的版本，不同的module及不同的开发人员都有不同的版本，所以需要一个统一版本规范的文件。


## 第三方库规范

希望Team能用时下较新的技术，对开源库的选取，一般都需要选择比较稳定的版本，作者在维护的项目，要考虑作者对issue的解决，以及开发者的知名度等各方面。选取之后，一定的封装是必要的。

个人推荐Team可使用如下优秀轮子：  

* **[Retrofit][Retrofit]**
* **[RxAndroid][RxAndroid]**
* **[OkHttp][OkHttp]**
* **[Glide][Glide]** / **[Fresco][Fresco]**
* **[Gson][Gson]** / **[Fastjson][Fastjson]**
* **[EventBus][EventBus]** 
* **[GreenDao][GreenDao]**
* **[Dagger2][Dagger2]** (选用)


## 注释规范

为了减少他人阅读你代码的痛苦值，请在关键地方做好注释。

### 类注释

每个类完成后应该有作者姓名和联系方式的注释，对自己的代码负责。

```java
/**
 * <pre>
 *     author : Aorise
 *     e-mail : developer@aorise.org
 *     time   : 2017/03/07
 *     desc   : 文件描述
 *     version: 1.0
 * </pre>
 */
public class WelcomeActivity {
       ...
}
```

具体可以在AS中自己配制，File → Settings → Editor → File and Code Templates → Includes → File Header，输入

```java
/**
 * <pre>
 *     author : ${USER}
 *     e-mail : xxx@xx
 *     time   : ${YEAR}/${MONTH}/${DAY}
 *     desc   :
 *     version: 1.0
 * </pre>
 */
```

这样便可在每次新建类的时候自动加上该头注释。


### 方法注释

每一个成员方法（包括自定义成员方法、覆盖方法、属性方法）的方法头都必须做方法头注释，在方法前一行输入 `/** + 回车` 或者设置 `Fix doc comment` (Settings → Keymap → Fix doc comment)快捷键，AS便会帮你生成模板，我们只需要补全参数即可，如下所示。

```java
/**
 * bitmap转byteArr
 *
 * @param bitmap bitmap对象
 * @param format 格式
 * @return 字节数组
 */
public static byte[] bitmap2Bytes(Bitmap bitmap, CompressFormat format) {
    if (bitmap == null) return null;
    ByteArrayOutputStream baos = new ByteArrayOutputStream();
    bitmap.compress(format, 100, baos);
    return baos.toByteArray();
}
```

### 块注释

块注释与其周围的代码在同一缩进级别。它们可以是`/* ... */`风格，也可以是`// ...`风格(**`//`后最好带一个空格**）。    
对于多行的`/* ... */`注释，后续行必须从`*`开始， 并且与前一行的`*`对齐。以下示例注释都是OK的。

```java
/*
 * This is          // And so           /* Or you can
 * okay.            // is this.          * even do this. */
 */
```

注释不要封闭在由星号或其它字符绘制的框架里。

> Tip：在写多行注释时，如果你希望在必要时能重新换行(即注释像段落风格一样)，那么使用`/* ... */`。

### 其他一些注释

AS已帮你集成了一些注释模板，我们只需要直接使用即可，在代码中输入`todo`、`fixme`等这些注释模板，回车后便会出现如下注释

``` java
// TODO: 17/3/14 需要实现，但目前还未实现的功能的说明
// FIXME: 17/3/14 需要修正，甚至代码是错误的，不能工作，需要修复的说明
```


## 测试规范

业务开发完成之后，开发人员做单元测试，单元测试完成之后，保证单元测试全部通过同时单元测试代码覆盖率达到一定程度（这个需要开发和测试约定，理论上越高越好），开发提测。

### Lint

- Correctness：不够完美的编码，比如硬编码、使用过时 API 等
- Performance：对性能有影响的编码，比如：静态引用，循环引用等
- Internationalization：国际化，直接使用汉字，没有使用资源引用等
- Security：不安全的编码，比如在 WebView 中允许使用 JavaScriptInterface 等
- Security：不安全的编码，比如在 WebView 中允许使用 JavaScriptInterface 等


### Findbugs

- Bad practice: 坏的实践
- Malicious code vulnerability: 可能受到的恶意攻击
- Correctness: 一般的正确性问题
- Dodgy: 危险的，具有潜在危险的代码，可能运行期产生错误
- Performance: 性能问题
- Multithreaded correctness: 多线程的正确性多线程编程时，可能导致错误的代码
- Internationalization: 国际化

### SonarQube



## Ionic规范

// TODO...


## 其他规范

- 合理布局，有效运用`<merge>`、`<ViewStub>`、`<include>`标签；

- `Activity`和`Fragment`里面有许多重复的操作以及操作步骤，所以我们都需要提供一个`BaseActivity`和`BaseFragment`，让所有的`Activity`和`Fragment`都继承这个基类。

- 启动`Activity`的话建议使用AS自带的模板，输入`starter`即可，如下所示：

  ``` java
  public static void start(Context context, String data) {
      Intent starter = new Intent(context, MainActivity.class);
      starter.putExtra("data", data);
      context.startActivity(starter);
  }
  ```

  同理，启动`Fragment`输入`newInstance`即可，如下所示：

  ``` java
  public static MainFragment newInstance(String data) {
      Bundle args = new Bundle();
      args.putString("data", data);
      MainFragment fragment = new MainFragment();
      fragment.setArguments(args);
      return fragment;
  }
  ```

- 方法基本上都按照调用的先后顺序在各自区块中排列；

- 相关功能作为小区块放在一起（或者封装掉）；

- 当一个类有多个构造函数，或是多个同名方法，这些函数/方法应该按顺序出现在一起，中间不要放进其它函数/方法；

- 数据提供统一的入口。无论是在 MVP、MVC 还是 MVVM 中，提供一个统一的数据入口，都可以让代码变得更加易于维护。比如可使用一个`DataManager`，把 `http`、`preference`、`eventpost`、`database` 都放在`DataManger`里面进行操作，我们只需要与`DataManger`打交道；

- 多用组合, 少用继承；

- 提取方法, 去除重复代码。对于必要的工具类抽取也很重要，这在以后的项目中是可以重用的。

- 可引入 Dagger2 减少模块之间的耦合性。Dagger2 是一个依赖注入框架，使用代码自动生成创建依赖关系需要的代码。减少很多模板化的代码，更易于测试，降低耦合，创建可复用可互换的模块；

- 项目引入`RxJava` + `RxAndroid`这些响应式编程，可以极大的减少逻辑代码；

- 通过引入事件总线，如：`EventBus`、`AndroidEventBus`、`RxBus`，它允许我们在`DataLayer`中发送事件，以便`ViewLayer`中的多个组件都能够订阅到这些事件，减少回调；

- 尽可能使用局部变量；

- 及时关闭流；

- 尽量减少对变量的重复计算；

  如下面的操作：

  ``` java
  for (int i = 0; i < list.size(); i++) {
      ...
  }
  ```

  建议替换为：

  ``` java
  for (int i = 0, len = list.size(); i < len; i++) {
      ...
  }
  ```

- 尽量采用懒加载的策略，即在需要的时候才创建；

  例如：

  ``` java
  String str = "aaa";
  if (i == 1) {
      list.add(str);
  }
  ```

  建议替换为：

  ``` java
  if (i == 1) {
      String str = "aaa";
      list.add(str);
  }
  ```

- 不要在循环中使用try…catch…，应该把其放在最外层；

- 使用带缓冲的输入输出流进行IO操作；

- 尽量使用HashMap、ArrayList、StringBuilder，除非线程安全需要，否则不推荐使用Hashtable、Vector、StringBuffer，后三者由于使用同步机制而导致了性能开销；

- 尽量在合适的场合使用单例；   
  使用单例可以减轻加载的负担、缩短加载的时间、提高加载的效率，但并不是所有地方都适用于单例，简单来说，单例主要适用于以下三个方面：
  - 控制资源的使用，通过线程同步来控制资源的并发访问
  - 控制实例的产生，以达到节约资源的目的
  - 控制数据的共享，在不建立直接关联的条件下，让多个不相关的进程或线程之间实现通信

- 把一个基本数据类型转为字符串，`基本数据类型.toString()`是最快的方式、`String.valueOf(数据)`次之、`数据 + ""`最慢；

- 使用AS自带的Lint来优化代码结构（什么，你不会？右键module、目录或者文件，选择Analyze → Inspect Code）；

- 最后不要忘了内存泄漏的检测；

---

最后啰嗦几句：

- 好的命名规则能够提高代码质量，使得新人加入项目的时候降低理解代码的难度；
- 规矩终究是死的，适合团队的才是最好的；
- 命名规范需要团队一起齐心协力来维护执行，在团队生活里，谁都不可能独善其身；
- 一开始可能会有些不习惯，持之以恒，总会成功的。

## 附录

### UI控件缩写表

| 名称             | 缩写   |
| -------------- | ---- |
| TextView       | tv   |
| EditText       | et   |
| ImageButton    | ib   |
| Button         | btn  |
| ImageView      | iv   |
| ListView       | lv   |
| GridView       | gv   |
| ProgressBar    | pb   |
| SeekBar        | sb   |
| RadioButtion   | rb   |
| CheckBox       | cb   |
| ScrollView     | sv   |
| LinearLayout   | ll   |
| FrameLayout    | fl   |
| RelativeLayout | rl   |
| RecyclerView   | rv   |
| WebView        | wv   |
| VideoView      | vv   |
| Spinner        | spn  |
| ToggleButton   | tb   |

### 常见的英文单词缩写表

| 名称                   | 缩写                                       |
| -------------------- | ---------------------------------------- |
| icon                 | ic （主要用在app的图标）                          |
| color                | cl（主要用于颜色值）                              |
| average              | avg                                      |
| background           | bg（主要用于布局和子布局的背景）                        |
| selector             | sel主要用于某一view多种状态，不仅包括Listview中的selector，还包括按钮的selector） |
| buffer               | buf                                      |
| control              | ctrl                                     |
| default              | def                                      |
| delete               | del                                      |
| document             | doc                                      |
| error                | err                                      |
| escape               | esc                                      |
| increment            | inc                                      |
| infomation           | info                                     |
| initial              | init                                     |
| image                | img                                      |
| Internationalization | I18N                                     |
| length               | len                                      |
| library              | lib                                      |
| message              | msg                                      |
| password             | pwd                                      |
| position             | pos                                      |
| server               | srv                                      |
| string               | str                                      |
| temp                 | tmp                                      |
| window               | wnd(win)                                 |

程序中使用单词缩写原则：不要用缩写，除非该缩写是约定俗成的。

    
### 项目名字统计表

|   项目名字CN    |  项目名字EN   |        项目包名         |
| --------------- | ------------- | ----------------------- |
| 智慧城市        | platform      | cn.aorise.platform      |
| 样例            | sample        | cn.aorise.sample        |
| 互联网+民生服务 | live          | cn.aorise.live          |
| 综治网格化      | grid          | cn.aorise.grid          |
| 数字化城市管理  | city          | cn.aorise.city          |
| 信访            | petition      | cn.aorise.petition      |
| 信访员工版      | petitionStaff | cn.aorise.petitionStaff |
| 教育            | education     | cn.aorise.education     |
| 医疗            | hospital      | cn.aorise.hospital      |
| 交通            | traffic       | cn.aorise.traffic       |
| 扶贫            | aid           | cn.aorise.aid           |



注：项目名字中`aorise` `common` `component`等均为保留字段，请后期添加项目的时候不要使用。另外添加项目名字请使用英文名字，并且考虑到资源文件均需要增加项目名字做前缀，请尽量控制英文名字短小。


## 参考
[Android包命名规范][Android包命名规范]

[Android 开发最佳实践][Android 开发最佳实践]

[Android 编码规范][Android 编码规范]

[阿里巴巴Java开发手册][阿里巴巴Java开发手册]

[Google Java编程风格指南][Google Java编程风格指南]

[小细节，大用途，35 个 Java 代码性能优化总结！][小细节，大用途，35 个 Java 代码性能优化总结！]


## 版本日志

- 17/09/08: 发布整理后的初稿；


　　　　　　　　　　　　　　　——powered by jianye.tang

> 本文档由@Blankj创建，由@jianye.tang整理和优化。

[Package by features, not layers]: https://medium.com/@cesarmcferreira/package-by-features-not-layers-2d076df1964d#.mp782izhh
[iosched]: https://github.com/google/iosched/tree/master/android/src/main/java/com/google/samples/apps/iosched
[安卓开发规范(updating)]: https://github.com/Blankj/AndroidStandardDevelop
[Android开发者工具]: http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2017/0526/7973.html
[Android Studio下对资源进行分包]: http://www.jianshu.com/p/8e893581b9c7
[Android开发之版本统一规范]: http://www.jianshu.com/p/db6ef4cfa5d1
[Android 流行框架查速表]: http://www.ctolib.com/cheatsheets-Android-ch.html
[Android开发人员不得不收集的代码]: https://github.com/Blankj/AndroidUtilCode
[Retrofit]: https://github.com/square/retrofit
[RxAndroid]: https://github.com/ReactiveX/RxAndroid
[OkHttp]: https://github.com/square/okhttp
[Glide]: https://github.com/bumptech/glide
[Fresco]: https://github.com/facebook/fresco
[Gson]: https://github.com/google/gson
[Fastjson]: https://github.com/alibaba/fastjson
[EventBus]: https://github.com/greenrobot/EventBus
[AndroidEventBus]: https://github.com/bboyfeiyu/AndroidEventBus
[GreenDao]: https://github.com/greenrobot/greenDAO
[Dagger2]: https://github.com/google/dagger
[Tinker]: https://github.com/Tencent/tinker
[Android包命名规范]: http://www.ayqy.net/blog/android%E5%8C%85%E5%91%BD%E5%90%8D%E8%A7%84%E8%8C%83/
[Android 开发最佳实践]: https://github.com/futurice/android-best-practices/blob/master/translations/Chinese/README.cn.md
[Android 编码规范]: http://www.jianshu.com/p/0a984f999592
[阿里巴巴Java开发手册]: https://102.alibaba.com/newsInfo.htm?newsId=6
[Google Java编程风格指南]: http://www.hawstein.com/posts/google-java-style.html
[小细节，大用途，35 个 Java 代码性能优化总结！]: http://www.jianshu.com/p/436943216526
