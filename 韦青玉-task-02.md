## ***我的自我介绍***
  各位好！我是物联网2402的韦青玉，很高兴来到GameCore工作室！大家可以叫我小玫瑰（名字由来比较复杂可以忽略不计）。最近比较热门地会介绍自己mbti类型不过虽然我的结果是enfp但是除了做事没啥计划之外感觉不是完全符合我呢，毕竟本人还是有点内向的哈哈哈。在有时间的时候我会玩玩游戏看看漫画什么的。人生理想是做自己喜欢的事情，如果能给别人带来什么帮助的话就更好了，希望世界和平，希望大家都能现实善良的对待然后成为自己理想中的样子。  
  希望可以学习做游戏的知识，然后试着做出自己心目中的很棒的游戏。希望能够和大家一起学习！一起进步！
## ***关于我喜欢的游戏***
  对我来说经常玩的游戏类型基本是女性向，不过非要说的话似乎也并不能完全说得上是喜欢(..?)，更像是一种习惯，因为有我推所以我会一直关注着这方面，但比如像是世界之外，没有我喜欢的角色的话我就不会深入的体验了。 
 
 另外像绣湖这种我也很喜欢，还有muse dash这样比较简单的音游吧。原因是在随时随地都可以玩，并且不像对抗类游戏那样容易红温！平时要上课什么的没有很多空余时间玩游戏，所以很累的时候玩这种类型的游戏会很放松。比较考验操作水平的游戏对我来说难度还是太高了比较难上手，最近也没有什么时间玩游戏所以还没有尝试其中。  
 总而言之，我会比较喜欢有比较完整复杂的主线和丰满的人物塑造或是益于慢下来享受时间的游戏。还有各类抽卡我都很感兴趣！！！第一个想出抽卡玩法的人太厉害了。
# ***Unity中生命周期函数***
## 场景开始时调用函数（每个对象只调用一次：
### Awake：
   在Start函数之前，游戏对象启动激活之后；
### Onable函数：
   （对象处于激活状态时调用）启用对象后立即调用，创建MonoBehaviour实例时执行此调用
### OnLevelWasLoaded：
   执行此函数可告知游戏已加载新关卡。
## Reset：初始化脚本属性
## 第一次帧更新前：
### Start:
   启用脚本实例后调用  
   调用Update之前调用Start,游戏运行过程中不能强制执行。
## 帧之间：
### OnApplicationPause:
   帧的结尾处调用。  
   调用后发出额外帧——>允许游戏显示图形指示暂停状态
## 更新顺序：
### FixedUpdate:
   帧率很低：调用多次该函数；帧率很高：可以不调用  
   基于可靠的计时器
### Update:
   用于帧更新的主要函数
   每帧调用一次
### LateUpdate:
   常见用途：跟随第三人称摄像机  
   每帧调用一次
## 动画更新循环：
>unity评估动画系统时，将调用以下函数和Profiler标记
### OnaStateMachineEnter:
  >在状态机更新 (State Machine Update) 步骤中，当控制器的状态机进行流经 Entry**(?)**状态的转换时，将在第一个更新帧上调用此回调。
  >在转换到 StateMachine 子状态时不会调用此回调。
  >仅当动画图中存在控制器组件时 才会发生回调。
### OnStateMachineExit:
  >在状态机更新 (State Machine Update) 步骤中，当控制器的状态机进行流经 Exit 状态的转换时，将在最后一个更新帧上调用此回调。
  >在转换到 StateMachine 子状态时不会调用此回调。
  >仅当动画图中存在控制器组件时 才会发生回调。
  >注意：将此回调添加到 StateMachineBehaviour 组件会禁用多线程的状态机评估。
### 触发动画事件：
   调用在上次更新时间和当前更新时间之间采样的所有剪辑中的所有动画事件
### StateMachineBehaviour:
  >仅当动画图中存在控制器组件时才会执行此步骤
  一层最多3个活动状态：当前状态、中断状态、下一个状态。
  使用一个定义 OnStateEnter OnStateUpdate OnStateExit 回调的StateMachineBehaviour 组件为每个活动状态调用此函数。
  依次针对每个状态调用此函数。

### OnAnimatorMove:
  在每个帧更新中为每个Animator组件调用一次函数来修改根运动（Root Motion)
### StateMachineBehaviour(OnStateMove):
  使用定义此回调的StateMachineBehaviour 在每个活动状态中调用此函数
### OnAnimatorIK:
>仅当使用人形骨架时执行此事件
  设置动画IK**(?)**
  为每个启用IK pass的Animator Controller 层进行一次此调用
### StateMachineBehaviour(OnStateIK):
  >使用 在启用IK pass的层上定义此回调的StateMachineBehaviour组件在每个活动状态中调用此函数**(?)**
### WriteProperties:
  从主线程**(?)**将所有其他动画属性写入场景
##有用的性能分析标记
>脚本生命周期流程图中显示的某些动画函数不是可以调用的事件函数；它们是 Unity 处理动画时调用的内部函数。
>这些函数具有 Profiler 标记，因此您可以使用 Profiler 查看 Unity 在帧中调用这些函数的时间。知道 Unity 调用这些函数的时间有助于准确了解所调用的事件函数的具体执行时间。
>例如，假设在 FireAnimationEvents 回调中调用 Animator.Play。如果知道只有在执行状态机更新 (State Machine Update) 和流程图 (Process Graph) 函数后才会触发 FireAnimationEvents 回调，就可以预期动画剪辑会在下一帧播放，而不是马上播放。
###状态机更新(State Machine Update):
  在执行序列的此步骤中评估所有状态机。
  >仅当动画图中存在控制器组件时才会发生此回调
### ProcessGraph:
  评估所有动画组
### ProcessAnimation:
  混合动画图的结果
### WriteTransforms:
  将所有动画变换从工作线程写入场景
## Rendering
### OnBecameVisivle/OnBecameInvisible:
  将对象变为任何摄像机可见/不可见
### OnWillRenderObject:
  对象可见：为每个摄像机调用一次
### OnPreRender:
  摄像机开始渲染场景之前调用
### OnPostRender:
  摄像机完成场景渲染后调用
### OnRenderImage:
  *场景渲染完成后*调用以允许对图像进行后处理
### OnGUI:
  *每帧调用多次*以响应GUI事件，首先*处理布局*和*重新绘制事件*，然后为每个输入事件*处理布局*和*键盘/鼠标*事件
### OnDrawGizmos:
  用于在场景试图中绘制辅助图标以实现可视化
## 销毁对象时
###  OnDestroy:
  对象存在的最后一帧*完成所有帧更新之后*，调用此函数
## 退出时：
>在场景中的所有活动对象上调用以下函数
### OnApplicationQuit:
  在*退出应用程序之前*在所有游戏对象上调用此函数。在编辑器中，用户*停止播放模式时*调用函数。
### OnDisable:
  行为被禁用或处于非活动状态时，调用此函数。
## 执行顺序
>1. Awake()
>2. OnEnable()
>3. Start()
>4. FixedUpdate()
>5. Update()
>6. LateUpdate()
>7. OnGUI()
>8. OnDisable()
>9. OnDestroy()
# ***学习笔记***
## C#基础学习  
键盘按键：e.g:空格键KeyCode.space
### 1.作为行为组件(component)的脚本,将他们附加到对象 使用GetComponent
```
using UnityEngine;
using System.Collections;
public class BehaviourScript : MonoBehaviour
{
  变量名 = GetComponent<组件名>();
  GetComponent<组件名>().xxx.xxx = xxx.xxx;
}
```
### 2.变量和函数  
先定义变量类型(int float...)再初始化  
` int myInt = 5;`  
   此处myInt是自定义变量的名称。后半部分给变量初始化值为5.  
   
   如何在控制台观察变量的变化？  
   `Debug.Log(变量名);`  
   函数：
   ```返回值类型 函数名（临时变量）  
     {
        xxx;  
        return 变量名;  
     }
   ```
   >此处临时变量可不加

   返回值为空的类型：void  
 ### 3.约定和语法  
 分号结尾;  
 点运算符:用于代码内的单词之间,作用相当于找地址  
  e.g:```transform.position.x```    
  注释:  
  单行 ```// 这是一行注释```  
  多行```/*xxx*/```
 ### 4.If语句  
 ```if(判断语句)  执行语句```  
 ### 5.循环（loop)  
 for循环、While循环、Do While循环、foreach循环  
 ### 6.作用域和访问修饰符  
 用花括号包含某类的代码块 代码内的变量在该类的作用域内  
 访问修饰符：声明变量是放在数据类型前的关键词 public/private  
 ```public int a;```  
 >用public可以在该类之外的类中访问该类中的变量 private则不行。
### 7.Awake和Start  
在加载脚本时自动调用的函数（一个对象绑定脚本中的生命周期内只能调用一次）  
### 8.Update和FixedUpdate  
每帧调用一次  
区别：Update不按固定时间调用，FixedUpdate按固定时间调用(任何影响刚体的动作都要用这个执行)  
### 9.矢量(Vector)数学  
所有坐标相对于原点2d（x,y）;3d(x,y,z)  
### 10.启用和禁用组件（脚本也是组件！）  
启用："Enabled" flag;
e.g:  
```
myLight.enabled = !myLight.enabled;
```
>感叹号表示把它设为非当前值
```myLight.enabled = false;//表示禁用```
```myLight.enabled = true;//表示启用```
### 11.激活游戏对象  
使用SetActive函数
```
using UnityEngine;
using System.Collections;
public class ActiveObjects : MonoBehaviour
{
    void Start ()
    {
        gameObject.SetActive(false);
    }
}
```
>此处对象被停用，同理True则表示激活对象。
如何检查对象是否被激活？
```
using UnityEngine;
using System.Collections;

public class CheckState : MonoBehaviour
{
    public GameObject myObject;
    
    
    void Start ()
    {
        Debug.Log("Active Self: " + myObject.activeSelf);
        Debug.Log("Active in Hierarchy" + myObject.activeInHierarchy);
    }
}
```
>将对象设为public 输入以上代码可在控制台查看状态。
### 12.Translate和Rotate  
>更改非刚体对象的位置和旋转
```
public class TransformFunctions : MonoBehaviour
{
    public float moveSpeed = 10f;//设置移动速度
    public float turnSpeed = 50f;//设置旋转速度
    
  void Update ()
  {
    if(Input.GetKey(KeyCode.UpArrow))//按向上箭头则会移动
    transform.Translate(Vector3.forward * moveSpeed * Time.deltaTime);
  }
}
```
### 13.Look At  
>让摄像头对准某特定对象
```
public Transform 变量名;
    void Update ()
    {
        transform.LookAt(变量名);
    }
}
```
>变量为你想对准的对象
### 14.线性插值：Lerp函数  
>具体可见：第14点 <a href="[超链接地址](https://learn.u3d.cn/tutorial/beginner-gameplay-scripting?chapterId=63562b29edca72001f21d179#60405ee029cbbf0021dda603)" target="_blank">线性插值</a>
Mathf.Lerp函数：接受3个float参数：起始值、结束值、距离  
e.g：``` float result = Mathf.Lerp (3f, 5f, 0.5f);//result=4```  
>Color.Lerp 和 Vector3.Lerp使用方法完全相同。
e.g:
```
Vector3 from = new Vector3 (1f, 2f, 3f);
Vector3 to = new Vector3 (5f, 6f, 7f);
// 此处 result = (4, 5, 6)
Vector3 result = Vector3.Lerp (from, to, 0.75f);
```
### 15.Destroy  
删除对象：```Destroy(对象名)```  
删除组件：```Destroy()GetComponent<组件名>());```
### 16.Getbutton和Getkey  
获取用于输入的按键  
返回True/False
### 17.GetAxis  
Edit-Project Settings-Input  
返回浮点值(-1,1)  
### 18.OnMouseDown  
通过点击鼠标对游戏内的刚体进行一系列操作  
### 19.GetComponent  
用于访问其他组件（包括脚本）  
会占用大量空间，尽量减少调用 最好在Awake/Start中调用(这俩只调用一次)
<>作用：让类型成为参数  
### 20.Delta Time  
如何平滑改变值详见第20点：[C#初级编程]（https://learn.u3d.cn/tutorial/beginner-gameplay-scripting?chapterId=63562b29edca72001f21d179#60406191141da10020320549""）  
### 21.数据类型  
值类型(Value)包含某个值：int float double bool char Structs(e.g:Vector3)  
引用类型(Reference)包含值存储位置的存储地址:Classes(e.g:Transform GameObject)
### 22.类  
作用：存储变量和函数，易于管理  
一个类能有多个不同的构造函数处理各个部分
### 23.Instantiate  
作用：克隆游戏对象（常用于克隆prefab(预配置对象)）
```Instantiate(参数)//参数就是要克隆的对象名称```  
### 24.数组Arrays  
作用：储存同类型数据 便于管理。（不属于类型）  
引入：```数据类型[] 数组名 = new 数据类型[元素个数]```
```
//两种方法使用数组
//1.定义一个数组 并初始化存有2个同类型数据
public class Arrays : MonoBehaviour
{
  int[] myIntArray = new int[2];
  void Start()
  {
    myIntArray[0]=33;
    myIntArray[1]=66;
  }
}
public class Arrays : MonoBehaviour
{
  int
}
//引入同时初始化数组内数据
int[] myIntArray = {33,66};
```
>可通过public数组在inspector内改变长度属性
### 25.Invoke
>用于安排在以后的时间进行方法调用
### 26.枚举  
>创建相关常量的集合
### 27.Switch语句
>简化条件，将单个变量与一系列常量比较时使用





   
   


  
  
  
  
