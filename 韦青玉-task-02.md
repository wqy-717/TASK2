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
  场景渲染完成后调用一允许对图像进行后处理
 
  

  
  
  
  
