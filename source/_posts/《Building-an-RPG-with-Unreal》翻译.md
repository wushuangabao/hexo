---
title: 《Building an RPG with Unreal》翻译
tags:
  - 阅读
  - UE4
  - 翻译
categories:
  - 游戏引擎
toc: false
date: 2021-04-02 16:02:58
---

本文是对《Building an RPG with Unreal 4.x》英文书的翻译。因为偷懒，删去了我认为没必要的内容。

<!--more-->

## 开始在Unreal中设计RPG

在写下代码前，很有必要弄清你想要做什么样的RPG、游戏怎么玩、是实时战斗还是回合制、玩家关注的属性（stats）有哪些。

### 游戏设计的工具

有很多种工具能帮助我们完成设计文档。其中特别值得注意的是Google工具套件，它们有多种应用而且免费。

- Google Drive
一种云端文件存储系统。还可以设置他人的修改权限。

- Google Docs
与Google Drive集成的在线文字处理应用，具有实时合作编辑、评论、聊天功能。

- Google Spreadsheets电子表格
同样与Google Drive集成，提供Excel风格的界面。

- 笔和纸
当你灵光一闪时，也许应该马上把它记到纸上。

### 设计和概念阶段

几乎所有的游戏都是从一堆粗糙的概念或设计文档中开始诞生的。

设计文档的目的是描述游戏的方方面面。在RPG中，它需要描述玩家如何在游戏世界中移动，如何与NPC和敌人交互，如何战斗，等等。设计文档是构建游戏代码的基石。

- 概念
例如，我们设想本书中的游戏是一款线性回合制的RPG冒险。这是一个很粗犷的概念，但是完全没问题——只要能从此开始充实和创建设计文档就行了。

- 设计
设计文档以前面的粗糙概念为基础，目的是细化概念并说清运行机制。你的设计文档要让任何人能明白你的游戏会是什么样的，它是如何运作的。设计文档的一个最大功能就是确保开发团队的认知一致。

### 现存角色扮演游戏的tropes

尽管RPG的类型多到爆，但是仍然有大量共通的主题。

#### Stats和养成（Progress）

这都不用多提，每个RPG都有这些基础概念。

Stats，或者说Statistics，是一些支配游戏的战斗的数字。Stats可以有很多种，但一般都包括最大生命、最大MP、力量（攻击）、防御等。

随着游戏的进行，玩家的这些stats也会提高。提高的途径是多样的，但大部分游戏都用战斗获得的经验点（XP）来实现——当获得足够的XP后，角色的等级会提升，stats也会增加。

#### 职阶（Classes）

在RPG中有classes很正常。一个class有丰富的内涵，但它通常用来定义一个角色有什么能力，以及角色会如何养成。
举个例子，战士职阶可能意味着角色能够挥剑，并且在升级时专注于提升攻击力和防御力。

#### 特殊能力

很少有角色扮演游戏会没有魔法咒语或某种特殊能力。

一般而言，角色会有某种类型的魔力值，当他们使用特殊技能时被消耗掉。当角色没有足够的魔力时无法施放技能（魔力也会被称作法力、耐力、能量等融入游戏剧情的东西）。

### RPG设计概览

现在我们来看看我们将在本书中开发的RPG的设计。

#### 设定

这个游戏设在一座塔里。玩家们在前进到塔顶的过程中一路与敌人战斗，最终遇到大恶人。游戏的目标是战胜大恶人。

#### 探索

非战斗时，玩家们以等距透视视角探索世界，就像《暗黑破坏神》那样。这个视角下，玩家可以和NPC及道具交互，也可以暂停游戏来管理队伍成员、仓库（inventory）和装备。

#### 对话

当和NPC或道具交互时，可能会触发对话。这游戏中的对话主要依赖文本。对话框可以是线条，玩家简单地按个按钮来前进到下一个对话页，或者在一张选项列表中做选择。每一个选项将推进至不同的对话页。

#### 购物

对话也可以触发商店UI。例如，店老板可以问玩家是否想购买物品。如果玩家选择“是”。就会显示一个商店UI。在商店中，玩具架可以从NPC处购买物品。

#### 金币

战斗中击败怪物会获得金币。这里金币被看做是怪物掉落的一种。

#### The pause screen

游戏暂停时，玩家可以做以下事情：
- 查看队伍成员和他们的状态（生命，魔法，等级，效果等）；
- 查看每个队友学会的技能；
- 查看当前携带的金币总数；
- 浏览仓库，对成员使用物品（药剂，酒类等）；
- 管理每个队员的装备物品（武器，盔甲等）。

#### 队友

玩家有个队伍成员表。表里是当前在玩家队伍里的所有角色。玩家可能在塔里遇到一个角色加入队伍并参战。

#### 装备

玩家队伍中的每个角色都有以下武器槽：
- 盔甲：通常增加防御
- 武器：通常提升攻击力（参与到攻击公式计算中）

#### 职阶

角色的职阶定义了以下要素：
- 升级的经验曲线
- 升级后增加的属性值
- 升级后能学哪种技能

本教学只实现一个玩家角色和职阶。

##### Soldier
战士职阶专注于提升攻击、最大HP和幸运。此外，特技主要是对敌人造成巨量伤害。

#### 战斗

探索游戏世界时会触发随机事件。战斗事件会在过场景和剧情时触发。触发后会转换到战斗区域视图。

战斗人员分为两方：敌人和玩家。双方各处于战斗区域一端，面对面排成一条线。先是玩家的回合，再是敌人的回合。一回合战斗分为两阶段：决策和行动。

首先，每个参战者选择自己的行为。可以攻击一个敌人或者使用一个技能。所有参战者决策完毕之后，按顺序执行动作。多数动作都有一个特定目标。如果执行动作时目标无效，会选择下一个目标，要是没有可选目标，动作执行失败，角色什么也不做。

直到所有敌人或玩家队伍角色死亡前，回合会一直循环。如果所有敌人死亡，玩家队伍成员就会获得经验奖励，还会从被击败的敌人身上劫获奖励（通常是随机数额的金币）。如果所有玩家角色死亡，那么游戏失败。

#### 战斗stats

每一个参战者都拥有以下属性：
- 生命点数（HP）：代表角色能承受的伤害多少。HP达到0时角色死亡。角色存活时可以通过物品或技能补充HP，死亡后不行。
- 最大生命：任何时候角色HP所能达到的最大值。治疗物品或技能也不能令HP超出这个上限。在角色升级时会提高。特定的装备也能临时性提高最大生命。
- 魔法点数（MP）：施放技能时被消耗。可通过物品补充。
- 最大魔法值
- 攻击力：角色攻击一个敌人时造成的伤害量。武器会给攻击附加额外攻击力。伤害计算公式为`max(attacker.ATK-enemy.DEF,0)+attacker.weapon.ATK`。
- 防御：减少角色受到的伤害。
- 幸运：影响角色的暴击率（暴击造成正常伤害的2倍）。幸运值范围是0到100，均匀地对应0%到25%的暴击率。

#### 战斗行为

分为两部分：（普通）攻击和技能。

##### 攻击
每个角色都有攻击的技能，消耗0MP，位于行动菜单的第一个选项。
一般攻击的目标为单个敌人，伤害公式在之前的攻击力描述中给出了。

##### 技能
除了攻击以外的技能都会消耗MP并造成不同效果。技能的目标分为不同的类型：
- 单个敌人
- 所有敌人
- 单个友方
- 全体友方

技能可以治疗、复活、移除一些特殊状态、召唤临时同伴、临时增益等。然而技能无法增加MP。

#### 战斗结束后
##### 战利品
##### 经验
死亡的队友无法获得经验。经验会平均分配给角色，均分后如果是小数会向上取整。

#### 经验与等级
每升1级需要的经验值为：
```
f(x) = (xa) + c
```
这里，x是当前等级，a是比1大的数，c是常数（正值）。
游戏中角色的最大等级是50级。升级时的属性增益和可学的技能取决于职阶。

### 选择合适的公式
好的数值公式使游戏变得平衡、有趣，但是没有万能的银弹。

## Unreal中的编程和数据

我们已经有了设计，工作由此展开，可以开始开发游戏了。

这一章涵盖了以下主题：
- 下载Unreal
- 为Unreal配置Visual Studio
- 配置一个新Unreal项目
- 新建一个C++类
- 创建蓝图和蓝图视图
- 用Data Table导入数据表中的数据

### 下载Unreal

### 下载Visual Studio

### 针对Unreal配置Visual Studio
#### 增加Solution Platforms下拉列表
#### 关闭Error List tab

### 配置一个新Unreal项目

我们已经下载安装了Unreal和Visual Studio，开始创建一个游戏项目。
本书用的引擎版本是4.7.6。创建新项目的步骤如下：
1. 加载虚幻引擎后，点击New Project选项卡，然后点击C++选项卡并选择Basic Code。
2. 选择一个项目存放的路径，为项目起一个名字（这里叫做RPG）。

打开新建的项目，编辑器加载完成后，点击File|Open Visual Studio。

### 创建一个新C++类

步骤如下：
1. 在Unreal编辑器中，点击File|Add Code To Project，选择Actor作为父类。
2. 输入新类的名字，比如`MyNewActor`，点击Create Class。在它把文件加入项目后，点Yes在VS中打开MyNewActor.h。
3. 我们想让actor在游戏开始时输出一条信息。为此将使用`BeginPlay`事件。
4. 在MyNewActor.h文件的`GENERATED_BODY()`后面有一行：`public: virtual void BeginPlay();`
5. 在MyNewActor.cpp文件的`void AnyNewActor::BeginPlay()`函数中加入代码，输出Hello,world!
6. 回到编辑器，点击主工具栏中的Compile按钮。
7. 我们需要将编译好的类加入场景。搜索MyNewActor并将其拖入场景视窗。它不可见也不能被点击，但你能在Scene/World Outliner面板的底部看见MyNewActor1已经在了。
8. 点击Play按钮进行测试。你应该能看见一条黄色的“Hello,world!”消息输出到了控制台。

第5步中的代码如下：
```
void AnyNewActor::BeginPlay()
{
    Super::BeginPlay();
    UE_LOG( LogTemp, Warning, TEXT("Hello, world!") );
}
```

### 蓝图

Unreal中的蓝图是基于C++的、专为Unreal构建的、可视化的脚本语言。
……

#### 创建一个新蓝图
这几节都过于简单，省略……

#### 向场景中加入一个蓝图
#### Actor类的蓝图

### 使用Data Table导入电子表格的数据

首先，你得确保你的电子表格符合某种格式；同时，你写个包含表格中一行数据的C++ struct。然后，导出CSV文件（到Unreal）并以C++ struct为这个文件的数据类型。

#### 电子表格的格式

电子表格必须符合一些简单的规则，以确保能被正确导出到Unreal。

- 第一个单元格必须留空。
- 第一行要包含字段的名称，它们要和你的C++ struct中的变量名一致。
- 第一列包含每条记录的lookup key（查找键）。每行的键不能重复。
- 其他列包含每个变量的值。

#### 一个电子表格例子

| |SomeNumber|SomeString|
|-|-|-|
|1|10|Hello, world|
|2|15|Hello, again|
|3|30|Hello!|

如果你用的是Google Spreadsheet，你须要点击File|Download as|Comma-separated values(.csv)，导出数据到CSV。大多电子表应用都能导出CSV格式。

在把CSV导入到Unreal之前，我们要先为其创建C++ struct。

#### Data Table的结构

就像之前创建actor类一样，我们创建一个新的actor类，名为TestCustomData。这个类之所以继承自Actor仅仅是为了让Unreal生成一些默认代码。

接着打开TestCustomData.h文件并把代码整体替换为下面的：
```
#pragma once
#include "TestCustomData.generated.h"

USTRUCT(BlueprintType)
struct FTestCustomData : public FTableRowBase
{
    GENERATED_USTRUCT_BODY()

    UPROPERTY( BlueprintReadOnly, Category = "TestCustomData" )
    int32 SomeNumber;

    UPROPERTY( BlueprintReadOnly, Category = "TestCustomData" )
    FString SomeString;
};
```

成员变量名要与电子表中的header单元格一致，这很重要，虚幻引擎据此将对应的表格字段与struct匹配。

然后把TestCustomData.cpp文件中除了include语句外的代码全删除。回到Unreal编辑器并点击Compile。

#### 导入电子表格

将CSV文件拖入Content Browser面板，出现一个弹窗让你选择如何导入数据、这是什么类型的数据。分别选择Data Table和TestCustomData。点击OK就会导入CSV文件了。

#### 查询电子表格

你可以通过名称查找表中特定的行。我们将把这个查询能力添加到自定义类MyNewActor中。我们需要向蓝图暴露一个Data Table让我们的actor来用。

首先，把下面的代码添加到MyNewActor.h文件的`GENERATED_BODY()`后面：
```
public:
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "My New Actor")
    UDataTable* DataTable;
```

接下来，fetch一行记录并输出它的SomeString字段。在MyNewActor.cpp文件中的`BeginPlay`函数末尾添加代码：
```
if( DataTable != NULL )
{
    FTestCustomData* row = DataTable->FindRow<FTestCustomData>( TEXT( "2" ), TEXT(" LookupTestCustomData" ) );  // 2号记录
    FString someString = row->SomeString;
    UE_LOG( LogTemp, Warning, TEXT( "%s" ), *someString );
}
```
同时在MyNewActor.cpp顶端添加`#include TestCustomData.h`。

在编辑器中编译代码。然后打开你从MyNewActor继承来的蓝图，在Data Table字段你可以选择已导入的CSV文件。

编译、保存蓝图，然后按下Play来测试。




## 探索和战斗

本章，我们将制作一个可在世界移动的游戏角色，定义游戏数据，制作一个基础战斗系统的原型。我们将涉及以下主题：
- 创建player pawn
- 定义角色，职阶和敌人
- 保持探测队友
- 创建基本的回合制战斗引擎
- 触发游戏失败画面


### 创建player pawn

player分为两部分：有一个pawn负责处理移动、物理和渲染。然后，Player Controller负责将玩家的输入转化为pawn的行为。

此外，我们将通过让pawn实现一个`IControllableCharacter`接口来实现一层分离。这并不是必须的，但是确实有助于让不同的类之间减少信息共享（例如，任何其他actor都可以便利地实现`IControllableCharacter`接口，然后我们的控制器可以无缝衔接地控制这些新的actor）。

#### 接口

我们新建一个继承自Actor的类，名叫`ControllableCharacter`。在Unreal生成代码文件之后，打开ControllableCharacter.h，将它改为下面这样：
```
#pragma once
#include "Object.h"
#include "ControllableCharacter.generated.h"

UINTERFACE()
class RPG_API UControllableCharacter : public UInterface
{
    GENERATED_UINTERFACE_BODY()
};

class RPG_API IControllableCharacter
{
    GENERATED_IINTERFACE_BODY()
    virtual void MoveVertical( float Value );
    virtual void MoveHorizontal( float Value );
};
```

在Unreal中，**接口有两个部分：`UInterface`类和真正的接口类**。这两个类和Unreal的宏系统一起，使你能实现适用`Cast`宏的接口类（`Cast`宏能把一个actor转换为它所实现的接口）。

`UInterface`类用U做前缀，只包含一行代码。

打开ControllableCharacter.cpp并改成这样：
```
UControllableCharacter::UControllableCharacter( const class FObjectInitializer& ObjectInitializer ) : Super( ObjectInitializer )
{}

void IControllableCharacter::MoveVertical( float Value )
{}

void IControllableCharacter::MoveHorizontal( float Value )
{}
```
这里我们定义了`UControllableCharacter`类的构造函数，和成员函数的默认实现。

#### PlayerController

创建一个新类并选择PlayerController作为父类，取名RPGPlayerController。在生成的头文件中加入：
```
protected:
    void MoveVertical( float Value ); //监听玩家输入
    void MoveHorizontal( float Value ); //监听玩家输入
    virtual void SetupInputComponent() override;
```

在RPGPlayerController.cpp中添加以下代码：
```
void ARPGPlayerController::MoveVertical( float Value )
{
    IControllableCharacter* pawn = Cast<IControllableCharacter>( GetPawn() );
    if( pawn != NULL )
        pawn->MoveVertical( Value );
}

void ARPGPlayerController::MoveHorizontal( float Value )
{
    IControllableCharacter* pawn = Cast<IControllableCharacter>( GetPawn() );
    if( pawn != NULL )
        pawn->MoveHorizontal( Value );
}

void ARPGPlayerController::SetupInputComponent()
{
    if( InputComponent == NULL )
    {
        InputComponent = ConstructObject<UInputComponent>( UInputComponent::StaticClass(), this, TEXT( "PC_InputComponent0" ) );
        InputComponent->RegisterComponent();
    }
    // 绑定输入映射
    InputComponent->BindAxis( "MoveVertical", this, &ARPGPlayerController::MoveVertical );
    InputComponent->BindAxis( "MoveHorizontal", this, &ARPGPlayerController::MoveHorizontal );
    this->bShowMouseCursor = true; // 不隐藏鼠标
} 
```

#### Pawn

建一个新类，名为RPGCharacter，选择Character作为父类。打开RPGCharacter.h更改类的定义为：
```
UCLASS()
class RPG_API ARPGCharacter : public ACharacter, public IControllableCharacter
{
    GENERATED_BODY()
    ARPGCharacter( const class FObjectInitializer& ObjectInitializer );

public:
    virtual void MoveVertical( float Value );
    virtual void MoveHorizontal( float Value );
};
```
这个类实现了`IControllableCharacter`接口，所以也要实现接口中定义的两个成员方法。

接下来，在cpp文件中添加如下代码：
```
ARPGCharacter::ARPGCharacter( const class FObjectInitializer& ObjectInitializer ) : Super( ObjectInitializer )
{
    bUseControllerRotationYaw = false;
    GetCharacterMovement()->bOrientRotationToMovement = true;
    GetCharacterMovement()->RotationRate = FRotator( 0.0f, 0.0f, 540.0f ); //默认转向速度
    GetCharacterMovement()->MaxWalkSpeed = 400.0f; //默认最大步行（奔跑）速度
}

void ARPGCharacter::MoveVertical( float Value )
{
    if( Controller != NULL && Value != 0.0f )
    {
        const FVector moveDir = FVector( 1, 0, 0 );
        AddMovementInput( moveDir, Value );
    }
}

void ARPGCharacter::MoveHorizontal( float Value )
{
    if( Controller != NULL && Value != 0.0f )
    {
        const FVector moveDir = FVector( 0, 1, 0 );
        AddMovementInput( moveDir, Value );
    }
}
```

#### GameMode类

新建一个类并选择GameMode作为父类，命名为RPGGameMode。

打开RPGGameMode.h并修改类的定义为：
```
UCLASS()
class RPG_API ARPGGameMode : public AGameMode
{
    GENERATED_BODY()
    ARPGGameMode( const class FObjectInitializer& ObjectInitializer );
};
```

然后在RPGGameMode.cpp中实现这个构造函数：
```
#include "RPGPlayerController.h"
#include "RPGCharacter.h"

ARPGGameMode::ARPGGameMode( const class FObjectInitializer& ObjectInitializer ) : Super( ObjectInitializer )
{
    // 设置默认类
    PlayerControllerClass = ARPGPlayerController::StaticClass();
    DefaultPawnClass = ARPGCharacter::StaticClass();
}
```

编译完后，将这个新的game mode设为默认的游戏模式。前往Edit|Project Settings|Default Modes中设置，在Default GameMode下拉框中选择RPGGameMode。

然而，我们并不想直接使用这个类。我们建一个蓝图，将game mode的属性暴露到蓝图中，就可以方便修改。这个蓝图名为DefaultRPGGameMode，继承自RPGGameMode类。

在这个蓝图中，我们可以修改很多默认设置，包括Default Pawn Class，HUD Class，PlayerController Class等。

为了看见Pawn，我们要给它添加蒙皮网格，让摄像机跟随。

#### 添加skinned mesh蒙皮网格

基于Third Person案例建一个新项目，然后把下列资源迁移到RPG项目中：
- HeroTPP
- HeroTPP_AnimBlueprint
- HeroTPP_Skeleton
- IdleRun_TPP

迁移步骤：
1. 全选这些资源。
2. 右键单击其中任意一个，选择Migrate（迁移），点OK。
3. 选择要导入的RPG项目中的目录，点OK。

有了HeroTPP模型，可以为我们的Pawn创建新蓝图了。新建RPGCharacter的子蓝图，名为FieldPlayer。

展开Mesh，选择HeroTPP作为pawn的skeletal mesh。展开Animation，选择HeroTPP_AnimBlueprint作为使用的动画蓝图。

最后，打开game mode蓝图，把pawn改为FieldPlayer。现在你的角色已经可以被看见了，而且移动时会播放奔跑动画。但是摄像机还需要设置。

#### 创建Camera组件

新建一个CameraComponent的子类，名为RPGCameraComponent。然后，打开RPGCameraComponent.h更改类定义为：
```
UCLASS( meta = ( BlueprintSpawnableComponent ) ) //使摄像机组件可以添加到pawn蓝图中
class RPG_API URPGCameraComponent : public UCameraComponent
{
    GENERATED_BODY()
public:
    UPROPERTY( EditAnywhere, BlueprintReadWrite, Category = CameraProperties)
    float CameraPitch;

    UPROPERTY( EditAnywhere, BlueprintReadWrite, Category = CameraProperties)
    float CameraDistance;

    // 当游戏视角切换到挂载这个摄像机组件的pawn上时，调用此方法。
    virtual void GetCameraView( float DeltaTime, FMinimalViewInfo& DesiredView ) override;
};
```

打开RPGCameraComponent.cpp添加以下代码：
```
void URPGCameraComponent::GetCameraView( float DeltaTime, FMinimalViewInfo& DesiredView )
{
    UCameraComponent::GetCameraView( DeltaTime, DesiredView );
    DesiredView.Rotation = FRotator( CameraPitch, 0.0f, 0.0f );
    if( APawn* OwningPawn = Cast<APawn>( GetOwner() ) )
    {
        FVector location = OwningPawn->GetActorLocation();
        location -= DesiredView.Rotation.Vector() * CameraDistance;
        DesiredView.Location = location;
    }
}
```

打开之前创建的pawn蓝图，添加RPGCamera组件。设置组件的属性，pitch为-50，distance为600.


### 定义角色和敌人
#### Classes
#### 角色
#### 敌人

### 队友
#### GameInstance类

### 回合制战斗
#### 表演actions
#### 做决策
#### 目标选择
#### 计算伤害
#### 使用UMG的战斗UI
#### 以UI驱动做决策
#### 创建游戏失败的画面

### 小结

## 暂停菜单的构架

### UMG暂停画面的初始化
### UMG背景色
### UMG文本
### UMG按钮
### UMG inventory（库存）子菜单
### UMG装备子菜单
### 按键绑定
### 按钮编程
### 小结

## Bridging角色的Statistics

### 获取角色数据
### 获取player instance
### 显示stats
### 小结

## NPC和对话

### 创建NPC角色蓝图
### 和NPC交互
### 对话box设置
### 创建一个NPC欢迎的box
### 添加NPC talk的box
### 小结

## 金币，物品和商店

### 金币instances的setting和getting
### 物品数据
### 商店画面构架
### 物品按钮构架
### 链接到物品数据
### 小结

## Inventory（库存）Population和物品使用

### 创建FieldPlayer布尔值
### 判断库存画面是否开启
### 库存和商品在逻辑上的区别
### 关闭库存画面
### 购买物品
### 使用物品
### 小结

## 装备

## 等级，技能和存档