---
title: iOS开发相关知识
categories: Study
abbrlink: Knowledge-About-iOS-Development
date: 2024-12-19 21:40:57
tags:
---

![](topic.jpg)

iOS开发相关知识。

<!-- more -->

# 资料

```
https://objccn.io/
https://swiftgg.gitbook.io/swift/
https://swift.org/
https://github.com/anupamchugh/iOS14-Resources
https://awesome-tips.gitbook.io/ios/

# 动画
https://github.com/CalvinCheungCoder/SwiftAnimation
https://zhangdinghao.cn/2017/07/02/Swift-Animation10/

# 侧滑菜单
https://ithelp.ithome.com.tw/articles/10195966
https://www.jianshu.com/p/1b704103be1f
https://blog.coding.net/blog/creating-a-sidebar-menu-using-swrevealviewcontroller-in-swift

# 设计规范
https://developer.apple.com/documentation/
https://developer.apple.com/design/human-interface-guidelines/
https://www.microsoft.com/design/fluent/
https://principleformac.com/tutorial.html
https://material.io/design
```

# 基本知识

## 常量

### if let

判断对象的值是否为nil。

```
let name: String? = "老王"
let age: Int? = 10

if let nameNew = name,
    let ageNew = age {
    // nameNew和ageNew不为nil时执行
}
```

### guard let

保证对象的值不为nil。

```
let name: String? = "老王"
let age: Int? = 10

guard let nameNew = name,
    let ageNew = age else {
    // nameNew和ageNew有其一为nil时执行
}
```

## Extension

当类A需要遵循协议B时，可按照以下原始写法。

```
class A: B{
  ...
}
```

也可通过extension的方式实现，在协议众多的情况下会使代码逻辑更加清楚。

```
class A{
  ...
}

extension A: B{
  ...
}
```

## 页面生命周期

### viewDidLoad

新建页面时被调用，即页面首先载入的方法，类似初始化。一个页面只会调用一次viewDidLoad方法。

### viewDidAppear

将页面放置到视图时被调用。每次显示页面都会调用该方法，比如进入页面后返回时会调用一次viewDidAppear方法。

### viewDidLayoutSubviews

当页面被放置好时被调用。viewDidLayoutSubviews只会在视图上所有Auto Layout设定或是大小自动计算完成后才会执行，因此会在视图更新、旋转、变动时调用该方法。

在开启APP时，viewDidLayoutSubviews会在viewDidLoad之后执行。

注意，如果要获取控件在设备上的实际尺寸，需要在viewDidLayoutSubviews而不是viewDidLoad进行操作。在viewDidLayoutSubviews调用会得到计算完约束后得到的尺寸，而在viewDidLoad调用会得到在Storyboard或程序添加控件时设定的默认尺寸。

# Xcode

## 设置代码折叠

打开Xcode的Preference-Text Editing，勾选code folding ribbon即可。

# 组件

## Collection View

展示Cell列表。

### 使用方法

在Storyboard中添加UICollectionView，在其中的Cell布局所需要的控件。

在该页对应的ViewController建立该UICollectionView的Outlet，此处将名称设定为collectionView。新建一个类并继承自UICollectionViewCell，此处取名为MyCell。

将Storyboard中UICollectionView的Cell的Class设定为MyCell，并设置Identifier，此处设置为`identifierMyCell`。

将Cell中的控件通过Outlet添加到MyCell中。回到ViewController的代码页，修改以下代码以建立委托。

```
override func viewDidLoad(){
  super.viewDidLoad()
  ...
  collectionView.delegate = self
  collectionView.dataSource = self
}
```

建立委托后会提示错误，点击错误提示后会自动添加UICollectionViewDataSource协议。需要重写以下协议实现方法。

```
func collectionView(collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    // 返回Cell的数量
    return 4
}

func collectionView(collectionView: UICollectionView, cellForItemAtIndexPath indexPath: NSIndexPath) -> UICollectionViewCell {
    // 对Cell进行设置
    // 获取当前操作的Cell
    let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "identifierMyCell", for: indexPath) as! MyCell

    // 设置对应控件的值
    // 控件Outlet在MyCell中被定义
    cell.label.text = "Text"

    // 返回Cell
    return cell
}
```

### 布局设置

```
# 新建layout
let layout = UICollectionViewFlowLayout()

# 设置滚动方向
layout.scrollDirection = .horizontal

# 设置cell的大小
layout.itemSize = CGSize(width: 100, height: 100)

# 设置分组的间距
layout.sectionInset = UIEdgeInsets.zero

# 设置最小行间距
layout.minimumLineSpacing = 0

# 设置最小列间距
layout.minimumInteritemSpacing = 10

# 设置边界的填充距离
flowLayout.sectionInset = UIEdgeInsets.init(top: 10, left: 10, bottom: 10, right: 10)

# 最后将layout赋值给UICollectionView
self.mCollectionView.collectionViewLayout = layout 
```

### 点击事件

#### 添加

直接在Storyboard中将Cell拖动至需要跳转的页面即可。

#### 获取点击的位置

点击上面添加的跳转线即segue，设置该segue的identifier，此处为bookJump。重写跳转前的ViewController的prepare方法，示例代码如下。

```
# DestViewController为跳转后的ViewController
# indexInCollection为DestViewController的变量，用于存储当前点击的index
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    if(segue.identifier == "bookJump") {
        (segue.destination as! DestViewController).indexInCollection = collectionView.indexPath(for: sender as! UICollectionViewCell)?.item
            
    }
}
```

## UIColorPickerViewController

颜色选择器。

### 使用方法

通过以下代码新建。

```
// 组件初始化
let picker = UIColorPickerViewController()

// 设置初始颜色
picker.selectedColor = self.view.backgroundColor!

// 设置delegate
picker.delegate = self

// 展示组件
self.present(picker, animated: true, completion: nil)
```

需要给呈现该组件的页面ViewController添加UIColorPickerViewControllerDelegate协议，示例如下。

```
extension ViewController: UIColorPickerViewControllerDelegate {
    // 选择颜色完成，关闭picker时被调用
    func colorPickerViewControllerDidFinish(_ viewController: UIColorPickerViewController) {
        // 需要进行的设置
    }
    
    // 每次切换颜色时被调用
    func colorPickerViewControllerDidSelectColor(_ viewController: UIColorPickerViewController) {
        // 需要进行的设置
    }
}
```

也可不使用以上协议，而使用Combine框架。示例代码如下。

```
import Combine

class ViewController: UIViewController{
    // Global declaration, to keep the subscription alive.
    var cancellable: AnyCancellable?

    @IBAction func changeBackground(_ sender: Any) {
        
        let picker = UIColorPickerViewController()
        picker.selectedColor = self.view.backgroundColor!
        
        //  Subscribing selectedColor property changes.
        self.cancellable = picker.publisher(for: \.selectedColor)
            .sink { color in
                
                //  Changing view color on main thread.
                DispatchQueue.main.async {
                    self.view.backgroundColor = color
                }
            }
        
        self.present(picker, animated: true, completion: nil)
    }
}
```

## UIFontPickerViewController

字体选择器。

### 使用方法

通过以下代码新建。

```
let vc = UIFontPickerViewController()
vc.delegate = self
present(vc, animated: true)
```

需要给呈现该组件的页面ViewController添加UIFontPickerViewControllerDelegate协议，示例如下。

```
extension ViewController: UIFontPickerViewControllerDelegate {
    // 完成字体选择后调用
    func fontPickerViewControllerDidPickFont(_ viewController: UIFontPickerViewController) {
        // 读取字体描述符
        guard let descriptor = viewController.selectedFontDescriptor else { return }

        // 从字体描述符获取字体
        let font = UIFont(descriptor: descriptor, size: 36)
        
        // 根据需求完成剩余操作
    }

    // 取消选择字体时调用
    func fontPickerViewControllerDidCancel(_ viewController: UIFontPickerViewController) {
        // 根据需求完成剩余操作
    }
}
```

## UIImageView

### 添加点击事件

```
let singleTapGesture = UITapGestureRecognizer(target: self, action: #selector(imageViewClick))
imageView?.addGestureRecognizer(singleTapGesture)
imageView?.isUserInteractionEnabled = true
```

## Button

### 设置按钮为系统图标

```
# 一般做法
button.setImage(UIImage(systemName: "search"), for: .normal)

# 设置图标权重
let boldConfig = UIImage.SymbolConfiguration(weight: .bold)
let boldSearch = UIImage(systemName: "search", withConfiguration: boldConfig)
button.setImage(boldSearch, for: .normal)
```

## UIView

### 添加点击事件

```
view.isUserInteractionEnabled = true
let tapGesture = UITapGestureRecognizer(target: self, action: #selector(tapGestureAction))
tapGesture.numberOfTapsRequired = 1
view.addGestureRecognizer(tapGesture)

@objc func tapGestureAction(){
    // todo  
}
```

### 属性

```
# 描述UIView的大小和在父系坐标系中的位置
testView.frame = CGRect(x: 100, y: 100, width: 50, height: 50)

# 描述UIView的大小和在自身坐标系中的位置
# 一般bounds.origin = (0,0)，而bounds.size = frames.size
testView.bounds = CGRect(x: 0, y: 0, width: 50, height: 50)

# 背景颜色
testView.backgroundColor = UIColor.black

# 是否切除子视图超出部分
testView.clipsToBounds = true

# 透明度
testView.alpha = 0.5

# 是否隐藏视图
# 若为true，表示该View及其子视图都被隐藏
# 同时该View会从响应链中移除，而响应链的下一个会成为第一响应者
testView.isHidden = false
```

### 插入/删除与移动

```
# 将子视图从父视图中删除
removeFromSuperview()

# 添加视图，加在父视图层级结构的最上层
addSubview(view:)

# 在指定位置插入视图
insertSubview(view:,at:)

# 将视图添加到指定视图的下方
insertSubview(view:,belowSubview:)

# 将视图添加到指定视图的下方
insertSubview(view:,aboveSubview:)

# 交换两个指定位置的子视图在父视图中的位置
exchangeSubview(at:,withSubviewAt:)

# 将指定子视图移动到最前面
bringSubview(toFront:)

# 将指定子视图移动到最后面
sendSubview(toBack:)
```

## UIBarButtonItem

### 绑定点击事件

无法调用addTarget()方法。

```
barbuttonitem.target = self;
barbuttonitem.action = @selector(myMethod);
```

## UITextView

### 检测所点击的文本内容

```

```

## Activity Indicator

显示转圈动画。

```
# 假设通过Outlet，连接为activityIndicator
# 开始转圈
activityIndicator.startAnimating()

# 停止转圈
activityIndicator.stopAnimating()
```

## AVPlayer

用于播放视频。需要AVFoundation库，使用前添加以下语句。

```
import AVFoundation
```

### 初始化

```
guard let path = Bundle.main.path(forResource: "video", ofType:"m4v") else {
    debugPrint("video not found")
    return
}
        
let player = AVPlayer(url: URL(fileURLWithPath: path))
let playerLayer = AVPlayerLayer(player: player)
playerLayer.frame = self.view.bounds
self.view.layer.addSublayer(playerLayer)
```

也可通过函数调用的方式，如下。

```
var player: AVPlayer!
var playerLayer: AVPlayerLayer!

func viewDidLoad(){
    ...
    addVideo(file: "video.m4v")
}

func addVideo(file: String) {
    let files = file.components(separatedBy: ".")

    guard let path = Bundle.main.path(forResource: files[0], ofType:files[1]) else {
        debugPrint( "\(files.joined(separator: ".")) not found")
        return
    }
    player = AVPlayer(url: URL(fileURLWithPath: path))

    playerLayer = AVPlayerLayer(player: player)
    playerLayer.frame = self.view.bounds
    self.view.layer.addSublayer(playerLayer)
}
```

### 播放

```
player.play()
```

### 暂停

```
player.pause()
```

### 停止

先将时间设为起始点，然后暂停播放。

```
player.seek(to: CMTime.zero)
player.pause()
```

## UIPageViewController

页面切换控制器，可独立使用或嵌入到其它页面使用。

### 放置

#### 独立使用

直接在Storyboard中添加Page View Controller即可。

#### 嵌入到页面

在页面上添加Container View，再添加一个Page View Controller，连接Container View和Page View Controller，选择Embed。

### 初始化

将Storyboard中的Page View Controller链接到自定义类，此处为PageViewController。示例代码如下。

```
import UIKit

class PageViewController: UIPageViewController {
    // 要展示的所有页面，放置在数组之中
    var controllers: [UIViewController] = []

    override func viewDidLoad() {
        super.viewDidLoad()

        // 新建要展示的页面
        let firstVC = storyboard!.instantiateViewController(withIdentifier: "FirstView") as! FirstViewController
        let secondVC = storyboard!.instantiateViewController(withIdentifier: "SecondView") as! SecondViewController
        let ThirdVC = storyboard!.instantiateViewController(withIdentifier: "ThirdView") as! ThirdViewController

        // 放置到数组
        self.controllers = [ firstVC, secondVC, ThirdVC ]

        // 设置首先显示的页面
        setViewControllers([self.controllers[0]], direction: .forward, animated: true, completion: nil)

        // 数据来源为自身
        self.dataSource = self 
    }
}

// MARK: - UIPageViewController DataSource
extension PageViewController: UIPageViewControllerDataSource {
    // 页面数量
    func presentationCount(for pageViewController: UIPageViewController) -> Int {
        return self.controllers.count
    }
   
    // 左滑时的操作
    func pageViewController(_ pageViewController: UIPageViewController, viewControllerAfter viewController: UIViewController) -> UIViewController? {
        if let index = self.controllers.firstIndex(of: viewController),
            index < self.controllers.count - 1 {
            // 后面还有页面，页面后滑一页
            return self.controllers[index + 1]
        } else {
            // 后面没有页面，不允许滑动
            return nil

            // 后面没有页面，滑动到第一页
            return self.controllers.first
        }
    }

    // 右滑时的操作
    func pageViewController(_ pageViewController: UIPageViewController, viewControllerBefore viewController: UIViewController) -> UIViewController? {
        if let index = self.controllers.firstIndex(of: viewController),
            index > 0 {
            // 前面还有页面，页面前滑一页
            return self.controllers[index - 1]
        } else {
            // 前面没有页面，不允许滑动
            return nil

            // 前面没有页面，滑动到最后一页
            return self.controllers.last
        }
    }
}
```

### 添加当前页显示

若需要添加页控件用以显示当前页码，则需要在放置时选择将UIPageViewController嵌入到页面，然后在Container View下添加UIPageControl控件。

添加完成后，除了完成初始化中的步骤外，还需要在显示时和切换页面时给UIPageControl控件传递信息。该操作通过自定义协议PageViewControllerDelegate实现，代码如下，仅显示比初始化时增加的部分。

```
protocol PageViewControllerDelegate: class {
    // 当页面数量改变时调用
    func pageViewController(pageViewController: PageViewController,
                                    didUpdatePageCount count: Int)
     
    // 当前页索引改变时调用
    func pageViewController(pageViewController: PageViewController,
                                    didUpdatePageIndex index: Int)
}

class PageViewController: UIPageViewController, UIPageViewControllerDelegate {
    ...
    var pageDelegate: PageViewControllerDelegate?

    override func viewDidLoad() {
        ...
        // 协议来源为自身
        self.delegate = self

        // 页面数量改变，通知委托对象
        pageDelegate?.pageViewController(self, didUpdatePageCount: allViewControllers.count)
    }
}

// MARK: - UIPageViewController DataSource
extension PageViewController: UIPageViewControllerDataSource {
    ...
    // 页面切换完毕
    func pageViewController(pageViewController: UIPageViewController,
                            didFinishAnimating finished: Bool,
                            previousViewControllers: [UIViewController],
                            transitionCompleted completed: Bool) {
        if let firstViewController = viewControllers?.first,
          let index = controllers.indexOf(firstViewController) {
            //当前页改变，通知委托对象
            pageDelegate?.pageViewController(self, didUpdatePageIndex: index)
        }
    }
}

// Container View的父控制器类
class ViewController: UIViewController, PageViewControllerDelegate {

    // 页控件
    @IBOutlet weak var pageControl: UIPageControl!
     
    override func viewDidLoad() {
        super.viewDidLoad()
    }
     
    // 场景切换
    override func prepare(for segue: UIStoryboardSegue, sender: AnyObject?) {
        if let pageViewController = segue.destinationViewController as? PageViewController {
            // 设置委托（当页面数量、索引改变时当前视图控制器能触发页控件的改变）
            pageViewController.pageDelegate = self
        }
    }
     
    // 当页面数量改变时调用
    func pageViewController(pageViewController: PageViewController,
                                    didUpdatePageCount count: Int) {
        pageControl.numberOfPages = count
    }
     
    // 当前页索引改变时调用
    func pageViewController(pageViewController: PageViewController,
                                    didUpdatePageIndex index: Int) {
        pageControl.currentPage = index
    }
}
```


### 样式设置

#### 修改滑动样式

将Transition Style设置成Scroll时为滚动形式，Page Curl时为翻页形式。

#### 设置UIPageControl大小

```
// 整体放大，包括圆点和间距
pageControl.transform = CGAffineTransform(scaleX: 2, y: 2)

// 仅放大圆点
pageControl.subviews.forEach {
    $0.transform = CGAffineTransform(scaleX: 2, y: 2)
}
```

# 框架

## 不规则按钮

### 类定义

定义IrregularButton类，在默认按钮的基础上进行拓展。

```
import UIKit

enum BtnType {
    case leftUp
    case leftDown
    case rightUp
    case rightDown
    case center
}

class IrregularButton: UIButton {
    private var path = UIBezierPath()
    private var drawLayer = CAShapeLayer()
    private var textLayer = CATextLayer()
    
    // 初始化时为let btn = IrregularButton(frame: CGRect(...))
    // 用于标记按钮在父控制器的位置
    override init(frame: CGRect) {
        super.init(frame: frame)
        
        self.layer.addSublayer(self.drawLayer)
        self.layer.addSublayer(self.textLayer)
    }
    
    required init?(coder aDecoder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    // 通过path定义按钮形状
    // path.move为移动到起点
    // (0, 0)为初始化时标记的位置
    // 用(0, 0)标记为无左侧偏移
    // path.addLine为从当前点绘制到指定点
    // path.addArc为添加圆弧
    // path.close()标记路径完成
    func path(type: BtnType) {
        
        let path = UIBezierPath()
        
        switch type {
        case .leftUp:
            path.move(to: CGPoint(x: 60, y: 100))
            path.addLine(to: CGPoint(x: 0, y: 100))
            path.addArc(withCenter: CGPoint(x: 100, y: 100), radius: 100, startAngle: .pi, endAngle: .pi*1.5, clockwise: true)
            path.addLine(to: CGPoint(x: 100, y: 60))
            path.addArc(withCenter: CGPoint(x: 100, y: 100), radius: 40, startAngle: .pi*1.5, endAngle: .pi, clockwise: false)
            path.close()
            
            self.path = path
            
        case .leftDown:
            path.move(to: CGPoint(x: 60, y: 100))
            path.addLine(to: CGPoint(x: 0, y: 100))
            path.addArc(withCenter: CGPoint(x: 100, y: 100), radius: 100, startAngle: .pi, endAngle: .pi*0.5, clockwise: false)
            path.addLine(to: CGPoint(x: 100, y: 140))
            path.addArc(withCenter: CGPoint(x: 100, y: 100), radius: 40, startAngle: .pi*0.5, endAngle: .pi, clockwise: true)
            path.close()
            
            self.path = path
        case .rightUp:
            path.move(to: CGPoint(x: 100, y: 60))
            path.addLine(to: CGPoint(x: 100, y: 0))
            path.addArc(withCenter: CGPoint(x: 100, y: 100), radius: 100, startAngle: .pi*1.5, endAngle: 0, clockwise: true)
            path.addLine(to: CGPoint(x: 140, y: 100))
            path.addArc(withCenter: CGPoint(x: 100, y: 100), radius: 40, startAngle: 0, endAngle: .pi*1.5, clockwise: false)
            path.close()
            
            self.path = path
        case .rightDown:
            path.move(to: CGPoint(x: 140, y: 100))
            path.addLine(to: CGPoint(x: 200, y: 100))
            path.addArc(withCenter: CGPoint(x: 100, y: 100), radius: 100, startAngle: 0, endAngle: .pi*0.5, clockwise: true)
            path.addLine(to: CGPoint(x: 100, y: 140))
            path.addArc(withCenter: CGPoint(x: 100, y: 100), radius: 40, startAngle: .pi*0.5, endAngle: 0, clockwise: false)
            path.close()
            
            self.path = path
        case .center:
            path.move(to: CGPoint(x: 140, y: 100))
            path.addArc(withCenter: CGPoint(x: 100, y: 100), radius: 40, startAngle: 0, endAngle: .pi*2, clockwise: true)
            path.close()
            
            self.path = path
        }
        
        self.drawLayer.path = self.path.cgPath

        setNeedsDisplay()
    }
    
    func text(text: String) {
        
        let stringSize = text.boundingRect(with: CGSize(width:100,height:CGFloat.greatestFiniteMagnitude), options: .usesLineFragmentOrigin, attributes: [NSAttributedString.Key.font:UIFont.systemFont(ofSize: 14)], context: nil).size
        
        textLayer.frame = CGRect(x: self.path.bounds.origin.x+(self.path.bounds.size.width/2)-(stringSize.width/2), y: self.path.bounds.origin.y+(self.path.bounds.size.height/2)-(stringSize.height/2), width: stringSize.width, height: stringSize.height)
        
        textLayer.string = NSAttributedString(string: text, attributes: [NSAttributedString.Key.foregroundColor:UIColor.black,
            NSAttributedString.Key.font:UIFont.systemFont(ofSize: 14)])
        textLayer.backgroundColor = UIColor.clear.cgColor
        
        // 设置是否自动换行
        textLayer.isWrapped = false

        // 寄宿图的像素尺寸和视图大小的比
        // 不设置为屏幕比例文字就会像素化
        textLayer.contentsScale = UIScreen.main.scale
        
        setNeedsDisplay()
    }
    
    func backgroundColor(color: UIColor) {
        
        self.drawLayer.fillColor = color.cgColor
        
        setNeedsDisplay()
    }
    
    override func point(inside point: CGPoint, with event: UIEvent?) -> Bool {
        if self.path.contains(point) {
            return true
        } else {
            return false
        }
    }
}
```

### 使用

```
let btn = IrregularButton(frame: CGRect(x: 80, y: 100, width: 0, height: 0))

btn.path(type: .leftUp)
btn.backgroundColor(color: UIColor(red: 0, green: 0, blue: 0, alpha: 1))
btn.text(text: "Some Text")
btn.addTarget(self, action: #selector(btnClick), for: .touchUpInside)

self.view.addSubview(btn)

func btnClick(){
    // todo
}
```

## OCR识别

```
https://developer.apple.com/documentation/vision/structuring_recognized_text_on_a_document
https://developer.apple.com/documentation/vision/recognizing_text_in_images
```

### 修改识别语言

默认识别语言为英文。

```
# 修改识别语言为简体中文
textRecognitionRequest.recognitionLanguages = ["zh-CN"]
```

## 手部识别

### 例程

```
https://developer.apple.com/documentation/vision/detecting_hand_poses_with_vision
https://heartbeat.fritz.ai/swipeless-tinder-using-ios-14-vision-hand-pose-estimation-64e5f00ce45c
```

### 常见问题

#### 镜头方向随设备旋转而变化

需要旋转cameraPreviewLayer，即相机预览层。

```
cameraPreviewLayer.connection.videoOrientation = getCaptureVideoOrientation()
```

其中getCaptureVideoOrientation()函数如下。

```
func getCaptureVideoOrientation() -> AVCaptureVideoOrientation {
    switch UIDevice.current.orientation {
    case .portrait,.faceUp,.faceDown:
        return .portrait
    case .portraitUpsideDown:
        return .portrait
    case .landscapeLeft:
        return .landscapeRight
    case .landscapeRight:
        return .landscapeLeft
    default:
        return .portrait
    }
}
```

<details>
<summary>【进阶】旋转视频</summary>

按照以下方式设置。

```
session.outputs[0].connection(with: .video)?.videoOrientation = true
```
</details>

## 文本转语音

```
// 
class ViewController: UIViewController, AVSpeechSynthesizerDelegate {
    // 开始朗读
    func StartSpeech(){
        let utterance = AVSpeechUtterance(string: inportTextField.text!)
        utterance.voice = AVSpeechSynthesisVoice(language: "zh-CN")
        let synthesizer = AVSpeechSynthesizer()

        // 音调
        utterance.pitchMultiplier = tonebar.value

        // 速度
        utterance.rate = speedbar.value
        synthesizer.speak(utterance)
    }

    // 停止朗读
    func StopSpeech() {
        // 立即中断语音
        synth.stopSpeaking(at: AVSpeechBoundary.immediate)
    }

    // 语音结束后的操作
    func speechSynthesizer(_ synthesizer: AVSpeechSynthesizer, didFinish utterance: AVSpeechUtterance) {
        // code
    }
}
```

# 类型

## NSMutableAttributedString

可设置样式的富文本。

### 样式设置

```
let labelString = "Underline Label"
let textColor: UIColor = .blue
let underLineColor: UIColor = .red
let underLineStyle = NSUnderlineStyle.styleSingle.rawValue


let labelAtributes:[NSAttributedStringKey : Any]  = [
    NSAttributedStringKey.foregroundColor: textColor,
    NSAttributedStringKey.underlineStyle: underLineStyle,

    # 下划线颜色
    NSAttributedStringKey.underlineColor: underLineColor
]

let underlineAttributedString = NSAttributedString(string: labelString, attributes: labelAtributes)
textView.attributedText = underlineAttributedString
```

### 样式追加

```
let attributed = NSMutableAttributedString(attributedString: textView.attributedText!)
attributed.addAttribute(.foregroundColor, value: ChangeColor, range: .init(location: 0, length: attributed.length))
textView.attributedText = NSAttributedString(attributedString: attributed)
```

### 转换为String

使用string方法即可。

```
var attributedString = NSMutableAttributedString(string: "hello, world!")
var s = attributedString.string
```

## 数组

### 创建

```
var animals = ["cats", "dogs", "chimps", "moose"]
```

### 删除元素

```
# 删除第一个元素
animals.removeFirst() // "cats"

# 删除最后一个元素
animals.removeLast() // "moose"

# 删除索引处的元素
animals.remove(at: 2) // "chimps"
# 或以下方法
let pets = animals.filter { $0 != "chimps" }

# 删除未知索引的元素（仅一个元素）
if let index = animals.firstIndex(of: "chimps") {
    animals.remove(at: index)
}

# 删除未知索引的元素（多个元素）
animals = animals.filter(){$0 != "chimps"}
```

# 页面与动作控制

## 子视图添加和移除

```
# 添加
self.addSubview(subView)

# 移除
subView.removeFromSuperView()
```

## 监听进程事件

在AppDelegete.swift下的AppDelegete类添加以下函数即可。

```
func applicationDidFinishLaunching(_ application: UIApplication) {
    print("程序启动")
}

func applicationWillResignActive(_ application: UIApplication) {
    print("程序变为不活跃状态")
}

func applicationDidEnterBackground(_ application: UIApplication) {
    print("程序进入后台")
}

func applicationDidEnterForeground(_ application: UIApplication) {
    print("程序进入前台")
}

func applicationDidBecomeActive(_ application: UIApplication) {
    print("变为活跃状态")
}

func applicationWillTerminate(_ application: UIApplication) {
    print("程序终止")
}
```

## 监听触摸事件

UIView等相关视图是UIResponder的子类，而UIResponder可对相关触摸事件做出反馈。将Storyboard中的视图绑定到自定义类后，即可重写以下方法对触摸事件做出反馈。

```
# 点击事件触发
func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?)

# 移动事件触发
func touchesMoved(_ touches: Set<UITouch>, with event: UIEvent?)

# 结束点击事件触发
func touchesEnded(_ touches: Set<UITouch>, with event: UIEvent?) 

# 取消点击事件触发
func touchesCancelled(_ touches: Set<UITouch>, with event: UIEvent?)
```

## 在View中获取点击坐标

需要重写View的touchesBegan方法。

```
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
    for touch:AnyObject in touches {
        let t:UITouch = touch as! UITouch
        print(t.location(in: self.contentView))
    }
}
```

## 隐藏返回键

在需要隐藏返回键页面的ViewController的viewDidLoad()方法中添加以下语句即可。

```
self.navigationController?.navigationBarHidden = true
```

## 从跳转中获取父/子控制器

在Storyboard中对需要设置的segue设置identifier，此处为`Jump`。假设父控制器为ParentViewController，子控制器为ChildViewController。

父控制器代码如下。

```
class ParentViewController: UIViewController {
    var child: ChildViewController?

    // Set the child delegate
    // And child's parent delegate
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
        if segue.identifier =="Jump" {
                self.child = (segue.destinationViewController as ChildViewController)
                (segue.destinationViewController as ChildViewController).parent = self
        }
    }
}
```

子控制器代码如下。

```
class ChildViewController: UITableViewController {
    var parent: ParentViewController?
}
```

## 从视图获取父控制器

```
func nextresponsder(viewself:UIView) -> UIViewController{
    var vc:UIResponder = viewself
     
    while vc.isKind(of: UIViewController.self) != true {
        vc = vc.next!
    }

    return vc as! UIViewController
}
```

## 获取子视图在父视图中的坐标

```
# 获取childView在fatherView的坐标，size是childView
let crect = childView?.convert((childView?.frame)!, to: fatherView)

# 或以下方式
let crect2 = fatherView?.convert((childView?.frame)!, from: childView)
```

## 不可退回的页面跳转

### 设置

使用present()方法即可。

### 淡出切换

```
# toVC为目标控制器
toVC.modalTransitionStyle = .crossDissolve
```

## 可退回的页面跳转

### 原理

若使用普通的present方法，将无法实现页面回退。

设当前有两个页面firstVC和secondVC，通过按钮进行两个页面的切换，按钮均绑定btnClicked方法。若使用正常跳转逻辑，代码示例如下。

```
// firstVC的类
class FirstViewController: UIViewController {
    var secondVC: SecondViewController!
    ...

    func btnClicked() {
        secondVC = SecondViewController()
        secondVC.firstVC = self
        self.present(secondVC, animated: true)
    }
}

// secondVC的类
class SecondViewController: UIViewController {
    var firstVC: FirstViewController!
    ...

    func btnClicked() {
        self.present(firstVC, animated: true)
    }
}
```

在firstVC点击按钮可以顺利跳转到secondVC，但在secondVC点击按钮时将会报错，原因是跳转到的页面已经被呈现过。因此需要使用NavigationController作为页面跳转的载体，通过pushViewController和popViewController方法实现回退跳转。

### 初始化与跳转设置

在Storyboard点击底层页面，即无法再回退的页面，此处以firstVC为例，在菜单栏选择Editor-Embed in-Navigation Controller以使NavigationController嵌入到页面中。

嵌入后代码示例如下。

```
// firstVC的类
class FirstViewController: UIViewController {
    ...

    func btnClicked() {
        let secondVC = SecondViewController()
        self.navigationController?.pushViewController(secondVC, animated: true)
    }
}

// secondVC的类
class SecondViewController: UIViewController {
    ...

    func btnClicked() {
        // 返回前一页
        self.navigationController?.pushViewController(firstVC, animated: true)

        // 返回到最前页（根页面）
        // self.navigationController?.popToRootViewController(animated: true)

        // 返回前两页
        // let count = self.navigationController!.viewControllers.count
        // if let preController = self.navigationController?.viewControllers[count-3] {
            self.navigationController?.popToViewController(preController, animated: true)
        }
    }
}
```

### 隐藏导航栏

若需要隐藏所有页面的导航栏，可点击Navigation Controller的导航栏，在右侧勾选isHidden即可。或者通过以下代码实现。

```
self.navigationController?.navigationBar.isHidden = true
```

若需要隐藏特定页面的导航栏，则在该页面的控制器添加以下重写函数即可。

```
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    self.navigationController?.setNavigationBarHidden(true, animated: animated)
}

override func viewWillDisappear(_ animated: Bool) {
    super.viewWillDisappear(animated)
    self.navigationController?.setNavigationBarHidden(false, animated: animated)
}
```

### 转场样式

#### 淡出转场

添加以下扩展。

```
extension UINavigationController {
    func fadeIn(_ viewController: UIViewController) {
        let transition: CATransition = CATransition()
        transition.duration = 0.3
        transition.type = CATransitionType.fade
        view.layer.add(transition, forKey: nil)
        pushViewController(viewController, animated: false)
    }

    func fadeOut(_ viewController: UIViewController) {
        let transition: CATransition = CATransition()
        transition.duration = 0.3
        transition.type = CATransitionType.fade
        view.layer.add(transition, forKey: nil)
        popViewController(animated: false)
    }
}
```

按照如下方式使用即可。

```
# 用于取代self.navigationController?.pushViewController(viewController, animated: true)
self.navigationController?.fadeIn(viewController)

# 用于取代self.navigationController?.popViewController(animated: true)
self.navigationController?.fadeOut()
```

### 转场动画

在页面跳转时，可设置转场动画。转场动画通过自定义的TransitionCoordinator类实现，该类用于承接NavigationController的delegate，定义转场时的操作。

为向TransitionCoordinator类传递动画前后的控件位置，首先定义animTransitionable协议用于从主控制器获取相关内容。

```
# 定义需要获取的控件
protocol animTransitionable {
    var backGroundView: UIView { get }
    var AnimateButton: UIButton { get }
}
```

在主控制器添加关于animTransitionable的拓展，从而指定每个选项对应的应当获取的内容。

```
extension MainViewController : animTransitionable
{
    # 保证动画过程中背景一致
    var backGroundView: UIView {
        return backgroundView
    }

    # 需要进行动画操作的控件
    var AnimateButton: UIButton {
        return animateButton
    }
}
```

在需要转场的页面添加以下代码。

```
let transition = TransitionCoordinator()

# 开启转场动画
navigationController?.delegate = transition

# 关闭转场动画
navigationController?.delegate = nil
```

TransitionCoordinator类定义如下。在进行push时，会调用PushAnimator()方法，在进行pop时则调用PopAnimator()方法。

```
import UIKit

class TransitionCoordinator: NSObject, UINavigationControllerDelegate {
    func navigationController(_ navigationController: UINavigationController,
        animationControllerFor operation: UINavigationControllerOperation,
        from fromVC: UIViewController,
        to toVC: UIViewController) -> UIViewControllerAnimatedTransitioning? {
        
        switch operation {
        case .push:
            return PushAnimator()
        case .pop:
            return PopAnimator()
        default:
            return nil
        }
    }  
}
```

以PushAnimator()方法为例，示例如下。其原理为新建一个视图，在其上隐藏来源控制器，完成动画操作，然后显示目标控制器。PopAnimator()方法原理完全一致。

```
import UIKit

class PushAnimator: NSObject, UIViewControllerAnimatedTransitioning {
    # 动画时长
    func transitionDuration(using transitionContext: UIViewControllerContextTransitioning?) -> TimeInterval {
        return 2.0
    }
    
    # 动画设置
    func animateTransition(using transitionContext: UIViewControllerContextTransitioning) {
        # 新建视图
        let containerView = transitionContext.containerView

        # 用于提取来源控制器和目标控制器中的指定控件
        # 这些控件在animTransitionable协议中被定义
        guard let fromVC = transitionContext.viewController(forKey: .from) as? animTransitionable,
            let toVC = transitionContext.viewController(forKey: .to) as? animTransitionable else {
                transitionContext.completeTransition(false)
                return
        }
        
        # 获取来源控制器和目标控制器
        let fromViewController = transitionContext.viewController(forKey: .from)!        
        let toViewController = transitionContext.viewController(forKey: .to)!
        
        # 添加背景到containerView并设置为来源控制器的颜色
        let backgroundView = UIView()
        backgroundView.frame = fromVC.backGroundView.frame
        backgroundView.backgroundColor = fromVC.backGroundView.backgroundColor
        containerView.addSubview(backgroundView)

        # 添加需要设置动画的控件
        # 初始位置来源于来源控制器，通过convert进行坐标变换
        let animateButton = UIButton()
        animateButton.frame = containerView.convert(fromVC.AnimateButton.frame, from: fromVC.AnimateButton.superview)
        animateButton.backgroundColor = fromVC.AnimateButton.backgroundColor
        animateButton.layer.cornerRadius = fromVC.AnimateButton.layer.cornerRadius
        animateButton.layer.masksToBounds = fromVC.AnimateButton.layer.masksToBounds

        # 添加来源控制器和目标控制器
        # 并设置为隐藏
        containerView.addSubview(fromViewController.view)
        containerView.addSubview(toViewController.view)
        fromViewController.view.isHidden = true
        toViewController.view.isHidden = true

        # 动画设置
        # 缩放为原来的0.9倍，持续时间0.4秒，弹跳动作强度1.3
        let animator1 = {
            UIViewPropertyAnimator(duration: 0CGAffineTransform(scaleX: 0.9, y: 0.9).4, dampingRatio: 1.3) {
                animateButton.transform = 
            }
        }()
        
        # 消除圆角，持续时间0.3秒，弹跳动作强度0.9
        let animator2 = {
            UIViewPropertyAnimator(duration: 0.3, dampingRatio: 0.9) {
                animateButton.layer.cornerRadius = 0
            }
        }()

        # 在第一个动画完成后再进行第二个动画
        animator1.addCompletion { _ in
            animator2.startAnimation()
            # 若需要添加延迟则使用以下代码
            animator2.startAnimation(afterDelay: 0.1)
        }

        # 在第二个动画完成后显示目标控制器
        animator2.addCompletion { _ in
            # 隐藏相关控件
            cellBackground.removeFromSuperview()

            # 显示目标控制器
            toViewController.view.isHidden = false
            
            transitionContext.completeTransition(true)
        }

        # 开启动画
        animator1.startAnimation()  
    }
}
```

# 界面与外观设置

## 添加自定义字体

将所需字体拖动到Xcode的文件栏，选择Copy items if needed和create groups。然后打开Info.plist，添加`Fonts provided by application`，内容为字体文件名称，注意包括扩展名。

可在编辑组件时直接修改组件的Font属性使用自定义字体，也可在代码中调用。由于字体名称与字体文件名称不一定完全对应，因此需要通过以下代码输出当前APP可用的字体列表。该段代码放置于任意可被执行的位置即可，比如放置在会出现的ViewController的viewDidLoad()方法中。

```
for family in UIFont.familyNames.sorted() {
    let names = UIFont.fontNames(forFamilyName: family)
    print("Family: \(family) Font names: \(names)")
}
```

知道所用字体的名称后，通过以下命令即可。

```
# label为需要改变字体的标签
guard let customFont = UIFont(name: "CustomFont-Light", size: UIFont.labelFontSize) else {
    fatalError("""
    Failed to load the custom font.
    Make sure the font file is included in the project and the font name is spelled correctly.
    """)
}

label.font = UIFontMetrics.default.scaledFont(for: customFont)
label.adjustsFontForContentSizeCategory = true
```

## 启动页相关

### 设置启动页

启动页一般在LaunchScreen.storyboard中设置，可点击该文件后，查看右侧Interface Builder Document下Use as Launch Screen是否已被勾选，若未勾选则勾选即可。

也可在项目设置中点击APP，在General的Deployment Info-Main Interface选择启动后显示的页面，一般为Main。在App Icons and Launch Images-Launch Screen File选择启动页，即LaunchScreen。

### 修改启动页界面

在LaunchScreen.storyboard中放置控件即可。注意该文件中的ViewController不可绑定到自定义类，因此该ViewController的控件属性只可提前设定。

若启动页有图片，注意需要直接放置到项目目录，而不要放置到Assets.xcasset文件夹中，且应当为JPG而非PNG格式。

### 设置启动页停留时间

在AppDelegate.swift下设置启动页时长，如下。

```
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
    # 在顶部状态栏显示风火轮
    UIApplication.shared.isNetworkActivityIndicatorVisible = true
    # 启动页显示时间3s
    Thread.sleep(forTimeInterval: 3)
    # 关闭风火轮
    UIApplication.shared.isNetworkActivityIndicatorVisible = false

    return true
}
```

### 在不同设备设置不同布局

主要用于在iPhone和iPad设置不同的布局。假设Main_iPad.storyboard为用于iPad的布局，Main_iPhone.storyboard为用于iPhone的布局，在AppDelegate.swift下添加以下代码即可。

```
func applicationDidFinishLaunching(_ application: UIApplication) {
    if(UIDevice.current.userInterfaceIdiom == .pad){
        currentStoryBoard = UIStoryboard(name: "Main_iPad", bundle: nil)
        initViewController = currentStoryBoard.instantiateInitialViewController()
        self.window?.rootViewController = initViewController
    }
    else if(UIDevice.current.userInterfaceIdiom == .phone){
        currentStoryBoard = UIStoryboard(name: "Main_iPhone", bundle: nil)
        initViewController = currentStoryBoard.instantiateInitialViewController()
        self.window?.rootViewController = initViewController
    }
}
```


## 添加圆角/描边

可通过以下代码设置。

```
# 圆角
label.layer.cornerRadius = 10

# 描边
label.layer.masksToBounds = true
label.layer.borderWidth = 1
label.layer.borderColor = #colorLiteral(red: 1, green: 1, blue: 1, alpha: 1)
```

也可在Storyboard中点击需要设置的控件，在右侧的Runtime Attributes添加以下项目。

```
layer.borderWidth
layer.borderColorFromUIColor
layer.cornerRadius
clipsToBounds
```

## 添加阴影

```
# 设置阴影颜色
view.layer.shadowColor = sColor.cgColor

# 设置透明度
view.layer.shadowOpacity = opacity

# 设置阴影半径
view.layer.shadowRadius = radius

# 设置阴影偏移量
# offset为CGSize(width: height:)类型
# width为正数时向右偏移，height为正数时向下偏移
view.layer.shadowOffset = offset

# 显示阴影
view.layer.masksToBounds = false
```

## 控件变换与动画

通过transform属性可以修改控件的位移、缩放、旋转。

### 变换

```
# 修改位置
# 每次形变都基于原始值
btn.transform = CGAffineTransformMakeTranslation()
# 基于btn上次的值
btn.transform = CGAffineTransformTranslate()

# 缩放
# 按照比例缩放
btn.transform = CGAffineTransform(scaleX: 0.9, y: 0.9)
# a表示x水平方向的缩放，tx表示x水平方向的偏移
# d表示y垂直方向的缩放，ty表示y垂直方向的偏移
# b和c不为零表示视图发生了旋转
btn.transform = CGAffineTransform(a: 0.9, b: 0, c: 0, d: 0.9, tx: 100, ty: 100)

# 旋转
# angle是弧度值，通过宏M_PI设置
btn.transform = CGAffineTransformMakeRotation()
btn.transform = CGAffineTransformRotate()

# 旋转和移动
btn.transform = CGAffineTransformMakeTranslation()
btn.transform = CGAffineTransformRotate()

# 重置位置
btn.transform = CGAffineTransformIdentity
```

### 动画

以为按钮添加缩放动画为例，代码如下。

```
# 设置动画类型
let zoomInAndOut = CABasicAnimation(keyPath: "transform.scale")

# 起始比例
zoomInAndOut.fromValue = 1.0

# 终止比例
zoomInAndOut.toValue = 0.5

# 动画时长
zoomInAndOut.duration = 1.0

# 重复次数
zoomInAndOut.repeatCount = 5

# 自动翻转
# fromValue->toValue后自动进行toValue->fromValue
zoomInAndOut.autoreverses = true

# 速度
# 由于设置了autoreverses，则fromValue->toValue和toValue->fromValue各耗费1.0s
# 将速度设置为2倍后保证这组动作在1.0s完成
zoomInAndOut.speed = 2.0

# 为按钮添加动作
button.layer.addAnimation(zoomInAndOut, forKey: nil) 
```


## Popover

Popover是类似气泡的ViewController显示模式，一般在点击按钮时触发。

### 通过segue

若在Storyboard中通过segue实现跳转，在拉segue时选择Present As Popover即可。在segue的Anchor设置中可设置箭头指向的对象。

在iPhone上Popover默认无法显示出像iPad的效果，需要进行以下设置。

```
// ViewController为Popover对应控制器的父控制器
// 添加UIPopoverPresentationControllerDelegate，让本页的相关函数控制该Popover的行为
class ViewController: UIViewController, UIPopoverPresentationControllerDelegate {
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        // 设置delegate为该控制器
        segue.destination.popoverPresentationController?.delegate = self

        // 设置大小
        segue.destination.preferredContentSize = CGSize(width: 150, height: 200)

    }

    // 使iPhone显示Popover效果
    func adaptivePresentationStyle(for controller: UIPresentationController, traitCollection: UITraitCollection) -> UIModalPresentationStyle {
        return .none
    }
}
```

### 通过纯代码

以按钮点击事件触发Popover显示为例，触发代码如下。在iPhone上显示iPad的Popover效果需要的代码操作与用Storyboard的实现相同。

```
@IBAction func buttonPressed(_ sender: Any) {
    // TableViewController为需要显示的Popover的控制器的identifier
    if let controller = storyboard?.instantiateViewController(withIdentifier: "TableViewController") {
        // 设置显示效果为Popover
        controller.modalPresentationStyle = .popover

        // 指定箭头指向的位置
        controller.popoverPresentationController?.barButtonItem = navigationItem.rightBarButtonItem
        // 也可指定为指向特定的View
        controller.popoverPresentationController?.sourceView = eyeSwitch
        // 指定为特定View后，可通过以下代码设置箭头在View中的具体位置（不设置时默认指向左上方）
        // 以下代码将位置改为View的右下方
        controller.popoverPresentationController?.sourceRect = CGRect(origin: .zero, size: eyeSwitch.frame.size)

        // 显示Popover
        present(controller, animated: true, completion: nil)
    }
}
```

# 功能实现

## 分页

准备好处理完成的NSAttributedString，提前调整好字体、颜色、格式等信息。然后通过以下代码实现。

```
# 创建NSLayoutManager
let layoutManager = NSLayoutManager()

# 如果没有给特定部分文字区域设置单独的布局，可设置此项为false以提高性能
layoutManager.allowsNonContiguousLayout = false

# 使用之前准备好的NSAttributedString进行初始化NSTextStorage
let textStorage = NSTextStorage(attributedString: string)
textStorage.addLayoutManager(layoutManager)

# 设定文字显示区域参数
let viewSize: CGSize = CGSize(width: textAreaWidth, height:  textAreaHeight)

# 设定textView的内间距
let textInsets = UIEdgeInsets.zero
let textViewFrame = CGRect(x: 0, y: 0, width: viewSize.width, height: viewSize.height)

# 开始分页
var glyphRange: Int = 0
var numberOfGlyphs: Int = 0

var ranges: [NSRange] = []
repeat {
    let textContainer = NSTextContainer(size: viewSize)
    layoutManager.addTextContainer(textContainer)
    
    # 不断创建textView让NSLayoutManager进行内容分页
    let textView = UITextView(frame: textViewFrame, textContainer: textContainer)
    textView.isEditable = false
    textView.isSelectable = false
    textView.textContainerInset = textInsets
    textView.showsVerticalScrollIndicator = false
    textView.showsHorizontalScrollIndicator = false
    textView.isScrollEnabled = false
    textView.bounces = false
    textView.bouncesZoom = false
    
    # 获取当前分页内容所在位置
    let range = layoutManager.glyphRange(for: textContainer)
    ranges.append(range)
    
    # 判定是否分页完成
    glyphRange = NSMaxRange(range)
    numberOfGlyphs = layoutManager.numberOfGlyphs
} while glyphRange < numberOfGlyphs - 1
```

## 分词/分句

为String添加Extension，将需要分词/分句的内容设为String类型后调用其tokenize()方法即可。

```
extension String {
    func tokenize() -> [String] {
        let word = self

        # kCFStringTokenizerUnitWord更改为kCFStringTokenizerUnitSentence即为分句
        let tokenize = CFStringTokenizerCreate(kCFAllocatorDefault, word as CFString!, CFRangeMake(0, word.characters.count), kCFStringTokenizerUnitWord, CFLocaleCopyCurrent())
        CFStringTokenizerAdvanceToNextToken(tokenize)
        var range = CFStringTokenizerGetCurrentTokenRange(tokenize)
        var keyWords : [String] = []
        while range.length > 0 {
            let wRange = word.index(word.startIndex, offsetBy: range.location)..<word.index(word.startIndex, offsetBy: range.location + range.length)
            let keyWord = word.substring(with:wRange)
            keyWords.append(keyWord)
            CFStringTokenizerAdvanceToNextToken(tokenize)
            range = CFStringTokenizerGetCurrentTokenRange(tokenize)
        }
        return keyWords
    }
}
```

# Unity相关

Unity工程文件可以打包为Xcode工程文件，但其语言为Objective-C。

## 嵌入到Swift工程

### 新版教程

适用于Unity 2020及以上版本。官方示例如下。

```
https://github.com/Unity-Technologies/uaal-example/blob/master/docs/ios.md
```

准备好需要嵌入的Swift工程，注意新建工程时需要将Interface设为Storyboard，Life Cycle设为UIKit App Delegate。

打开Unity工程，点击File-Build Settings，将Platforms设为iOS，右侧设置中Run in Xcode as选择Release，选项全部取消勾选，然后导出工程文件。

将以上两个工程放置到同一目录，示例如下。

```
└── APP
    ├── Interface
    │   ├── ...
    │   └── Interface.xcodeproj
    └── Unity
        ├── ...
        └── Unity-iPhone.xcodeproj
```

打开Xcode并点击File-New-Workspace，存放位置为上述的目录，此处为APP。完成后在左侧点击右键并选择`Add Files to ...`，选择所有的工程文件，此处为Interface.xcodeproj和Unity-iPhone.xcodeproj。

添加完成后左侧点击Interface工程，选择TARGETS下的APP，在General-Frameworks, Libraries, and Embedded Content下点击`+`号，选择Unity-iPhone下的UnityFramework.framework。若没有该文件，需要先点击Unity-iPhone工程，设置好签名后运行一次代码，使该Framework得以生成。

然后点击Unity-iPhone下的Data文件夹，在右侧的Target Membership勾选UnityFramework。然后打开Interface工程下的Info.plist，删除`Application Scene Manifest`一项。

在Interface工程下新建Unity.swift文件，继承自UIResponder。该类用于控制Unity的启动与关闭，代码如下。

```
import Foundation
import UnityFramework

class Unity: UIResponder, UIApplicationDelegate {

    static let shared = Unity()

    private let dataBundleId: String = "com.unity3d.framework"
    private let frameworkPath: String = "/Frameworks/UnityFramework.framework"

    private var ufw : UnityFramework?
    private var hostMainWindow : UIWindow?

    private var isInitialized: Bool {
        ufw?.appController() != nil
    }

    func show() {
        if isInitialized {
            showWindow()
        } else {
            initWindow()
        }
    }

    func setHostMainWindow(_ hostMainWindow: UIWindow?) {
        self.hostMainWindow = hostMainWindow
    }

    private func initWindow() {
        if isInitialized {
            showWindow()
            return
        }

        guard let ufw = loadUnityFramework() else {
            print("ERROR: Was not able to load Unity")
            return unloadWindow()
        }

        self.ufw = ufw
        ufw.setDataBundleId(dataBundleId)
        ufw.register(self)
        ufw.runEmbedded(
            withArgc: CommandLine.argc,
            argv: CommandLine.unsafeArgv,
            appLaunchOpts: nil
        )
    }

    private func showWindow() {
        if isInitialized {
            ufw?.showUnityWindow()
        }
    }

    private func unloadWindow() {
        if isInitialized {
            ufw?.unloadApplication()
        }
    }

    private func loadUnityFramework() -> UnityFramework? {
        let bundlePath: String = Bundle.main.bundlePath + frameworkPath

        let bundle = Bundle(path: bundlePath)
        if bundle?.isLoaded == false {
            bundle?.load()
        }

        let ufw = bundle?.principalClass?.getInstance()
        if ufw?.appController() == nil {
            let machineHeader = UnsafeMutablePointer<MachHeader>.allocate(capacity: 1)
            machineHeader.pointee = _mh_execute_header

            ufw?.setExecuteHeader(machineHeader)
        }
        return ufw
    }
}

extension Unity: UnityFrameworkListener {

    func unityDidUnload(_ notification: Notification!) {
        ufw?.unregisterFrameworkListener(self)
        ufw = nil
        hostMainWindow?.makeKeyAndVisible()
    }
}
```

然后打开AppDelegate.swift，删除所有与场景相关的代码，并添加以下内容，以指定关闭Unity后应当返回的窗口。

```
import UIKit

@main
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

        Unity.shared.setHostMainWindow(window)

        return true
    }
}
```

在需要调用Unity模块的地方调用以下代码即可。

```
Unity.shared.show()
```

若需要退出Unity，需要在Unity新建一个按钮，点击按钮时执行退出操作并返回到iOS App中。新建一个Button后新建一个名为QuitBehavior.cs的脚本并绑定到该Button，脚本代码如下，事件为OnButtonPressed。完成后重新导出Xcode工程文件并重复上述步骤即可。

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class QuitBehavior : MonoBehaviour
{
    public void OnButtonPressed()
    {
        Application.Unload();
    }
}
```

### 常见问题

#### 加载Vuforia框架时输出台显示dataset xxx could not be loaded and cannot be activated

数据集没有加载到原Swift工程，在此处为Interface工程。

数据集位置在Unity-iPhone工程的Data/Raw/Vuforia中。点击Unity-iPhone工程，点击TARGETS下的App，在Build Phases-Copy Bundle Resources下删除Vuforia文件夹并重新添加，注意不要勾选Copy items if needed，选择create folder references方式。

点击左侧新出现的Vuforia文件夹，在右侧勾选UnityFramework。点击Interface工程下TARGETS的App，在Build Phases-Copy Bundle Resources添加Unity-iPhone工程的Vuforia，同样不要勾选Copy items if needed，选择create folder references方式。完成后重新运行即可。

#### 加载Unity时输出台显示dyld: Library not loaded: @rpath/ARFoundationDriver.framework/ARFoundationDriver...Reason: image not found

点击Interface工程，选择Build Phases，点击左侧`+`号并选择New Copy Files Phase，然后在下面添加的Copy Files中，将Destination选为Frameworks，然后点击`+`号，添加所需要的Frameworks，此处为ARFoundationDriver.framework。

### 旧版教程

适用于Unity 2017/2018。

准备好需要嵌入的Swift工程。打开Unity工程，在Scripts/Editor下新建一个文件，名为XcodePostBuild.cs，内容如下。注意需要修改XcodeProjectRoot和XcodeProjectName，可使用相对路径，至xcodeproj所在目录为止。

```
/*
MIT License

Copyright (c) 2017 Jiulong Wang

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
*/

#if UNITY_IOS

using System;
using System.Linq;
using System.Collections.Generic;
using System.IO;
using System.Text;

using UnityEngine;
using UnityEditor;
using UnityEditor.Callbacks;
using UnityEditor.iOS.Xcode;

/// <summary>
/// Adding this post build script to Unity project enables Unity iOS build output to be embedded
/// into existing Xcode Swift project.
///
/// However, since this script touches Unity iOS build output, you will not be able to use Unity
/// iOS build directly in Xcode. As a result, it is recommended to put Unity iOS build output into
/// a temporary directory that you generally do not touch, such as '/tmp'.
///
/// In order for this to work, necessary changes to the target Xcode Swift project are needed.
/// Especially the 'AppDelegate.swift' should be modified to properly initialize Unity.
/// See https://github.com/jiulongw/swift-unity for details.
/// </summary>
public static class XcodePostBuild
{
    /// <summary>
    /// Path to the root directory of Xcode project.
    /// This should point to the directory of '${XcodeProjectName}.xcodeproj'.
    /// It is recommended to use relative path here.
    /// Current directory is the root directory of this Unity project, i.e. the directory of 'Assets' folder.
    /// Sample value: "../xcode"
    /// </summary>
    private const string XcodeProjectRoot = <PROJECT PATH>;

    /// <summary>
    /// Name of the Xcode project.
    /// This script looks for '${XcodeProjectName} + ".xcodeproj"' under '${XcodeProjectRoot}'.
    /// Sample value: "DemoApp"
    /// </summary>
    private const string XcodeProjectName = <PROJECT NAME>;

    /// <summary>
    /// Directories, relative to the root directory of the Xcode project, to put generated Unity iOS build output.
    /// </summary>
    private const string ClassesProjectPath = XcodeProjectName + "/Unity/Classes";
    private const string LibrariesProjectPath = XcodeProjectName + "/Unity/Libraries";

    /// <summary>
    /// Path, relative to the root directory of the Xcode project, to put information about generated Unity output.
    /// </summary>
    private const string ExportsConfigProjectPath = XcodeProjectName + "/Unity/Exports.xcconfig";

    private const string PbxFilePath = XcodeProjectName + ".xcodeproj/project.pbxproj";

    private const string BackupExtension = ".bak";

    /// <summary>
    /// The identifier added to touched file to avoid double edits when building to existing directory without
    /// replace existing content.
    /// </summary>
    private const string TouchedMarker = "https://github.com/jiulongw/swift-unity#v1";

    [PostProcessBuild]
    public static void OnPostBuild(BuildTarget target, string pathToBuiltProject)
    {
        if (target != BuildTarget.iOS)
        {
            return;
        }

        PatchUnityNativeCode(pathToBuiltProject);

        UpdateUnityIOSExports(pathToBuiltProject);

        UpdateUnityProjectFiles(pathToBuiltProject);
    }

    /// <summary>
    /// Writes current Unity version and output path to 'Exports.xcconfig' file.
    /// </summary>
    private static void UpdateUnityIOSExports(string pathToBuiltProject)
    {
        var config = new StringBuilder();
        config.AppendFormat("UNITY_RUNTIME_VERSION = {0};", Application.unityVersion);
        config.AppendLine();
        config.AppendFormat("UNITY_IOS_EXPORT_PATH = {0};", pathToBuiltProject);
        config.AppendLine();

        var configPath = Path.Combine(XcodeProjectRoot, ExportsConfigProjectPath);
        var configDir = Path.GetDirectoryName(configPath);
        if (!Directory.Exists(configDir))
        {
            Directory.CreateDirectory(configDir);
        }

        File.WriteAllText(configPath, config.ToString());
    }

    /// <summary>
    /// Enumerates Unity output files and add necessary files into Xcode project file.
    /// It only add a reference entry into project.pbx file, without actually copy it.
    /// Xcode pre-build script will copy files into correct location.
    /// </summary>
    private static void UpdateUnityProjectFiles(string pathToBuiltProject)
    {
        var pbx = new PBXProject();
        var pbxPath = Path.Combine(XcodeProjectRoot, PbxFilePath);
        pbx.ReadFromFile(pbxPath);

        ProcessUnityDirectory(
            pbx,
            Path.Combine(pathToBuiltProject, "Classes"),
            Path.Combine(XcodeProjectRoot, ClassesProjectPath),
            ClassesProjectPath);

        ProcessUnityDirectory(
            pbx,
            Path.Combine(pathToBuiltProject, "Libraries"),
            Path.Combine(XcodeProjectRoot, LibrariesProjectPath),
            LibrariesProjectPath);

        pbx.WriteToFile(pbxPath);
    }

    /// <summary>
    /// Update pbx project file by adding src files and removing extra files that
    /// exists in dest but not in src any more.
    ///
    /// This method only updates the pbx project file. It does not copy or delete
    /// files in Swift Xcode project. The Swift Xcode project will do copy and delete
    /// during build, and it should copy files if contents are different, regardless
    /// of the file time.
    /// </summary>
    /// <param name="pbx">The pbx project.</param>
    /// <param name="src">The directory where Unity project is built.</param>
    /// <param name="dest">The directory of the Swift Xcode project where the
    /// Unity project is embedded into.</param>
    /// <param name="projectPathPrefix">The prefix of project path in Swift Xcode
    /// project for Unity code files. E.g. "DempApp/Unity/Classes" for all files
    /// under Classes folder from Unity iOS build output.</param>
    private static void ProcessUnityDirectory(PBXProject pbx, string src, string dest, string projectPathPrefix)
    {
        var targetGuid = pbx.TargetGuidByName(XcodeProjectName);
        if (string.IsNullOrEmpty(targetGuid)) {
            throw new Exception(string.Format("TargetGuid could not be found for '{0}'", XcodeProjectName));
        }

        // newFiles: array of file names in build output that do not exist in project.pbx manifest.
        // extraFiles: array of file names in project.pbx manifest that do not exist in build output.
        // Build output files that already exist in project.pbx manifest will be skipped to minimize
        // changes to project.pbx file.
        string[] newFiles, extraFiles;
        CompareDirectories(src, dest, out newFiles, out extraFiles);

        foreach (var f in newFiles)
        {
            if (ShouldExcludeFile(f))
            {
                continue;
            }

            var projPath = Path.Combine(projectPathPrefix, f);
            if (!pbx.ContainsFileByProjectPath(projPath))
            {
                var guid = pbx.AddFile(projPath, projPath);
                pbx.AddFileToBuild(targetGuid, guid);

                Debug.LogFormat("Added file to pbx: '{0}'", projPath);
            }
        }

        foreach (var f in extraFiles)
        {
            var projPath = Path.Combine(projectPathPrefix, f);
            if (pbx.ContainsFileByProjectPath(projPath))
            {
                var guid = pbx.FindFileGuidByProjectPath(projPath);
                pbx.RemoveFile(guid);

                Debug.LogFormat("Removed file from pbx: '{0}'", projPath);
            }
        }
    }

    /// <summary>
    /// Compares the directories. Returns files that exists in src and
    /// extra files that exists in dest but not in src any more. 
    /// </summary>
    private static void CompareDirectories(string src, string dest, out string[] srcFiles, out string[] extraFiles)
    {
        srcFiles = GetFilesRelativePath(src);

        var destFiles = GetFilesRelativePath(dest);
        var extraFilesSet = new HashSet<string>(destFiles);

        extraFilesSet.ExceptWith(srcFiles);
        extraFiles = extraFilesSet.ToArray();
    }

    private static string[] GetFilesRelativePath(string directory)
    {
        var results = new List<string>();

        if (Directory.Exists(directory))
        {
            foreach (var path in Directory.GetFiles(directory, "*", SearchOption.AllDirectories))
            {
                var relative = path.Substring(directory.Length).TrimStart('/');
                results.Add(relative);
            }
        }

        return results.ToArray();
    }

    private static bool ShouldExcludeFile(string fileName)
    {
        if (fileName.EndsWith(".bak", StringComparison.OrdinalIgnoreCase))
        {
            return true;
        }

        return false;
    }

    /// <summary>
    /// Make necessary changes to Unity build output that enables it to be embedded into existing Xcode project.
    /// </summary>
    private static void PatchUnityNativeCode(string pathToBuiltProject)
    {
        EditMainMM(Path.Combine(pathToBuiltProject, "Classes/main.mm"));
        EditUnityAppControllerH(Path.Combine(pathToBuiltProject, "Classes/UnityAppController.h"));
        EditUnityAppControllerMM(Path.Combine(pathToBuiltProject, "Classes/UnityAppController.mm"));

        if (Application.unityVersion == "2017.1.1f1")
        {
            EditMetalHelperMM(Path.Combine(pathToBuiltProject, "Classes/Unity/MetalHelper.mm"));
        }

        // TODO: Parse unity version number and do range comparison.
        if (Application.unityVersion.StartsWith("2017.3.0f")
                || Application.unityVersion.StartsWith("2017.3.1f")
                || Application.unityVersion.StartsWith("2017.4.1f")
                || Application.unityVersion.StartsWith("2017.4.2f"))
        {
            EditSplashScreenMM(Path.Combine(pathToBuiltProject, "Classes/UI/SplashScreen.mm"));
        }
    }

    /// <summary>
    /// Edit 'main.mm': removes 'main' entry that would conflict with the Xcode project it embeds into.
    /// </summary>
    private static void EditMainMM(string path)
    {
        EditCodeFile(path, line =>
        {
            if (line.TrimStart().StartsWith("int main", StringComparison.Ordinal))
            {
                return line.Replace("int main", "int old_main");
            }

            return line;
        });
    }

    /// <summary>
    /// Edit 'UnityAppController.h': returns 'UnityAppController' from 'AppDelegate' class.
    /// </summary>
    private static void EditUnityAppControllerH(string path)
    {
        var inScope = false;
        var markerDetected = false;
        var markerAdded = false;

        EditCodeFile(path, line =>
        {
            markerDetected |= line.Contains(TouchedMarker);
            inScope |= line.Contains("inline UnityAppController");

            if (inScope && !markerDetected)
            {
                if (line.Trim() == "}")
                {
                    inScope = false;

                    return new string[]
                    {
                        "// }",
                        "",
                        "NS_INLINE UnityAppController* GetAppController()",
                        "{",
                        "    NSObject<UIApplicationDelegate>* delegate = [UIApplication sharedApplication].delegate;",
                        @"    UnityAppController* currentUnityController = (UnityAppController*)[delegate valueForKey: @""currentUnityController""];",
                        "    return currentUnityController;",
                        "}",
                    };
                }

                if (!markerAdded)
                {
                    markerAdded = true;
                    return new string[]
                    {
                        "// Modified by " + TouchedMarker,
                        "// " + line,
                    };
                }

                return new string[] { "// " + line };
            }

            return new string[] { line };
        });
    }

    /// <summary>
    /// Edit 'UnityAppController.mm': triggers 'UnityReady' notification after Unity is actually started.
    /// </summary>
    private static void EditUnityAppControllerMM(string path)
    {
        var inScope = false;
        var markerDetected = false;

        EditCodeFile(path, line =>
        {
            inScope |= line.Contains("- (void)startUnity:");
            markerDetected |= inScope && line.Contains(TouchedMarker);

            if (inScope && line.Trim() == "}")
            {
                inScope = false;

                if (markerDetected)
                {
                    return new string[] { line };
                }
                else
                {
                    return new string[]
                    {
                        "    // Modified by " + TouchedMarker,
                        "    // Post a notification so that Swift can load unity view once started.",
                        @"    [[NSNotificationCenter defaultCenter] postNotificationName: @""UnityReady"" object:self];",
                        "}",
                    };
                }
            }

            return new string[] { line };
        });
    }

    /// <summary>
    /// Edit 'MetalHelper.mm': fixes a bug (only in 2017.1.1f1) that causes crash.
    /// </summary>
    private static void EditMetalHelperMM(string path)
    {
        var markerDetected = false;

        EditCodeFile(path, line =>
        {
            markerDetected |= line.Contains(TouchedMarker);

            if (!markerDetected && line.Trim() == "surface->stencilRB = [surface->device newTextureWithDescriptor: stencilTexDesc];")
            {
                return new string[]
                {
                    "",
                    "    // Modified by " + TouchedMarker,
                    "    // Default stencilTexDesc.usage has flag 1. In runtime it will cause assertion failure:",
                    "    // validateRenderPassDescriptor:589: failed assertion `Texture at stencilAttachment has usage (0x01) which doesn't specify MTLTextureUsageRenderTarget (0x04)'",
                    "    // Adding MTLTextureUsageRenderTarget seems to fix this issue.",
                    "    stencilTexDesc.usage |= MTLTextureUsageRenderTarget;",
                    line,
                };
            }

            return new string[] { line };
        });
    }

    /// <summary>
    /// Edit 'SplashScreen.mm': Unity introduces its own 'LaunchScreen.storyboard' since 2017.3.0f3.
    /// Disable it here and use Swift project's launch screen instead.
    /// </summary>
    private static void EditSplashScreenMM(string path) {
        var markerDetected = false;
        var markerAdded = false;
        var inScope = false;
        var level = 0;

        EditCodeFile(path, line =>
        {
            inScope |= line.Trim() == "void ShowSplashScreen(UIWindow* window)";
            markerDetected |= line.Contains(TouchedMarker);

            if (inScope && !markerDetected)
            {
                if (line.Trim() == "{")
                {
                    level++;
                }
                else if (line.Trim() == "}")
                {
                    level--;
                }

                if (line.Trim() == "}" && level == 0)
                {
                    inScope = false;
                }

                if (level > 0 && line.Trim().StartsWith("bool hasStoryboard"))
                {
                    return new string[]
                    {
                        "    // " + line,
                        "    bool hasStoryboard = false;",
                    };
                }

                if (!markerAdded)
                {
                    markerAdded = true;
                    return new string[]
                    {
                        "// Modified by " + TouchedMarker,
                        line,
                    };
                }
            }

            return new string[] { line };
        });
    }

    private static void EditCodeFile(string path, Func<string, string> lineHandler)
    {
        EditCodeFile(path, line =>
        {
            return new string[] { lineHandler(line) };
        });
    }

    private static void EditCodeFile(string path, Func<string, IEnumerable<string>> lineHandler)
    {
        var bakPath = path + ".bak";
        if (File.Exists(bakPath))
        {
            File.Delete(bakPath);
        }

        File.Move(path, bakPath);

        using (var reader = File.OpenText(bakPath))
        using (var stream = File.Create(path))
        using (var writer = new StreamWriter(stream))
        {
            string line;
            while ((line = reader.ReadLine()) != null)
            {
                var outputs = lineHandler(line);
                foreach (var o in outputs)
                {
                    writer.WriteLine(o);
                }
            }
        }
    }
}

#endif
```

导出Xcode工程文件，在嵌入的Swift工程下会自动生成Unity文件夹，该文件夹内容由以上脚本生成。

下载以下仓库，将demo下的unity文件夹复制到项目文件夹中，与xcodeproj工程文件同级，与上面的文件夹内容合并。将导出的工程文件的Classes、Libraries和Data文件夹也复制到项目文件夹中，与xcodeproj工程文件同级，与上面的文件夹内容合并。

```
https://github.com/jiulongw/swift-unity
```

用Xcode打开工程，将Unity文件夹下的Classes和Libraries文件夹拖动到左侧文件树，选择Copy items if needed和Create groups。然后将Data文件夹拖动到左侧文件树，选择Create folder references。

点击工程文件，在左侧PROJECT下选择工程，在右侧General选项卡的Configurations下选择Unity配置文件，注意Debug和Release都应当选择。

然后用以下仓库中的AppDelegate.swift、Main.storyboard和ViewController.swift替换掉原工程的对应文件，具体方法为先以Move to trash的方式删除原工程的文件，再通过Copy items if needed和Create groups的方式拖动加入以上文件。

```
https://github.com/jiulongw/swift-unity/tree/master/demo/xcode/DemoApp
```

为防止错误，可删除LaunchScreen.storyboard，并在工程文件属性General选项卡的Launch Screen File下选择Main.storyboard。注意删除时应当以Remove Reference的方式。完成后即可运行。

# 常见问题

## Modifications to the > layout engine must not be performed from a background thread after it has been accessed from the main thread

修改视图内容的代码放到了后台线程导致该错误。修改为如下即可。

```
DispatchQueue.main.async {
    # 原出错代码
}
```

# 参考教程

## iOS - Vision Framework 文字识别

```
https://www.jianshu.com/p/4cea25704191
```

## Xcode折叠函数设置 及快捷键

```
https://blog.csdn.net/liufangbaishi2014/article/details/51602208
```

## 笔记：通过storyboard来创建UICollectionView

```
https://blog.csdn.net/shenjie_xsj/article/details/79679760
```

## Swift 5 UICollectionView中cell的对齐方法(重写flowlayout)

```
https://www.jianshu.com/p/e1d8b51fc2b9
```

## iOS中UICollectionView设置cell大小以及间距

```
https://www.liuandy.cn/ios/2017/12/07/1982.html#.YLzEZJMzYYF
```

## swift：如何在单元格中的按钮被点击时获取indexpath.row？

```
https://www.itranslater.com/qa/details/2135067127244653568
```

## iOS AppDelegate方法，监听进程在后台、被杀死事件

```
https://cloud.tencent.com/developer/article/1174777
```

## Swift -- 获取点击的坐标位置

```
https://www.jianshu.com/p/21de7f5d6591
```

## Modifications to the > layout engine must not be performed from a background thread after it has been accessed from the main thread

```
https://stackoverflow.com/questions/58087536/modifications-to-the-layout-engine-must-not-be-performed-from-a-background-thr
```

## Push to ViewController without back button

```
https://stackoverflow.com/questions/22301647/push-to-viewcontroller-without-back-button/22301820
```

## ios - 如何从uiviewcontainer控制器中获取父控制器

```
https://kb.kaifa99.com/ios/post_6230237
```

## How to use UIColorPickerViewController in Swift?

```
https://www.swiftpal.io/articles/how-to-use-uicolorpickerviewcontroller-in-swift
```

## 为UIImageView添加响应点击事件（Swift）

```
https://blog.csdn.net/feosun/article/details/77942049
```

## ios 子视图获取父视图的视图控制器的方法（oc 和 swift）

```
https://blog.csdn.net/qq_30963589/article/details/82967301
```

## 如何以编程方式为UIButton添加系统图标？

```
https://www.thinbug.com/q/37772411
```

## Swift（十）UIView

```
https://www.jianshu.com/p/ff7ffc8129a6
```

## 如何优雅的做一个小说阅读功能

```
https://syfh.github.io/2020/07/06/%E5%A6%82%E4%BD%95%E4%BC%98%E9%9B%85%E7%9A%84%E5%81%9A%E4%B8%80%E4%B8%AA%E5%B0%8F%E8%AF%B4%E9%98%85%E8%AF%BB%E5%8A%9F%E8%83%BD/
```

## Swift 3 分词

```
https://hicc.me/swift-3-tokenize/
```

## Swift 给一段话分句，或将一句话关键词分组

```
https://www.jianshu.com/p/ef05fc1371b2
```

## Three Ways to Enumerate the Words In a String Using Swift

```
https://medium.com/@sorenlind/three-ways-to-enumerate-the-words-in-a-string-using-swift-7da5504f0062
```

## swift - 使用Swift将NSAttributedString转换为NSString

```
https://www.coder.work/article/251231
```

## iOS：利用代码UIBarButtonItem如何响应事件？

```
https://blog.csdn.net/dchma20242/article/details/101445681
```

## How to let users choose a font with UIFontPickerViewController

```
https://www.hackingwithswift.com/example-code/uikit/how-to-let-users-choose-a-font-with-uifontpickerviewcontroller
```

## SwiftUI UIFontPickerViewController 基础教程

```
https://www.codenong.com/js0c37dafc3eca/
```

## Swift — 為你的 APP 添加自定義字體

```
https://medium.com/jeremy-xue-s-blog/swift-%E7%82%BA%E4%BD%A0%E7%9A%84-app-%E6%B7%BB%E5%8A%A0%E8%87%AA%E5%AE%9A%E7%BE%A9%E5%AD%97%E9%AB%94-1063a7fd30a4
```

## 如何在Swift中从数组中删除元素

```
https://qastack.cn/programming/24051633/how-to-remove-an-element-from-an-array-in-swift
```

## iOS 关于LaunchScreen不显示图片的问题

```
https://blog.csdn.net/RollingPin/article/details/103817911
```

## [Swift]LaunchScreen.storyboard设置启动页！UILaunchImages已被iOS弃用，请使用LaunchScreen.storyboard。 - 山青咏芝 - 博客园

```
https://www.cnblogs.com/strengthen/p/10636993.html
```

## 具有完全不同布局的通用應用程序

```
https://zh.stackoom.com/question/1qFDT/%E5%85%B7%E6%9C%89%E5%AE%8C%E5%85%A8%E4%B8%8D%E5%90%8C%E5%B8%83%E5%B1%80%E7%9A%84%E9%80%9A%E7%94%A8%E6%87%89%E7%94%A8%E7%A8%8B%E5%BA%8F
```

## 快速添加圆角和描边

```
https://vinefiner.github.io/2016/12/01/%E5%BF%AB%E9%80%9F%E6%B7%BB%E5%8A%A0%E5%9C%86%E8%A7%92%E5%92%8C%E6%8F%8F%E8%BE%B9/
```

## iOS 文本转语音(TTS)详解：Swift

```
https://www.geek-share.com/detail/2794338024.html
```

## Swift-视图阴影篇

```
https://www.jianshu.com/p/c47c52e9a8e3
```

## collectionViewCell添加阴影

```
https://www.jianshu.com/p/74894eb7ec3d
```

## 在 iPhone & iPad 上顯示 popover 彈出視窗

```
https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E5%95%8F%E9%A1%8C%E8%A7%A3%E7%AD%94%E9%9B%86/%E5%9C%A8-iphone-ipad-%E4%B8%8A%E9%A1%AF%E7%A4%BA-popover-%E5%BD%88%E5%87%BA%E8%A6%96%E7%AA%97-ac196732e557
```

## ykying/UnityInSwift

```
https://github.com/ykying/UnityInSwift
```

## Integrating Unity into native iOS applications

```
https://docs.unity3d.com/Manual/UnityasaLibrary-iOS.html
```

## Reason: image not found

```
https://www.jianshu.com/p/cadd1dc95cf5
```

## How to embed a Unity game into an iOS native Swift App

```
https://medium.com/@IronEqual/how-to-embed-a-unity-game-into-an-ios-native-swift-app-772a0b65c82
```

## swift - 在 Swift 中仅颜色下划线

```
https://www.coder.work/article/7052075
```

## Swift 4.2 自定义相机

```
https://www.jianshu.com/p/4de39664adfa
```

## ios - 镜像(翻转)相机预览层

```
https://www.coder.work/article/443646
```

## swift 获取子视图在父视图的坐标

```
https://blog.csdn.net/wm9028/article/details/81301064
```

## Swift - 触摸事件（点击，移动，抬起等）说明及用例

```
https://www.hangge.com/blog/cache/detail_674.html
```

## iOS开发系列--触摸事件、手势识别、摇晃事件、耳机线控

```
https://www.cnblogs.com/kenshincui/p/3950646.html
https://www.cnblogs.com/xjf125/p/4862386.html
```


## 在UIView中添加点击事件oc及swift

```
https://blog.csdn.net/timtian008/article/details/51857852
```

## CoderJTao/JTShapedButton

```
https://github.com/CoderJTao/JTShapedButton
```

## How to play a local video with Swift?

```
https://stackoverflow.com/questions/25348877/how-to-play-a-local-video-with-swift
```

## Swift 基本语法03-"if let"和"guard let"

```
https://www.jianshu.com/p/e1fe08c5db1a
```

## 解析 View Controller 生命週期：使用 viewDidLayoutSubviews 的時機

```
https://www.appcoda.com.tw/view-controller-lifecycle/
```

## 『簡易說明Xcode』顯示下一個畫面方法（由程式觸發的方式 — push）

```
https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E6%95%99%E5%AE%A4/%E7%B0%A1%E6%98%93%E8%AA%AA%E6%98%8Excode%E4%B8%AD%E7%9A%84%E9%A1%AF%E7%A4%BA%E4%B8%8B%E4%B8%80%E5%80%8B%E7%95%AB%E9%9D%A2%E6%96%B9%E6%B3%95-%E7%94%B1%E7%A8%8B%E5%BC%8F%E8%A7%B8%E7%99%BC%E7%9A%84%E6%96%B9%E5%BC%8F-push-e0da619641f7
```

## iOS-如何优雅的隐藏主页面的导航栏，而只展示详细页面的导航栏（UINavigationBar）

```
https://juejin.cn/post/6844903955051315208
```

## 【Swift】Swift入門 ~ UIPageViewControllerを使ってみる ~

```
https://swallow-incubate.com/archives/blog/20200313/#basic1
```

## Swift - 页视图控制器（UIPageViewController）的使用

```
https://www.hangge.com/blog/cache/detail_1282.html
https://www.hangge.com/blog/cache/detail_1283.html
```

## Increase the size of the indicator in UIPageViewController's UIPageControl

```
https://stackoverflow.com/questions/42432731/increase-the-size-of-the-indicator-in-uipageviewcontrollers-uipagecontrol
```

## 动画UIButton放大和缩小点击

```
http://cn.voidcc.com/question/p-ejgrgomn-pd.html
```

## IOS UIButton详解 & Button缩放旋转位移实例

```
https://my.oschina.net/wolx/blog/359398
```

## subView的添加与移除

```
https://blog.csdn.net/zhuiyi316/article/details/8308858
```

## franobarrio/animation-transition-viewcontroller-easy

```
https://github.com/franobarrio/animation-transition-viewcontroller-easy
```

## ViewController 轉場初階指南：簡單打造酷炫的轉場動畫

```
https://www.appcoda.com.tw/viewcontroller-transition-easy/
```

## How do I cross dissolve when pushing views on a UINavigationController in iOS 7?

```
https://stackoverflow.com/questions/23530538/how-do-i-cross-dissolve-when-pushing-views-on-a-uinavigationcontroller-in-ios-7
```

## CGAffineTransform

```
https://www.jianshu.com/p/1a2475af4378
```
