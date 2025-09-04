+++ 
draft = false
date = 2025-08-29T16:14:50+08:00
title = "贪吃蛇小游戏流程总结和回顾"
description = "重看贪吃蛇小游戏教程的笔记"
slug = ""
authors = ["liumo"]
tags = ["godot","game"]
categories = ["Summary"]
externalLink = ""
series = []
+++
# 贪吃蛇小游戏--游戏基本设计原则
## 一.问题记录
>Q:什么时候要用到唯一名称访问
>>移动也不会改变    

>Q:_physic_process这个函数是什么时候调用的，和process是什么差别
>>_physic_process是在每一帧中用积累时间判断的固定60hz的循环执行参数，用于移动、碰撞、物理的部分。  
>>process每帧都执行，用于动画、UI、输入、游戏逻辑。  
>>触发的类型事件就是用信号。


>Q:global这种类需要继承吗
>>无需继承也能用

>Q:节点的移动是整个添加就可以吗 pos=head.position+move_dir*32
>>是的

>Q:为什么要`if pos.x<x_min:
		pos=Vector2(x_max,pos.y)`而不是直接pos.x=x_max
>>其实是直接return Vector2(x_max,pos.y)

>Q:实例化的话类型是什么`var food_create=food.instantiate()`
>>就是food整个场景。

>Q:信号的函数到底放在哪里
>>涉及到主逻辑的部分就放在主场景的脚本下，其他的可以放在对应的节点中。

>Q:不知道该怎么删除食物
>>用碰撞检测信号函数的参数直接删除 area.queue_free()

>Q:随机食物要写在哪里，process里面吗，但是这个每帧都会刷新吧
>>间接的信号触发

>Q:绑在节点上的脚本和不绑在节点上的脚本,和公用有关系吗
>>没有关系，想要用不绑在节点上的脚本，export进来变成自己的参数就好了

>Q:什么时候需要打包场景变量，为什么要打包场景变量
>>一般是要多次复用的场景会进行打包调用

>Q:group起到什么作用
>>可以起到判断的作用，area.is_in_group("food")

>Q:写move_to这个函数的时候因为是蛇头和蛇身体公用的部分，自己只创建了蛇头，moveto需要执行的对象还有目标位置两个参数一时间不知道该怎么处理了。
>>解决办法就是创建一个snake类，这个类由head和snake——part共同继承，这样两者都可以轻松的调用这个函数了。

>Q:为什么不会出现添加蛇尾，传入位置参数之后，last_position变动导致的错位问题
>>因为游戏执行的基本单位是帧，是按照帧内函数顺序执行的，每一帧中的数据到下一帧之前是不会改变的。贪吃蛇中的_physics_process执行完了如果触发了碰撞检测，也会执行完这个碰撞函数到下一帧再次执行_physics_process，每一帧的状态几乎是静止的。
## 二.流程回顾(设计总结)
### 1.场景构建
   1. 场景的叠加可以通过链接点击加到主场景中，类似于实时显示的分数条ui，spwaner代表了要加在场景上的食物和尾巴。一开始就要出现的都要链接进来，在脚本中的表现形式就是onready，节点的添加。其他的可以通过packedscene添加进来。
   2. packedscene的多复用场景const就可以。如果是ui界面记得添加一个if not 的判断。因为这里没有删除逻辑
   3. export的数据引入，需要用数据的时候，比如说资源脚本中的export，还有spwaner脚本想要用ground节点的限制，相当于是这样加成了子节点
### 2.输入映射
### 3.信号系统
添加信号，就直接在开头  
`signal food_eaten`这样就相当于是添加了一个信号到这个节点上，还可以传递参数`signal health_changed(old_value, new_value)`。信号的发送，`food_eaten.emit()`,触发函数的编写  
1. 可视化连接，用节点和信号列表里面已经预设好的函数
2. 通过connect连接`player.health_changed.connect(_on_health_changed)`这样看一个信号似乎可以触发多个函数
3. 通过组连接多个实例  
   ```func _ready():
    # 将所有敌人添加到 "enemies" 组
    for enemy in get_tree().get_nodes_in_group("enemies"):
        enemy.connect("enemy_died", self, "_on_enemy_died")```
4. 信号总线（还没有试过，之后研究）,

断开连接`some_node.disconnect("some_signal", self, "_some_method")`检查链接状态```if not some_node.is_connected("some_signal", self, "_some_method"):
    some_node.connect("some_signal", self, "_some_method")```
godot的好处就是可以在多次触发信号。  
目前的问题  
>Q:实现函数放在哪里比较好
>>有主要逻辑放在主要逻辑，没有放在节点

>Q:如果是没有挂靠节点的脚本是同样处理的吗。
>>是的

>Q:是一个信号可以触发很多函数，还是一个函数可以被不同地方的信号触发。
>>都可以

### 4.全局设置
在设置中添加类和设置名称。

### 5.删除节点的用法
queue_free和call_deferred  
一般是用call_deferred比较安全这个是下一帧开始的时候删除的。详细细节后面再了解。
### 6.常见节点的使用
贪吃蛇游戏中使用到的节点和用法
1. Node2D
2. Area2D:2D空间区域(可以检测到物体的进入和推出,所以下面必须有碰撞类Collsionshape2D,碰撞类不能单独作为节点,只需要看黄色的提示就足够了)仅是检测碰撞而不产生其他的效果就可以用这个节点,完美的适配贪吃蛇的头部.
3. mark2D:用于设置边界检测
4. TileLayer的使用。可以用tileset里面的参数改变基本的地图大小，然后再导入图集就可以看到按照参数的大小切割的图块。
### 7.导出变量的使用
@export函数。添加新的参数节点到类里面，godot里面有对应得可视化，在侧边栏可以相应得调整。
### 8.构建用户界面基本操作
1. Canvaslayer 控制渲染层级（可以用canvaslayer来控制游戏的界面展示，用层级来组织游戏）
2. panel 界面白板
3. 各式各样的container
4. label
5. button  
   
ui界面和其他子场景相同，可以通过packedscene得形式添加到主场景中，一般是const。信号也是同样使用。
### 9.内置的暂停
自带的功能进入和推出  ```func _notification(what: int) -> void:
	match what:
		NOTIFICATION_ENTER_TREE:
			get_tree().paused=true
		NOTIFICATION_EXIT_TREE:
			get_tree().paused=false```
注意process的使用
### 10.场景转换（get_tree）
   1. game_over的restart按钮想要重启gamepaly的场景，gettree然后reload
   2. 退出所有场景gettree然后quit
   3. 暂停场景（记得使用what，focus_out等判断），退出的时候要记得重新修改pause状态，场景内的按钮还想用的话要记得把node下的模式给改了。
   4. 场景转换不需要实例化要转换的场景，直接change_scene_to_packed
   5. 需要打包的一般是要getaparent然后addchildren，如果已经是parent就直接addchildren

### 11.内存部分
1.创建一个资源类（一般放在data目录下面）extends Resource  
多记忆相关函数
```class_name SaveData extends Resource
   #意思是这个变量是可以被其他用的，resource需要使用export来保证自己得变量可用，node则不需要。
   @export var higt_score:int=0
   #tres为后缀的都是资源类型文件。地址是以string类型保存的。
    const SAVE_PATH:String="user://save_data.tres"

   func save()->void:
   #保存文件
	   ResourceSaver.save(self,SAVE_PATH)
	   pass
	
   #静态函数可以直接调用，有文件就load，没文件就创建
   static  func load_or_create()-> SaveData:
	var res:SaveData
   #godot的文件读写
	if FileAccess.file_exists(SAVE_PATH):
   #加载文件
		res=load(SAVE_PATH) as SaveData
	else:
		res=SaveData.new()
	return  res
   ```
2. 添加到全局，新建一个savedata实例。
```func _ready() -> void:
	save_data=SaveData.load_or_create()
```
3. 在开始场景里面调用global里面得savedata，并且可以直接访问里面得high_score  
   ```Global.save_data.higt_score=n
		Global.save_data.save()```
### 12.代码规范（调整代码顺序）
1. const和preload部分
2. exports变量
3. onready变量
4. 普通变量
5. 任何需要访问其他节点或其属性的初始化操作，都必须放在 _ready() 函数内部
### 13.getter和setter
### 14.命名规范
   1. 类，节点名--首字母大写
   2. 变量，函数，信号名--小写和短横线
   3. 常量--大写和短横线


## 三.视频总结
- autoloads
- signals
- getters&setters
- inheritanc
- ui&layouts
- saving data
- player movement
- runtime instantiatiin
- godot_notifications
- pausing your game 
- export&onready vars

### 流程部分
1. 先进行基本移动逻辑的代码编写
   输入（input）控制物体移动  
   移动需要有速度，还有时间和方向。  
   >Q:这个在godot里面是怎么处理的呢  
   >>1. 初始化一个方向参数Vector2
   `var move_dir:Vector2=Vector2.right`
   >>2. 制作移动物体，类型选择。需要检测碰撞所以选中了area2D，拖入场景
   >>3. 编写移动物体的代码，引入蛇头  
   `@onready var head=%head as head`%标志意味着唯一访问标识，as Head判断了类型必须得是Head型的才行
   >>4. 继续添加移动所需要的参数  ```var time_between_move=1000.0  
        var time_science_last_move:float=0  
        var speed:float=1000```
   >>5. 闹钟移动vs直接移动
   经典版贪吃蛇用的是闹钟移动。用delta控制移动的时间  
   time_since_last_move（delta*speed）>time_between_move。  
   速度不会直接起作用的，屏幕上物体的移动是通过坐标的改变，闹钟式移动就是通过改变移动的间隔来控制速度的。speed值越大就能越快的进行移动。
   >delta的概念
   delta是每帧经过的时间，每帧都要完整的执行process以及触发的函数，每帧用时不同 ，每一帧使用的参数delta都是上一帧的用时。时间点: 0.000s - 帧1开始执行
        0.010s - 帧1执行结束 → 测得帧0到帧1的耗时 = 0.010s
        0.010s - 帧2开始，调用 _process(0.010) ← 使用上一帧的耗时
        0.043s - 帧2执行结束 → 测得帧1到帧2的耗时 = 0.033s  
        0.043s - 帧3开始，调用 _process(0.033) ← 使用上一帧的耗时
        0.060s - 帧3执行结束 → 测得帧2到帧3的耗时 = 0.017s
   _physics_process的delta是固定的，执行的次数是用累积时间来进行判断  
   ```var physics_accumulator: float = 0.0
      const PHYSICS_STEP: float = 1.0 / 60.0
      func main_loop():
      # 1. 测量上一帧的耗时
      var previous_frame_delta = get_measured_frame_time()
    
      # 2. 调用_process（使用上一帧的耗时）
      _process(previous_frame_delta)
    
      # 3. 将上一帧的时间累积到物理accumulator
      physics_accumulator += previous_frame_delta
    
      # 4. 处理积压的物理帧（使用累积的过去时间）
      while physics_accumulator >= PHYSICS_STEP:
        _physics_process(PHYSICS_STEP)  # 固定时间步长
        physics_accumulator -= PHYSICS_STEP
    
      # 5. 开始测量本帧的耗时（用于下一帧）
      start_frame_time_measurement()```

执行过程如下帧0: 耗时 = 0.010s
帧1: 耗时 = 0.033s  
帧2: 耗时 = 0.017s

执行过程：
开始帧1:
  测得帧0耗时 = 0.010s
  调用 _process(0.010)
  accumulator += 0.010 → 0.010 < 0.0167 → 0次物理调用

开始帧2:
  测得帧1耗时 = 0.033s  
  调用 _process(0.033)
  accumulator += 0.033 → 0.043 ≥ 0.0167 → 2次物理调用
  accumulator = 0.043 - 0.0167 - 0.0167 = 0.0096

开始帧3:
  测得帧2耗时 = 0.017s
  调用 _process(0.017) 
  accumulator += 0.017 → 0.0266 ≥ 0.0167 → 1次物理调用
  accumulator = 0.0266 - 0.0167 = 0.0099

   1. 帧开始
   2. 处理输入事件
   3. 调用游戏逻辑process
   4. 处理消息队列（信号就在其中）
   5. 调用物理逻辑_physics_process
   6. 执行物理模拟
   7. 再次处理消息队列（物理产生的信号） 在物理模拟过程中新触发的信号。比如说移动贪吃蛇了之后，产生了碰撞，这个触发的信号要在这一帧结束
   8. 渲染绘制
   9. 帧结束
   
   ，帧数越少，delta就越大，这样可以保证每个机子上面的移动速度是相同的，因为游戏中的刷新是按帧刷新的。每一帧都会执行以下函数。```func _physics_process(delta: float) -> void:
	time_since_last_move+=delta*speed
	if  time_since_last_move>=time_between_moves:
		update_snake()
		time_since_last_move=0
	pass```  
    满一秒之后就会跑完速度长，把不稳定的帧率转换为了稳定的每秒。每一秒1000个像素点。