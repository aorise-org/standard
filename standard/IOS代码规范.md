# IOS代码规范

## 前言

**为了利于项目维护以及规范开发，促进成员之间`Code Review`的效率，故提出以下开发规范，如有更好的建议，欢迎提出。**
本文档的预期读者包括：iOS开发人员。

## 命名规范

>代码中的命名严禁使用拼音与英文混合的方式,更不允许直接使用中文的方式。正确的英文拼写和语法可以让阅读者易于理解,避免歧义。

**`*注意：即使纯拼音命名方式也要避免采用。但alibab、taobao、youku、hangzhou等国际通用的名称，可视同英文。`**
```
大驼峰规则：每个单词的首字母大写。例：NameTextField。
小驼峰原则：第一个单词首字母小写，其余都大写。例：nameTextField。
```

### 项目命名

项目名都遵循大驼峰命名。例如：`AoriseProject`。

### Bundle Identifier 命名

**`Bundle Identifier`**：采用反域名命名规范，全部采用小写字母，以域名后缀+公司顶级域名+应用名形式命名，例如：`cn.aorise.sample`。**详细请见最后附表**。

### 类名

类的命名都遵循大驼峰命名。一般是：前缀 + 功能 + 类型。例如：`MW + Login + ViewController`。
```
在实际开发中，一般都会给工程中所有的类加上属于本工程的前缀。
```
常用控件类命名类型对照表（下表中前缀为：`MW`，如果用到下表中没有列举出来，请去掉`UI首字母`，遵循实际规则即可。）

|        控件名         |      类型      |         示例          |
| --------------------- | -------------- | --------------------- |
| UIViewController      | ViewController | MWBaseViewController  |
| UView                 | View           | MWBaseView            |
| UITableView           | TableView      | MWOrderTableView      |
| **`UITableViewCell`** | **`Cell`**     | **`MWOrderListCell`** |
| **` UIButton `**      | **`Btn`**      | **`MWSuccessBtn`**    |
| UILabel               | Label          | MWSuccessLabel        |
| **`UIImageView`**     | **`ImgView`**  | **`MWGoodsImgView`**  |
| UITextField           | TextField      | MWNameTextField       |
| UITextView            | TextView       | MWSuggestTextView     |

其它类相关对照表

|    功能    |                                   类型                                    |                   示例                   |
| ---------- | ------------------------------------------------------------------------- | ---------------------------------------- |
| 工具类     | Tool                                                                      | MWOrderTool                              |
| 代理类     | Delegate                                                                  | MWOrderListDelegate                      |
| 管理类     | Manager                                                                   | MWOrderListModel                         |
| 模型类     | Model                                                                     | MWOrderListModel                         |
| Service类  | Service                                                                   | MWOrderService                           |
| 布局类     | Layout                                                                    | MWHomeLayout                             |
| `数据库类` | ` DataBase、表名+DBHelper `                                               | `MWFriendDataBase、MWUserTableDBHelper`  |
| `类目`     | `XXX+（范围，例如Extension， Additions 或者功能，例如Frame，Nib，Block）` | `MWUIButton+Additions、MWUIButton+Block` |

### **`UIViewController`**请按照如下分类

```
#pragma mark - life cycle
#pragma mark - private
#pragma mark - public
#pragma mark - event response
#pragma mark - UITableViewDelegate && UITableViewDataSource
（代理顺序往下排列）
#pragma mark - getters and setters
```
>**`注意：所有视图或者对象的创建请尽量使用懒加载，调用的时候全部使用self.textBtn这样的方式。如果是确定的不可变数组、字典，可直接给定数组中的元素。(getters and setters分类中，懒加载可出现_调用对象，其它情况请遵循self.调用原则)`**
```
#import "ViewController.h"

@interface ViewController ()
@property (nonatomic, strong)UIButton *textBtn;
@end

@implementation ViewController

#pragma mark - life cycle
- (void)viewDidLoad {
[super viewDidLoad];
[self.view addSubview:self.textBtn];
}

- (void)didReceiveMemoryWarning {
[super didReceiveMemoryWarning];
// Dispose of any resources that can be recreated.
}
#pragma mark - private

#pragma mark - public

#pragma mark - event response

#pragma mark - UITableViewDelegate && UITableViewDataSource
//（代理顺序往下排列）

#pragma mark - CTAPIManagerCallBackDelegate

#pragma mark - getters and setters
- (UIButton *)textBtn {
if (_textBtn == nil) {
_textBtn = [UIButton buttonWithType:UIButtonTypeCustom];
_textBtn.frame = CGRectMake(300, 250, 100, 100);
_textBtn.backgroundColor = [UIColor yellowColor];
_textBtn.titleLabel.text = @"text";
[_textBtn addTarget:self action:@selector(text) forControlEvents:UIControlEventTouchUpInside];
}
return _textBtn;
}
@end
```
### 变量和方法

变量和方法的命名都遵循小驼峰命名。例如：`textVariableStr`, `- (void)textAction`响应事件。
变量命名对照表（如果用到下表中没有列举出来，请去掉`UI`、`NS`遵循实际规则即可。或者一看就知道的通用简写）
方法命名对照表（方法多为动词或动名词）

|          功能          |                             示例                              |
| ---------------------- | ------------------------------------------------------------- |
| - (id)initXX           | 初始化相关方法,使用init为前缀标识，如初始化布局- (id)initView |
| - (BOOL)isXX           | 方法返回值为boolean型的请使用is前缀标识                       |
| - (UIView *)getXX      | 返回某个值的方法，使用get为前缀标识                           |
| - (void)setXX          | 设置某个属性值或者相关数据                                    |
| - (void)updateXX       | 更新数据                                                      |
| - (void)saveXX         | 保存数据                                                      |
| - (void)drawXX         | 绘制相关，使用draw前缀标识                                    |
| - (void)clearXX        | 清除数据                                                      |
| `- (void)XXXAction  `  | `响应事件，使用Action为后缀标识`                              |
| `- (void)loadData`     | `加载数据（一般情况下VC中都会有这个方法）`                    |
| `- (void)loadMoreData` | `加载更多数据`                                                |
| `- (void)setupUI`      | `加载布局（一般情况下VC中都会有这个方法）`                    |

### 常量

宏：小写k+大驼峰  即为：`#define kUserAgeKey @“ageKey”`
全局常量：工程前+缀全大写，下划线隔开 即为：**`extern const NSString MW_USER_AGE_KEY`**

### 参数名

参数名以小驼峰命名，尽量参考苹果原生方法风格编写。尽量可读性好，看到方法名就知道这个方法是用来干什么的。参数应该避免用单个字符命名。例：`- (void)setDataImageUrl:(NSString *)imageUrl name:(NSString *)nameStr content:(NSString *)contentStr`

## 资源文件规范

### 资源文件命名

全部小写，采用下划线命名法，加前缀区分。**所有的资源文件都需要加上工程前缀（小写形式）**。
命名模式：可加后缀`_smal`l表示小图,`_big`表示大图，逻辑名称可由多个单词加下划线组成，采用以下规则：
`用途_模块名_逻辑名称`
`用途_模块名_颜色`
`用途_逻辑名称`
`用途_颜色`

|    说明    | 前缀（工程前缀示例MW） |                     示例                      |
| ---------- | ---------------------- | --------------------------------------------- |
| 按钮相关   | mw_btn_                | mw_btn_home_normal、mw_btn_red,mw_btn_red_big |
| 背景相关   | mw_btn_                | mw_bg_home_header、mw_bg_main                 |
| 图标相关   | mw_ic_                 | mw_ic_home_location、mw_bg_input              |
| 分割线相关 | mw_div_                | mw_ic_home_location、mw_bg_input              |
| 默认相关   | mw_def_                | mw_ic_home_location、mw_bg_input              |

### 文件夹命名

创建文件夹最好创建实体文件夹，找到工程目录，创建相应文件夹并拖入工程。文件夹命名使用相应模块结构分层的英文，首字母要大写。例：`Model`，`View`，`Controller`，`Tool`，`Other`，`Service`等等。

## 版本规范

采用A.B.C 三位数字命名，比如：1.0.2，当有版本更新的时候，依据下面的情况来确定版本号规范。

|  版本号   |        说明        |          示例          |
| --------- | ------------------ | ---------------------- |
| **A**.b.c | 属于重大更新内容   | **1**.0.2 -> **2**.00  |
| a.**B**.c | 属于小部分更新内容 | 1.**0**.2 -> 1.**2**.2 |
| a.b.**C** | 属于补丁更新内容   | 1.0.**2** -> 1.0.**4** |

## 第三方库规范

**希望Team能用时下较新的技术，对开源库的选取，一般都需要选择比较稳定的版本，作者在维护的项目，要考虑作者对issue的解决，以及开发者的知名度等各方面。选取之后，一定的封装是必要的。**

项目使用cocoapods统一管理开源第三库文件，不需要手动导入和手动添加依赖库。如果第三方不支持cocoapods，可手动导入工程。安装cocoapods请移步这里[cocoapods安装](http://blog.csdn.net/qtds8810/article/details/50510910)

## 注释规范

**为了减少他人阅读你代码的痛苦值，请在关键地方做好注释。**

### 类注释

```
//
//  MyViewController.m
//  text
//
//  Created by 林霞 on 2017/9/12.
//  Copyright © 2017年 林霞 1826692128@qq.com. All rights reserved.
//
```
该注释是自动生成的，在`xcode`中设置即可。`Created by` 电脑用户名` on` 创建该文件的时间。`Copyright 2017` 后面的名字和邮箱是自己填写和设置的。具体可在`xcode`工程，`Project Document`中设置。这样便可在每次新建类的时候自动加上该头注释。

#### 方法注释

>**方法注释，方法外部统一用`option + command + /`，方法内部统一用`//`注释。**
```
/**
测试
*/
- (void)text
{
//测试按钮事件响应
}
```

### 模型注释

每个`model`中的，包含的每个属性，都必须要写上相对应的注释，用`//`注释。阅读者一看这个model，就清楚知道`model`中的每个字段代表的意思，用来做什么事情的。

```
@interface DeliveryModel : NSObject
//提货劵所在商圈id
@property (nonatomic, assign) long long mallId;
//商圈全称
@property (nonatomic, copy) NSString *mallFullName;
//商圈简称
@property (nonatomic, copy) NSString *mallShortName;
//提货劵号
@property (nonatomic, copy) NSString *credentialsCode;
//总金额
@property (nonatomic, assign) NSInteger totalAmount;
//提货劵所在店铺id
@property (nonatomic, assign) long long storeId;
//货劵所在店铺名称
@property (nonatomic, copy) NSString *storeName;
//提货劵id
@property (nonatomic, strong) NSNumber *credentialsId;
//状态:0：未提货、1：已提货、2：已分享、3：已退款
@property (nonatomic, assign) NSInteger state;
//提货商品(以下为提货商品参数)
@property (nonatomic, strong) NSArray<DeliveryGoodslist *> *goodsList;
//二维码
@property (nonatomic, copy) NSString *qrCode;
//商品总个数
@property (nonatomic, assign) NSInteger goodsCount;
@end
```
>**`如果不是model的属性，是其它类属性，需要注释，请按照model属性注释方式。`**

## 编码规范

* **所有的方法之间空一行。**
* **所有的代码块之间空一行，删除多余的注释。**
* **所有自定义的方法需要给出注释。并且所有的公开方法都要加上项目名字前缀小写。例如：+ (UIImage *)ed_imageNamedFromBundle:(NSString *)bundleName iconName:(NSString *)iconName 。**
* **尽量使用懒加载，在控制器分类时有提及和要求，其它自定义类按照控制器格式分类，没有的分类不写即可。**
* **代码后的’{‘不需要独占一行，包括方法之后，if，switch等。**
* **必须要统一的要求，属性的定义请按照下图property之后，空一格，括号之后空一格，写上类名，空一格之后跟上`*`和属性名。**

```
@property (nonatomic, strong) UITableView *tableView;
@property (nonatomic, strong) DeliveryModel *delivery;
@property (nonatomic, strong) DeliveryLookAdapter *lookAdapter;
@property (nonatomic, strong) DeliveryLookAPIManager *lookManager;
```
* 遵循一般代码规范，多模仿苹果API。
* 删除不用的代码。
* 如果有方法一直不会用到，请删除（除工具类）。
* 没有执行任何业务逻辑的方法，请删除或给予注释，删除多余的资源或文件，添加必要的注释。
* 比较大的代码块需要给出注释。

## 其它规范

* 建议项目统一使用Masonry和xib结合的方式布局。尽量不要直接设置frame。如果是纯代码的项目，不要建立xib文件和拉约束。不建议使用纯storyboard开发。
* 数据提供统一的入口。无论是在 MVP、MVC 还是 MVVM 中，提供一个统一的数据入口，都可以让代码变得更加易于维护。比如可使用一个DataManager，把 http、preference、eventpost、database 都放在DataManger里面进行操作，我们只需要与DataManger打交道
* 提取方法，去除重复代码。对于必要的工具类抽取也很重要，这在以后的项目中是可以重用的。
* 尽可能的使用局部变量
* 尽量减少对变量的重复计算。例如cell的高度尽量缓存起来，在用于加载。
* 尽量在合适的场合使用单例。使用单例可以减轻加载的负担，缩短加载的时间，提高加载效率。但并不是所有的地方都适用于单例，简单来说单例主要适用于以下三个方面：
1. 控制资源的使用，通过线程同步来控制资源的并发访问。
2. 控制实例的产生，以达到节约资源的目的。
3. 控制数据的共享，在不建立直接关联的条件下，让多个不相关的进程或线程之间实现通信。
* 最后不要忘了检测内存泄漏。可使用Instruments分析内存。

## 绪言

文章如果有什么问题，请阅读过的小伙伴告知，我会立即更正。
如果有什么意见或者建议，欢迎提出或者一块讨论。
希望可以一起写出更好的代码。

