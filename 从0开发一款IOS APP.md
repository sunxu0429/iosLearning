## 常见APP类型及技术架构

![avatar](./image/app类型.png)

![avatar](./image/业务模块.png)

## 创建第一个Xcode工程

![avatar](./image/ios开发常用工具.png)


```js
#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    [self.view addSubview:({
        UILabel * label = [[UILabel alloc] init];
        label.text = @"hello world11";
        [label sizeToFit];
        label.center = CGPointMake(self.view.frame.size.width/2
                                   , self.view.frame.size.height/2);
        label;
        
    })];
}


@end
```

## ios中的mvc

![avatar](./image/mvc.png)

## 创建一个 UIView

UIView：

+ 最基础的视图类，管理屏幕上一定区域的内容展示
+ 作为各种视图类型的父类，提供基础的能力
+ 可以添加子视图（嵌套）

![subView](./image/subView.png)

### 布局

1. 设置大小、位置(frame)
2. addSubView

使用栈管理全部的子View

+ 位置重叠的展示最后入栈的（view2和view3有重叠，先展示view3）
+ 可以随时调整位置
+ 插入到指定位置

![avatar](./image/栈管理View.png)

添加一个距离屏幕左上角(100,100)，宽高为100的红色区块

```js
//
//  ViewController.m
//  SampleAPP
//
//

#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    UIView *view = [[UIView alloc] init];
    view.backgroundColor = [UIColor redColor];
    view.frame = CGRectMake(100, 100,100,100);
    [self.view addSubview:view];
}

@end
```

## UIView声生命周期

+ willMoveToSuperview
+ didMoveToSuperView
+ willMoveToWindow
+ didMoveToWindow

```js

@interface TestView : UIView

@end

@implementation TestView

- (instancetype)init{
    self = [super init];
    if(self){
        
    }
    return self;
}

- (void)willMoveToSuperview:(nullable UIView *)newSuperview{
    [super willMoveToSuperview:newSuperview];
}

- (void)didMoveToSuperview{
    [super didMoveToSuperview];
}

- (void)willMoveToWindow:(UIWindow *)newWindo{
    [super willMoveToWindow:newWindo];
}

- (void)didMoveToWindow{
    [super didMoveToWindow];
}

@end
```

## 通过UIViewController来管理视图

视图控制器，管理视图View层级结构。自身包含View(default view)，可以理解为一个容器

+ 管理View视图的生命周期
+ 响应用户操作(滚动)
+ 和App整体交互，视图的切换
+ 作为一个container管理多个Contriller和动画

### ViewController的生命周期

+ init
+ viewDidLoad
+ viewWillAppear
+ viewDidAppear
+ viewWillDisappear
+ viewDidDisappear
+ Dealloc

选择合适的函数处理不同的业务

所有和view相关的初始化工作都应该放到viewDidLoad中

## 实现你的第一个TabBar页面

使用UIView和UIViewController的特性搭建App

+ UIView负责页面内的内容展现
+ 使用基础的UIViewController管理多个UIView
+ UIViewController在管理UIView的同时，负责不同页面的切换

![avatar](./image/常用App页面结构分析.png)

### 单页面展示

+ 通过列表展示简介
+ 通过较长滚动页面展示内容

单个页面就是一个UIViewController，其中包含多个UIView视图

### 多页面管理

+ 4个或5个底部按钮
+ 通过Push的方式进行页面切换

多页面切换，需要两个或多个UIViewController来控制页面的切换

![avatar](./image/页面结构.png)

### UITabBarController

UITabBarController功能就是管理多个ViewController切换，通过点击底部对应按钮，选中对应需要展示的ViewController,国内App一般展示4-5个选项

![avatar](./image/UITabBarController.png)

### UITabBar

![avatar](./image/UITabBar.png)

### 实现自己的TabBar

+ 使用系统函数实现
+ 相关开源框架和项目

    开源Tabbar/TabBarController主要是做了简易的封装+5个按钮的样式

    1.完全自己实现

    2.TabBar上粘贴自定义的SubView，响应事件调用系统方法

**注意**：

iOS13中appdelegate的职责发现了改变：
iOS13之前，Appdelegate的职责全权处理App生命周期和UI生命周期；
iOS13之后，Appdelegate的职责是：
1、处理 App 生命周期
2、新的 Scene Session 生命周期
那UI的生命周期交给新增的Scene Delegate处理

在SceneDelegate.m文件中，添加如下代码，为屏幕下边添加TabBar

```objc
    UIWindowScene *windowScene = (UIWindowScene *)scene;
    self.window = [[UIWindow alloc] initWithWindowScene:windowScene];
    self.window.frame = windowScene.coordinateSpace.bounds;
    
    UITabBarController *tabbarController = [[UITabBarController alloc] init];
    
    UIViewController *controller1 = [[UIViewController alloc] init];
    controller1.view.backgroundColor = [UIColor redColor];
    controller1.tabBarItem.title = @"新闻";
    
    UIViewController *controller2 = [[UIViewController alloc] init];
    controller2.view.backgroundColor = [UIColor yellowColor];
    controller2.tabBarItem.title = @"视频";
    
    UIViewController *controller3 = [[UIViewController alloc] init];
    controller3.view.backgroundColor = [UIColor blueColor];
    controller3.tabBarItem.title = @"推荐";
    
    UIViewController *controller4 = [[UIViewController alloc] init];
    controller4.view.backgroundColor = [UIColor greenColor];
    controller4.tabBarItem.title = @"我的";
    
    // 将四个页面的 UIViewController 加入到 UITabBarController 之中
    [tabbarController setViewControllers: @[controller1, controller2, controller3, controller4]];
    
    self.window.rootViewController = tabbarController;
    [self.window makeKeyAndVisible];

```

## 使用UINavigationController管理页面切换

UINavigationController

+ 通过栈管理页面间的跳转
+ 通常只展示栈顶页面
+ Push/Pop操作

![avatar](./image/UINavigationController.png)

使用UINavigationController管理页面

![avatar](./image/UINavigationController管理页面.png)

### UINavigationBar

+ 由UINavigationController管理
+ 顶部UINavigationController变化，UINavigationBar则同步变化

![avatar](./images/../image/UINavigationBar.png)

### 实现自己的Navigation

SceneDelegate.m文件,注意UINavigationController的创建

```objc
    UIWindowScene *windowScene = (UIWindowScene *)scene;
    self.window = [[UIWindow alloc] initWithWindowScene:windowScene];
    self.window.frame = windowScene.coordinateSpace.bounds;
    
    UITabBarController *tabbarController = [[UITabBarController alloc] init];
    
    ViewController *viewController = [[ViewController alloc] init];
    
    //UINavigationController创建
    UINavigationController *navigationController = [[UINavigationController alloc] initWithRootViewController:viewController];
    
//UIViewController *controller1 = [[UIViewController alloc] init];
 //   controller1.view.backgroundColor = [UIColor redColor];
    navigationController.tabBarItem.title = @"新闻";
    
    UIViewController *controller2 = [[UIViewController alloc] init];
    controller2.view.backgroundColor = [UIColor yellowColor];
    controller2.tabBarItem.title = @"视频";
    
    UIViewController *controller3 = [[UIViewController alloc] init];
    controller3.view.backgroundColor = [UIColor blueColor];
    controller3.tabBarItem.title = @"推荐";
    
    UIViewController *controller4 = [[UIViewController alloc] init];
    controller4.view.backgroundColor = [UIColor greenColor];
    controller4.tabBarItem.title = @"我的";
    
    // 将四个页面的 UIViewController 加入到 UITabBarController 之中
    [tabbarController setViewControllers: @[navigationController, controller2, controller3, controller4]];
    
    self.window.rootViewController = tabbarController;
    [self.window makeKeyAndVisible];
```

在ViewController.m中，在viewDid钩子函数添加

```objc
 UITapGestureRecognizer *tapGesture = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(pushController)];
    [view addGestureRecognizer:tapGesture];
```

对应的pushController

```objc
- (void)pushController{
    
    UIViewController *viewController = [[UIViewController alloc] init];
    viewController.view.backgroundColor = [UIColor whiteColor];
    viewController.navigationItem.title = @"内容";
    [self.navigationController pushViewController:viewController animated:(YES)];
    
    
}
```

## APP中的窗口

![avatar](./image/APP窗口.png)

上图中的两个Controller都处理了一些逻辑。开发者提供Model，系统提供了View供交互，同时系统也提供了Controller做相应的处理。

### UIWindow

+ 特殊形式的UIView,提供App中展示内容的基础窗口
+ 只作为容器，和ViewController一起协同工作
+ 通常屏幕上只存在、展示一个UIWindow
+ 使用storyboard会帮助我们自动创建
+ 手动创建的步骤如下
    
    + 创建UIWindow
    + 设置rootViewController
    + makeKeyAndVisible

![avatar](./image/UIWindow.png)

### App推荐

![avatar](./image/App推荐.png)

![avatar](./image/方便常用.png)

## delegate设计模式

UITabBarController设计了一些规范的协议，让我们自己去实现

![avatar](./image/delegate.png)

shouldSelectViewController可以判断是否切换```ViewController```

切换完成会触发didSelectViewController

### 如何使用delegate

1.设置self为delegate的接收者，也就是实现方法的对象

```objc
tabBarController.delegate = self;
```

2.根据需求按需实现方法

```objc
- (BOOL)tabBarController:(UITabBarController *)tabBarController shouldSelectViewController:(UIViewController *)viewController {
    return YES;
}

- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController {
    //播放这个viewController的第一个视频

}
```

### delegate设计模式

![avatar](./image/delegate设计模式.png)

```objc
@interface SceneDelegate ()<UITabBarControllerDelegate>
...

tabbarController.delegate = self;

....

- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController {
    NSLog(@"进didSelectViewController");
};

```

## 使用UITableView实现简单的列表

列表：

+ 数据量大
+ 样式较为统一
+ 通常需要分组
+ 垂直滚动
+ 通常可视区域只有一个(视图的复用)

![列表](./image/列表.png)

### UITableView

![UITableView](./image/UITableView.png)

### UITableViewDataSource

UITableView作为视图，只负责展示，协助管理，不管理数据。需要开发者为UITableView提供展示所需要的数据及Cell。通过delegate模式，开发者需要实现UITableViewDataSource


```objc
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;

// Row display. Implementers should *always* try to reuse cells by setting each cell's reuseIdentifier and querying for available reusable cells with dequeueReusableCellWithIdentifier:
// Cell gets various attributes set automatically based on table (separators) and data source (accessory views, editing controls)

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath;
```

UITableViewCell默认提供的样式

![avatar](./image/UITableViewCell样式.png)

UITableViewCell默认提供展示文字和图片

![avatar](./image/UITableViewCell默认提供文字和图片.png)

```objc

#import "ViewController.h"

@interface TestView : UIView

@end

@implementation TestView

...

@interface ViewController()<UITableViewDataSource>

@end

@implementation ViewController

...
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.

    self.view.backgroundColor = [UIColor whiteColor];
    
    UITableView *uiTableView = [[UITableView alloc] initWithFrame:self.view.bounds];
    uiTableView.dataSource = self;
    
    [self.view addSubview:uiTableView];
    
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section{
    return 20;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
    UITableViewCell *cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier: @"id"];
    cell.textLabel.text = @"主标题";
    cell.detailTextLabel.text = @"副标题";
    return cell;
}

@end

```

### UITableViewCell的重用

系统提供复用回收池，根据reuseIdentifier作为标识

滚动出去的cell进入回收池，即将进入屏幕的cell从回收池中复用。

![avatar](./image/UITableViewCell重用.png)

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
    
    //首先从系统复用hu'shou'chi回收池取
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"id"];
    //取不到创建一个UITableViewCell
    if(!cell) {
         UITableViewCell *cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier: @"id"];
    }
    
   
    cell.textLabel.text = @"主标题";
    cell.detailTextLabel.text = @"副标题";
    return cell;
}
```

### 如何标记和记录某个cell

通过section和row结合标记位置，```NSindexPath```对象

![avatar](./image/标记cell.png)

### UITableViewDelegate

+ 提供滚动过程中，UITableViewCell的出现，消失时机
+ 提供UITableViewCell的高度、headers以及footers设置
+ 提供UITableViewCell各种行为的回调(点击、删除等)

### UITableViewCell点击进入新页面

```objc

...
@interface ViewController()<UITableViewDataSource,UITableViewDelegate>

...

- (void)viewDidLoad {
    [super viewDidLoad];
    ...
    uiTableView.delegate = self;
    ...
}

//设置行高为100
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath{
    return 100;
}


- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
    
    UIViewController *controller = [[UIViewController alloc] init];
    controller.title = [NSString stringWithFormat:@"%@",@(indexPath.row)];
    [self.navigationController pushViewController:controller animated:YES];
}

...

```

### UITableView基本使用

+ 提供最基础的列表类型视图组件
+ 提供默认基础的UITableViewCell样式、header和footer的管理
+ 提供针对UITableViewCell的复用回收逻辑
+ 提供列表基础功能，如点击、删除、插入等

1. 创建UITableView，设置delegate和datasource，通过两个delegate
2. 选择实现UITableViewDataSource中方法、行数、cell复用
3. 选择实现UITableViewDelegate中方法(高度、headerFooter、点击)

## 使用UICollectionView实现瀑布流列表

UITableView的不足

![avatar](./image/UITablView不足.png)

UICollectionView,也只是一个展示的容器，与UITableView有相同的API设计理念--都是基于datasource以及delegate驱动的

+ 提供列表展示的容器
+ 内置复用回收池
+ 支持横向+纵向布局
+ 更加灵活的布局样式
+ 更加灵活的动画
+ 更多的装饰视图
+ 布局之间的切换

![avatar](./image/UICollectionViewCell.png)

row --> item

+ 由于一行可以展示多个视图，row不能准确的表达

### UICollectionViewCell

1.不提供默认的样式

+ 不是以"行"为设计基础
+ 只有contentView/backgroundView
+ 继承自UICollectionReusableView

2.必须先注册Cell类型用于重用

```objc
- (void)registerClass:(nullable Class)cellClass forCellWithReuseIdentifier:(NSString *)identifier;
```

```objc
- (__kindof UICollectionViewCell *)dequeueReusableCellWithReuseIdentifier:(NSString *)identifier forIndexPath:(NSIndexPath *)indexPath;
```

### 创建一个基本的UICollectionView

新建GTVideoControllerViewController

```objc
//
//  GTVideoControllerViewController.m
//  SampleAPP
//
//

#import "GTVideoControllerViewController.h"

@interface GTVideoControllerViewController ()<UICollectionViewDelegate,UICollectionViewDataSource>

@end

@implementation GTVideoControllerViewController

-(instancetype) init{
    self = [super init];
    if(self) {
        self.tabBarItem.title = @"视频";
    }
    return self;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    self.view.backgroundColor = [UIColor whiteColor];
    
    UICollectionViewFlowLayout *flowLayout = [[UICollectionViewFlowLayout alloc] init];
    
    //创建collectionView需要一个layout，和提供在屏幕中的大小
    
    UICollectionView *collectionView = [[UICollectionView alloc] initWithFrame:self.view.bounds collectionViewLayout:flowLayout];
    
    collectionView.delegate = self;
      
    collectionView.dataSource = self;
    
    //注册一个重用的cell类型
    
    [collectionView registerClass:[UICollectionViewCell class] forCellWithReuseIdentifier:@"UICollectionViewCell"];
    
    
    [self.view addSubview:collectionView];
    
    // Do any additional setup after loading the view.
}

- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section{
    return 20;
}

// The cell that is returned must be retrieved from a call to -dequeueReusableCellWithReuseIdentifier:forIndexPath:
- (__kindof UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath{
    //在复用回收池中取UICollectionViewCell,如果没有会自动创建
    UICollectionViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"UICollectionViewCell" forIndexPath:indexPath];
    cell.backgroundColor = [UIColor redColor];
    return cell;
}


@end

```

SceneDelegate.m的视频toolbarItem修改如下：

```objc
...

 GTVideoControllerViewController *videoController = [[GTVideoControllerViewController alloc] init];
 ...

[tabbarController setViewControllers: @[navigationController, videoController, controller3, controller4]];
```

运行后样式如下，点击视频：

![avatar](./image/UICollectionView效果-1.png)


### UICollectionViewLayout

用于生成UICollectionView布局信息的抽象类

+ UICollectionView提供基本的容器、滚动、复用功能
+ 布局信息完全交给开发者
+ 作为抽象类、业务逻辑需要继承
+ 实现UICollectionViewLayout(UISubclassHooks)中的方法
+ 开发者可以自定义生成attribute，系统通过此进行布局
+ 系统提供默认的流式布局Layout

整体上看，逻辑如下

1.设置一个默认的UICollectionViewLaout

2.在UICollectionViewLaout中设置每一个cell对应的attributes

3.最后将UICollectoinViewLayout赋值给UICollectionView

### 基本的流式布局UICollectionViewFlowLayout

```objc
UIKIT_EXTERN API_AVAILABLE(ios(6.0)) @interface UICollectionViewFlowLayout : UICollectionViewLayout

```

![UICollectionFlowLayout](./image/UICollectionViewFlowLayout.png)

流式布局，每行排满后自动换行

minimumInteritemSpacing:用来计算一行可以布局多少个item，实际的大小在布局后重设，设置的是minimum值

```objc
UICollectionViewFlowLayout *flowLayout = [[UICollectionViewFlowLayout alloc] init];
//统一设置流式布局中cell的宽高
flowLayout.minimumLineSpacing = 10;
flowLayout.minimumInteritemSpacing = 10;
flowLayout.itemSize = CGSizeMake((self.view.frame.size.width - 10) / 2, 300);
```

如需个性化设置高度，可以在UICollectionViewLayout的delegate提供的钩子函数中写逻辑

```objc
//个性化设置itemSize
- (CGSize)collectionView:(UICollectionView *)collectionView layout:(UICollectionViewLayout*)collectionViewLayout sizeForItemAtIndexPath:(NSIndexPath *)indexPath{
    if(indexPath.item % 3 == 0){
        return CGSizeMake(self.view.frame.size.width, 100);
    }else {
       return  CGSizeMake((self.view.frame.size.width - 10) / 2, 300);
    }
}
```

### UICollectionView使用总结

1. 创建UICollectoinViewLayout，使用系统默认流式布局，或自定义布局
2. 创建UICollectionView,设置delegate和datasource，注册cell类型
3. 选择实现UICollectionViewDataSource中方法、行数、cell复用
4. 选择实现UICollectionViewDelegate中方法(点击、滚动等)

### 列表的选择与使用

+ UITableView其实算是特殊Flow布局的UICollectionView
+ 简单的列表仍然可以使用UITableView
+ 有双向的布局、特殊布局等非普通场景(瀑布流、弹幕)使用UICollectionView
+ Layout的切换、在选择屏幕时有优雅的动画使用UICollectionView

## 如何实现多个列表的横向滑动

### UIScrollView

![avatar](./image/UIScrollView.png)

上图中，frame是屏幕的大小，contentSize是展示的图片大小

### UIScrollView的例子，设置翻页显示不同背景

```objc
//
//  GTRecommendViewController.m

#import "GTRecommendViewController.h"

@interface GTRecommendViewController ()

@end

@implementation GTRecommendViewController

-(instancetype) init{
    self = [super init];
    if(self) {
        self.tabBarItem.title = @"推荐";
    }
    return self;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    self.view.backgroundColor = [UIColor whiteColor];
    
    UIScrollView *scrollView = [[UIScrollView alloc] initWithFrame:self.view.bounds];
    scrollView.backgroundColor = [UIColor lightGrayColor];
    scrollView.contentSize = CGSizeMake(self.view.bounds.size.width *5,self.view.bounds.size.height);
    
    //滚动翻页类似效果
    NSArray *colorArray = @[[UIColor redColor],[UIColor blueColor], [UIColor yellowColor], [UIColor lightGrayColor], [UIColor grayColor]];
    
    for(int i = 0; i<5;i++) {
        [scrollView addSubview:({
            UIView *uiView = [[UIView alloc] initWithFrame:CGRectMake(scrollView.bounds.size.width * i, 0, scrollView.bounds.size.width, scrollView.bounds.size.height)];
            uiView.backgroundColor = [colorArray objectAtIndex:i];
            uiView;
        })];
    }
    
    scrollView.pagingEnabled = YES;
    [self.view addSubview:scrollView];
    // Do any additional setup after loading the view.
}

@end

```

### UIScrollViewDelegate

常用的UIScrollViewDelegate


滚动，监听页面滚动以及根据Offset做业务逻辑
```objc
- (void)scrollViewDidScroll:(UIScrollView *)scrollView; 
```

拖拽，中断一些业务逻辑，如视频/gif播放(微信聊天记录，拖拽时gif停止播放)

```objc
// called on start of dragging (may require some time and or distance to move)
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView;

// called on finger up if the user dragged. decelerate is true if it will continue moving afterwards
- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate;

```

减速，页面停止时开始逻辑，如视频自动播放

```objc
- (void)scrollViewWillBeginDecelerating:(UIScrollView *)scrollView;   // called on finger up as we are moving
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView;      // called when scroll view grinds to a halt

```


