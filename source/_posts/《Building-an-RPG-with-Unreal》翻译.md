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

我们已经会使用Data Table导入数据，也确定了什么属性会影响战斗、如何影响。现在将综合这些，来定义我们游戏的角色、职阶和敌人触发器（enemy encounter）。

#### Classes

根据第一章，我们的角色有以下属性：
- 生命
- 最大生命
- 魔法
- 最大魔法
- 攻击力
- 防御
- 幸运

我们忽视掉生命和魔法，因为他们在游戏中会不停变化，而其他值是随着职阶确定而固定的。我们需要存储50级（最大等级）时的这些属性。此外，角色初始会自带一些技能，升级后会学一些技能，

这些都在角色职阶电子表中定义，所以我们的角色职阶schema如下：
- 职阶名称（string）
- 初始最大HP（int）
- 50级最大HP（int）
- 初始最大MP（int）
- 50级最大MP（int）
- 初始攻击（int）
- 50级最大攻击（int）
- 初始防御（int）
- 50级防御（int）
- 初始幸运（int）
- 50级幸运（int）
- 初始的技能（string数组）
- 可学的技能（string数组）
- 可学技能的等级（int数组）

在游戏行业，你需要考虑写一个自己的工具来帮助管理数据，以减少人所犯的错误。不过这不在本书的讨论范围内。

我们先继承Actor（事实上继承谁都无所谓）来创建一个新类，名为`FCharacterClassInfo`。类定义如下：
```
USTRUCT( BlueprintType )
struct FCharacterClassInfo : public FTableRowBase
{
    GENERATED_USTRUCT_BODY()

    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    FString Class_Name;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    int32 StartMHP;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    int32 StartMMP;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    int32 StartATK;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    int32 StartDEF;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    int32 StartLuck;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    int32 EndMHP;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    int32 EndMMP;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    int32 EndATK;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    int32 EndDEF;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    int32 EndLuck;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    TArray<FString> StartingAbilities;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    TArray<FString> LearnedAbilities;
    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "ClassInfo" )
    TArray<int32> LearnedAbilityLevels;
};
```

编译完成后，以Character Class Info为基础创建一个您的Data Table（在Content Browser右键，选择Create Advanced Asset|Miscellaneous|Data Table），然后双击打开。

> 如果此时Row Editor面板为空，你可能须要重启Unreal编辑器。

我们在Row Editor面板中新增一条记录。Rename字段中输入S1（我们在其他Data Table中用它来访问角色职阶信息），按回车添加新记录，属性设置如下：
- Class name: Soldier
- Start MHP: 100
- Start MMP: 100
- Start ATK: 5
- Start DEF: 0
- Start Luck: 0
- End MHP: 800
- End MMP: 500
- End ATK: 20
- End DEF: 20
- End Luck: 10
- Starting abilities: (leave empty for now)
- Learned abilities: (leave empty for now)
- Learned ability levels: (leave empty for now)

#### 角色

现在用名字和职阶就能定义角色。

头文件FCharacter.h中角色数据结构如下：
```
USTRUCT(BlueprintType)
struct FCharacterInfo : public FTableRowBase
{
    GENERATED_USTRUCT_BODY()

    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "CharacterInfo" )
    FString Character_Name;

    UPROPERTY( BlueprintReadOnly, EditAnywhere, Category = "CharacterInfo" )
    FString Class_ID;
};
```

编译后新建基于CharacterInfo的DataTable。新增一条记录，在class ID栏中填S1（我们之前定义的Soldier职阶）。

#### 敌人

因为敌人没有经验、不升级，我们就不区分角色和职阶两种类型的信息了，而是统合成一张表。此外敌人也不消耗MP。因此，敌人的数据有如下属性：
- Enemy name (string array)
- MHP (integer)
- ATK (integer)
- DEF (integer)
- Luck (integer)
- Abilities (string array)

据此你应该知道怎么定义EnemyInfo类了，不过我们还是来看看吧：
```
USTRUCT( BlueprintType )
struct FEnemyInfo : public FTableRowBase
{
    GENERATED_USTRUCT_BODY()

    UPROPERTY( BlueprintReadWrite, EditAnywhere, Category = "EnemyInfo" )
    FString EnemyName;
    UPROPERTY( BlueprintReadOnly, EditAnywhere, Category = "EnemyInfo" )
    int32 MHP;
    UPROPERTY( BlueprintReadOnly, EditAnywhere, Category = "EnemyInfo" )
    int32 ATK;
    UPROPERTY( BlueprintReadOnly, EditAnywhere, Category = "EnemyInfo" )
    int32 DEF;
    UPROPERTY( BlueprintReadOnly, EditAnywhere, Category = "EnemyInfo" )
    int32 Luck;
    UPROPERTY( BlueprintReadOnly, EditAnywhere, Category = "EnemyInfo" )
    TArray<FString> Abilities;
};
```

编译后，新建EnemyInfo类的Data Table，加一条名为S1的记录，属性如下：
- Enemy name: Goblin
- MHP: 20
- ATK: 5
- DEF: 0
- Luck: 0
- Abilities: (leave empty for now)

现在我们有了一个角色，这个角色的职阶，以及单一的敌人。接下来，我们将检测active party中有哪些角色以及他们目前的属性怎么样。

### 队友

在我们检测队友前，我们需要有一种检测某角色当前状态（如有多少HP、装备如何）的方法。

为此，我们要创建一个`GameCharacter`类。父类选择Object。头文件如下：
```
#include "Data/FCharacterInfo.h"
#include "Data/FCharacterClassInfo.h"
#include "GameCharacter.generated.h"
UCLASS( BlueprintType )
class RPG_API UGameCharacter : public UObject
{
    GENERATED_BODY()

public:
    FCharacterClassInfo* ClassInfo;
    UPROPERTY( EditAnywhere, BlueprintReadWrite, Category = CharacterInfo )
    FString CharacterName;
    UPROPERTY( EditAnywhere, BlueprintReadWrite, Category = CharacterInfo )
    int32 MHP;
    UPROPERTY( EditAnywhere, BlueprintReadWrite, Category = CharacterInfo )
    int32 MMP;
    UPROPERTY( EditAnywhere, BlueprintReadWrite, Category = CharacterInfo )
    int32 HP;
    UPROPERTY( EditAnywhere, BlueprintReadWrite, Category = CharacterInfo )
    int32 MP;
    UPROPERTY( EditAnywhere, BlueprintReadWrite, Category = CharacterInfo )
    int32 ATK;
    UPROPERTY( EditAnywhere, BlueprintReadWrite, Category = CharacterInfo )
    int32 DEF;
    UPROPERTY( EditAnywhere, BlueprintReadWrite, Category = CharacterInfo )
    int32 LUCK;

    static UGameCharacter* CreateGameCharacter( FCharacterInfo* characterInfo, UObject* outer );

    void BeginDestroy() override;
};
```
现在我们可以随时检测角色的这些（成员）属性了，之后做战斗系统时会增加别的成员。

cpp文件像这样：
```
UGameCharacter::UGameCharacter( const class FObjectInitializer& objectInitializer ) : Super( objectInitializer )
{}

UGameCharacter* UGameCharacter::CreateGameCharacter( FCharacterInfo* characterInfo, UObject* outer )
{
    UGameCharacter* character = NewObject<UGameCharacter>( outer );

    // locate character classes asset
    UDataTable* characterClasses = Cast<UDataTable>( StaticLoadObject( UDataTable::StaticClass(), NULL, TEXT( "DataTable'/Game/Data/CharacterClasses.CharacterClasses'" ) ) );

    if( characterClasses == NULL )
    {
        UE_LOG( LogTemp, Error, TEXT( "Character classes datatable not found!" ) );
    }
    else
    {
        character->CharacterName = characterInfo->Character_Name;
        FCharacterClassInfo* row = characterClasses->FindRow<FCharacterClassInfo>( *( characterInfo->Class_ID ), TEXT( "LookupCharacterClass" ) );
        character->ClassInfo = row;

        character->MHP = character->ClassInfo->StartMHP;
        character->MMP = character->ClassInfo->StartMMP;
        character->HP = character->MHP;
        character->MP = character->MMP;

        character->ATK = character->ClassInfo->StartATK;
        character->DEF = character->ClassInfo->StartDEF;
        character->LUCK = character->ClassInfo->StartLuck;
    }
    return character;
}

void UGameCharacter::BeginDestroy()
{
    Super::BeginDestroy();
}
```

`CreateGameCharacter`是`UGameCharacter`类的工厂方法，它传入一个`FCharacterInfo`类型的指针（指向Data Table）和一个`Outer`对象（传递到`NewObject`函数），根据路径（这个路径可以通过右键Data Table选择Copy Reference来获取）找到角色职阶的Data Table，定位到某一行，拿到职阶数据，并初始化角色的属性和`CharacterName`字段。

虽然这只是对角色的非常基础性的表示，但目前能应付了。接下来，我们将存一张角色表来表示当前的队伍。

#### GameInstance类

我们已经有了一个GameMode类，似乎这是一个追踪诸如队友和inventory信息的好地方，是吗？

并不，因为GameMode在关卡切换期间不能持续存在！也就是说，除非你将某些信息存储到磁盘中，不然你会在加载一个新area时丢失数据。

GameInstance类就是为了解决此类问题而产生的，它在整个游戏生命周期中持续存在，无视关卡的重载。我们将创建一个新的GameInstance类来保持对队伍成员和inventory的track（追踪、检测）。

以GameInstance为父类新建一个类，取名RPGGameInstance。

在头文件中，我们添加一个`UGameCharacter`类型指针的`TArray`，一个bool变量来记游戏是否已经初始化，和一个`Init`函数：
```
#include "GameCharacter.h"

UCLASS()
class RPG_API URPGGameInstance : public UGameInstance
{
    GENERATED_BODY()

    URPGGameInstance( const class FObjectInitializer& ObjectInitializer );

public:
    TArray<UGameCharacter*> PartyMembers;

protected:
    bool isInitialized;

public:
    void Init();
};
```

这个`Init`函数中，我们加入一个默认的队伍成员，然后设`isInitialized`为true：
```
void URPGGameInstance::Init()
{
    if( this->isInitialized ) return;
    this->isInitialized = true;

    // locate characters asset
    UDataTable* characters = Cast<UDataTable>( StaticLoadObject( UDataTable::StaticClass(), NULL, TEXT( DataTable'/Game/Data/Characters.Characters'" ) ) );

    if( characters == NULL )
    {
        UE_LOG( LogTemp, Error, TEXT( "Characters data table not found!" ) );
        return;
    }

    // locate character
    FCharacterInfo* row = characters->FindRow<FCharacterInfo>( TEXT( "S1" ), TEXT( "LookupCharacterClass" ) );
    if( row == NULL )
    {
        UE_LOG( LogTemp, Error, TEXT( "Character ID 'S1' not found!" ) );
        return;
    }

    // add character to party
    this->PartyMembers.Add( UGameCharacter::CreateGameCharacter( row, this ) );
}
```

打开Edit|Project Settings，切到Maps&Modes，在Game Instance的下拉列表中选择RPGGameInstance，这样就把这个类设置为GameInstance类了。最后，我们要重载`BeginPlay`函数来调用`Init`方法：
```
// RPGGameMode.h
virtual void BeginPlay() override;

// RPGGameMode.cpp
void ARPGGameMode::BeginPlay()
{
    Cast<URPGGameInstance>( GetGameInstance() )->Init();
}
```

现在我们有了一个活动的队伍成员的列表，是时候搭建战斗引擎的雏形了。

### 回合制战斗

就如第一章提到的，我们的战斗是回合制，先让所有角色选择动作，然后依次序执行动作。
战斗被分为两个主要的阶段：决策阶段，和行动阶段。

#### CombatEngine

让我们创建一个`CombatEngine`类来处理战斗：
```
#include "RPG.h"
#include "GameCharacter.h"

enum class CombatPhase : uint8
{
    CPHASE_Decision,
    CPHASE_Action,
    CPHASE_Victory,
    CPHASE_GameOver,
};

class RPG_API CombatEngine
{
public:
    TArray<UGameCharacter*> combatantOrder;
    TArray<UGameCharacter*> playerParty;
    TArray<UGameCharacter*> enemyParty;
    CombatPhase phase;

protected:
    UGameCharacter* currentTickTarget;
    int tickTargetIndex;

public:
    CombatEngine( TArray<UGameCharacter*> playerParty, TArray<UGameCharacter*> enemyParty );
    ~CombatEngine();
    bool Tick( float DeltaSeconds );

protected:
    void SetPhase( CombatPhase phase );
    void SelectNextCharacter();
};
```

我们的战斗引擎在触发战斗时被分配内存，在战斗结束是被删除。
一个`CombatEngine`实例持有3个`TArray`：一个战斗次序表（包含所有参与战斗者的列表，决定了他们被轮到的顺序），一个玩家表，一个敌人表。
`CombatEngine`类定义了一个`Tick`方法。在战斗中game mode每一帧都会调用这个方法，如果战斗结束，那么它的返回值是true（否则是false）。它需要一个参数——上一帧所用的秒数。

这个类的CPP文件很大，我们把它分成小块。首先，构造函数和析构函数如下：
```
CombatEngine::CombatEngine( TArray<UGameCharacter*> playerParty, TArray<UGameCharacter*> enemyParty )
{
    this->playerParty = playerParty;
    this->enemyParty = enemyParty;

    // first add all players to combat order
    for( int i = 0; i < playerParty.Num(); i++ )
    {
        this->combatantOrder.Add( playerParty[i] );
    }

    // next add all enemies to combat order
    for( int i = 0; i < enemyParty.Num(); i++ )
    {
        this->combatantOrder.Add( enemyParty[i] );
    }

    this->tickTargetIndex = 0;
    this->SetPhase( CombatPhase::CPHASE_Decision );
}

CombatEngine::~CombatEngine()
{}
```

接下来是`Tick`方法：
```
bool CombatEngine::Tick( float DeltaSeconds )
{
    switch( phase )
    {
    case CombatPhase::CPHASE_Decision:
        // todo: ask current character to make decision
        // todo: if decision made
        SelectNextCharacter();
        // no next character, switch to action phase
        if( this->tickTargetIndex == -1 )
        {
            this->SetPhase( CombatPhase::CPHASE_Action );
        }
        break;
    case CombatPhase::CPHASE_Action:
        // todo: ask current character to execute decision
        // todo: when action executed
        SelectNextCharacter();
        // no next character, loop back to decision phase
        if( this->tickTargetIndex == -1 )
        {
            this->SetPhase( CombatPhase::CPHASE_Decision );
        }
        break;
    // in case of victory or combat, return true (combat is finished)
    case CombatPhase::CPHASE_GameOver:
    case CombatPhase::CPHASE_Victory:
        return true;
        break;
    }

    // check for game over
    int deadCount = 0;
    for( int i = 0; i < this->playerParty.Num(); i++ )
    {
        if( this->playerParty[ i ]->HP <= 0 ) deadCount++;
    }
    // all players have died, switch to game over phase
    if( deadCount == this->playerParty.Num() )
    {
        this->SetPhase( CombatPhase::CPHASE_GameOver );
        return false;
    }

    // check for victory
    deadCount = 0;
    for( int i = 0; i < this->enemyParty.Num(); i++ )
    {
        if( this->enemyParty[ i ]->HP <= 0 ) deadCount++;
    }
    // all enemies have died, switch to victory phase
    if( deadCount == this->enemyParty.Num() )
    {
        this->SetPhase( CombatPhase::CPHASE_Victory );
        return false;
    }

    // if execution reaches here, combat has not finished - return false
    return false;
}

void CombatEngine::SelectNextCharacter()
{
    for( int i = this->tickTargetIndex; i < this->combatantOrder.Num(); i++ )
    {
        GameCharacter* character = this->combatantOrder[ i ];
        if( character->HP > 0 )
        {
            this->tickTargetIndex = i + 1;
            this->currentTickTarget = character;
            return;
        }
    }
    this->tickTargetIndex = -1;
    this->currentTickTarget = nullptr;
}
```

设置阶段的函数：
```
void CombatEngine::SetPhase( CombatPhase phase )
{
    this->phase = phase;
    switch( phase )
    {
    case CombatPhase::CPHASE_Action:
    case CombatPhase::CPHASE_Decision:
        // set the active target to the first character in the combat order
        this->tickTargetIndex = 0;
        this->SelectNextCharacter();
        break;
    case CombatPhase::CPHASE_Victory:
        // todo: handle victory
        break;
    case CombatPhase::CPHASE_GameOver:
        // todo: handle game over
        break;
    }
}
```
在行动或决策阶段，它把战斗次序表中的第一个存活的角色设置为tick target。

#### 与GameCharacter联动

至此，还有一个严重的问题：角色无法做决策或执行决策的动作。让我们把这些加到`GameCharacter`类里，不过现在只是伪处理。
我们先加一个`testDelayTimer`字段，它仅仅是为了测试：
```
protected:
    float testDelayTimer;
```

然后我们在类中加几个方法：
```
public:
    void BeginMakeDecision();
    bool MakeDecision( float DeltaSeconds );
    void BeginExecuteAction();
    bool ExecuteAction( float DeltaSeconds );
```

Beginxxx方法会通知角色，要开始做决策或者执行动作了，这2个方法目前的实现只是输出log消息并延迟1秒；决策或行动方法会一直询问角色，直到决策完成或动作执行结束。
```
void UGameCharacter::BeginMakeDecision()
{
    UE_LOG( LogTemp, Log, TEXT( "Character %s making decision" ), *this->CharacterName );
    this->testDelayTimer = 1;
}

bool UGameCharacter::MakeDecision( float DeltaSeconds )
{
    this->testDelayTimer -= DeltaSeconds;
    return this->testDelayTimer <= 0;
}

void UGameCharacter::BeginExecuteAction()
{
    UE_LOG( LogTemp, Log, TEXT( "Character %s executing action" ), *this->CharacterName );
    this->testDelayTimer = 1;
}

bool UGameCharacter::ExecuteAction( float DeltaSeconds )
{
    this->testDelayTimer -= DeltaSeconds;
    return this->testDelayTimer <= 0;
}
```

我们也要加一个指向战斗实例的指针。因为战斗引擎引用了角色，如果让角色引用战斗引擎就会造成循环依赖。为此，我们要在GameCharacter.h顶部加一个前向声明`class CombatEngine;`，并把include语句放到GameCharacter.cpp文件中。

接下来，我们要让战斗引擎调用决策和行动方法。首先我们添加一个标记`bool waitingForCharacter`到`CombatEngine`类。这个标记使得我们在Beginxxx和xxx之间转换。

我们修改`Tick`方法中的决策和行动。首先是决策的switch case：
```
{
    if( !this->waitingForCharacter )
    {
        this->currentTickTarget->BeginMakeDecision();
        this->waitingForCharacter = true;
    }
    bool decisionMade = this->currentTickTarget->MakeDecision( DeltaSeconds);
    if( decisionMade )
    {
        SelectNextCharacter();
        // no next character, switch to action phase
        if( this->tickTargetIndex == -1 )
        {
            this->SetPhase( CombatPhase::CPHASE_Action );
        }
    }
}
break;
```
> 注意这里case语句如果不带最外面那层括号，就会产生编译错误，因为`decisionMade`的初始化会被跳过。

行动阶段的case几乎一样：
```
{
    if( !this->waitingForCharacter )
    {
        this->currentTickTarget->BeginExecuteAction();
        this->waitingForCharacter = true;
    }
    bool actionFinished = this->currentTickTarget->ExecuteAction( DeltaSeconds );
    if( actionFinished )
    {
        SelectNextCharacter();
        // no next character, switch to action phase
        if( this->tickTargetIndex == -1 )
        {
            this->SetPhase( CombatPhase::CPHASE_Decision );
        }
    }
}
break;
```

我们修改一下`SelectNextCharacter`，把`waitingForCharacter`设置成false：
```
void CombatEngine::SelectNextCharacter()
{
    this->waitingForCharacter = false;
    // ...
}
```

在构造函数中，我们的战斗引擎须要设置所有角色的`CombatInstance`指针，使它们指向战斗引擎自身。在析构函数中将清空这些指针，并释放敌人的指针：
```
CombatEngine::CombatEngine( TArray<UGameCharacter*> playerParty, TArray<UGameCharacter*> enemyParty )
{
    // ...
    for( int i = 0; i < this->combatantOrder.Num(); i++ )
    {
        this->combatantOrder[i]->combatInstance = this;
    }
    this->tickTargetIndex = 0;
    this->SetPhase( CombatPhase::CPHASE_Decision );
}

CombatEngine::~CombatEngine()
{
    // free enemies
    for( int i = 0; i < this->enemyParty.Num(); i++ )
    {
        this->enemyParty[i] = nullptr;
    }

    for( int i = 0; i < this->combatantOrder.Num(); i++ )
    {
        this->combatantOrder[i]->combatInstance = nullptr;
    }
}
```

#### 与GameMode联动

我们还需要有触发战斗和通过game mode更新战斗引擎的方法。
所以，首先在game mode类中，添加一个指向当前战斗实例的指针；重载`Tick`方法；维护一个敌人角色列表（用`UPROPERTY`装饰，这样才能被垃圾回收）：
```
#include "Combat/CombatEngine.h"

UCLASS()
class RPG_API ARPGGameMode : public AGameMode
{
    GENERATED_BODY()

    ARPGGameMode( const class FObjectInitializer& ObjectInitializer );
    virtual void Tick( float DeltaTime ) override;

public:
    CombatEngine* currentCombatInstance;
    TArray<UGameCharacter*> enemyParty;
};
```

然后在.cpp文件中，实现`Tick`方法：
```
void ARPGGameMode::Tick( float DeltaTime )
{
    if( this->currentCombatInstance != nullptr )
    {
        bool combatOver = this->currentCombatInstance->Tick( DeltaTime );
        if( combatOver )
        {
            if( this->currentCombatInstance->phase == CombatPhase::CPHASE_GameOver )
            {
                UE_LOG( LogTemp, Log, TEXT( "Player loses combat, game over" ) );
            }
            else if( this->currentCombatInstance->phase == CombatPhase::CPHASE_Victory )
            {
                UE_LOG( LogTemp, Log, TEXT( "Player wins combat" ) );
            }

            // enable player actor
            UGameplayStatics::GetPlayerController( GetWorld(), 0 )->SetActorTickEnabled( true );
            delete( this->currentCombatInstance );
            this->currentCombatInstance = nullptr;
            this->enemyParty.Empty();
        }
    }
}
```

然而我们现在还是没有开启战斗触发器。没有和玩家战斗的敌人。

我们定义了一张敌人的表格，但是我们的GameCharacter类不支持根据EnemyInfo来初始化。
为此，我们要为GameCharacter类添加一个新的工方法（当然要先include EnemyInfo类）：
```
static UGameCharacter* CreateGameCharacter( FEnemyInfo* enemyInfo, UObject* outer );

// implementation in .cpp file
UGameCharacter* UGameCharacter::CreateGameCharacter( FEnemyInfo* enemyInfo, UObject* outer )
{
    UGameCharacter* character = NewObject<UGameCharacter>( outer );

    character->CharacterName = enemyInfo->EnemyName;
    character->ClassInfo = nullptr; //敌人没有职阶

    character->MHP = enemyInfo->MHP;
    character->MMP = 0;
    character->HP = enemyInfo->MHP;
    character->MP = 0;

    character->ATK = enemyInfo->ATK;
    character->DEF = enemyInfo->DEF;
    character->LUCK = enemyInfo->Luck;

    return character;
}
```

为了测试我们的战斗系统，我们要在RPGGameMode.h中添加一个由Unreal控制台调用的函数：
```
UFUNCTION(exec) //使函数可以被控制台命令调用
void TestCombat();
```

这个方法在RPGGameMode.cpp中的实现如下：
```
void ARPGGameMode::TestCombat()
{
    // locate enemies asset
    UDataTable* enemyTable = Cast<UDataTable>( StaticLoadObject( UDataTable::StaticClass(), NULL, TEXT("DataTable'/Game/Data/Enemies.Enemies'" ) ) );
    if( enemyTable == NULL )
    {
        UE_LOG( LogTemp, Error, TEXT( "Enemies data table not found!" ) );
        return;
    }

    // locate enemy
    FEnemyInfo* row = enemyTable->FindRow<FEnemyInfo>( TEXT( "S1" ), TEXT( "LookupEnemyInfo" ) );
    if( row == NULL )
    {
        UE_LOG( LogTemp, Error, TEXT( "Enemy ID 'S1' not found!" ) );
        return;
    }

    // disable player actor
    UGameplayStatics::GetPlayerController( GetWorld(), 0 )->SetActorTickEnabled( false );

    // add character to enemy party
    UGameCharacter* enemy = UGameCharacter::CreateGameCharacter( row, this );
    this->enemyParty.Add( enemy );

    URPGGameInstance* gameInstance = Cast<URPGGameInstance>( GetGameInstance() );
    this->currentCombatInstance = new CombatEngine( gameInstance->PartyMembers, this->enemyParty );
    UE_LOG( LogTemp, Log, TEXT( "Combat started" ) );
}
```

#### 用控制台命令来测试战斗

最后，可以开始测试战斗引擎了。开始游戏，按下波浪符（~）键打开控制台命令输入框。输入TestCombat并按下回车。

在输出窗口你应该能看见如下的东西：
```
LogTemp: Combat started
LogTemp: Character Kumo making decision
LogTemp: Character Goblin making decision
LogTemp: Character Kumo executing action
LogTemp: Character Goblin executing action
LogTemp: Character Kumo making decision
LogTemp: Character Goblin making decision
LogTemp: Character Kumo executing action
LogTemp: Character Goblin executing action
LogTemp: Character Kumo making decision
LogTemp: Character Goblin making decision
LogTemp: Character Kumo executing action
LogTemp: Character Goblin executing action
LogTemp: Character Kumo making decision
```

这说明战斗引擎如预期的那样运行了。因为实际上什么也没做，现在战斗会永远循环下去。

目前还有两个课题待解决：一个如上所述——实际上没做什么事；另一个是玩家角色和敌对角色的决策方式需要区分（玩家角色需要UI来选择行为，而敌人是自动选行为的）。

我们先解决第一个课题。

#### 执行actions

为了让角色可以执行动作，我们要将所有战斗动作抽象为单个公用的接口。首先这个接口要能匹配我们已使用的角色的`BeginExecuteAction`和`ExecuteAxtion`方法。

我们新建一个`ICombatAction`接口：
```
#pragma once

#include "GameCharacter.h"

class UGameCharacter;

class ICombatAction
{
public:
    virtual void BeginExecuteAction( UGameCharacter* character ) = 0;
    virtual bool ExecuteAction( float DeltaSeconds ) = 0;
};
```

然后，可以新建一个动作来实现这个接口。作为测试，我们在`TestCombatAction`中让角色什么也不做。头文件如下：
```
#pragma once
#include "ICombatAction.h"

class TestCombatAction : public ICombatAction
{
protected:
    float delayTimer;
public:
    virtual void BeginExecuteAction( UGameCharacter* character ) override;
    virtual bool ExecuteAction( float DeltaSeconds ) override;
};
```

.cpp文件如下：
```
#include "RPG.h"
#include "TestCombatAction.h"

void TestCombatAction::BeginExecuteAction( UGameCharacter* character )
{
    UE_LOG( LogTemp, Log, TEXT( "%s does nothing" ), *character->CharacterName );
    this->delayTimer = 1.0f;
}

bool TestCombatAction::ExecuteAction( float DeltaSeconds )
{
    this->delayTimer -= DeltaSeconds;
    return this->delayTimer <= 0.0f;
}
```

然后我们要修改角色，令其能存储和执行动作。

先把测试时加的延迟计时器字段替换为一个战斗动作指针。让它成为public的，对决策系统开放：
```
public:
    ICombatAction* combatAction;
```

然后，我们需要在决策函数中对战斗动作赋值、在行为函数中执行这个动作：
```
void UGameCharacter::BeginMakeDecision()
{
    UE_LOG( LogTemp, Log, TEXT( "Character %s making decision" ), *( this->CharacterName ) );
    this->combatAction = new TestCombatAction();
}

bool UGameCharacter::MakeDecision( float DeltaSeconds )
{
    return true;
}

void UGameCharacter::BeginExecuteAction()
{
    this->combatAction->BeginExecuteAction( this );
}

bool UGameCharacter::ExecuteAction( float DeltaSeconds )
{
    bool finishedAction = this->combatAction->ExecuteAction( DeltaSeconds );
    if( finishedAction )
    {
        delete( this->combatAction );
        return true;
    }
    return false;
}
```

现在我们已经能存储和执行动作了，下一步是为角色构建决策系统。

#### 做决策

就如动作一样，我们将为决策创建一个接口，`IDecisionMaker`：
```
#pragma once
#include "GameCharacter.h"

class UGameCharacter;

class IDecisionMaker
{
public:
    virtual void BeginMakeDecision( UGameCharacter* character ) = 0;
    virtual bool MakeDecision( float DeltaSeconds ) = 0;
};
```

然后创建`TestDecisionMaker`来实现这个接口：
```
// TestDecisionMaker.h
#pragma once
#include "IDecisionMaker.h"

class RPG_API TestDecisionMaker : public IDecisionMaker
{
public:
    virtual void BeginMakeDecision( UGameCharacter* character ) override;
    virtual bool MakeDecision( float DeltaSeconds ) override;
};

// TestDecisionMaker.cpp
#include "RPG.h"
#include "TestDecisionMaker.h"
#include "../Actions/TestCombatAction.h"

void TestDecisionMaker::BeginMakeDecision( UGameCharacter* character )
{
    character->combatAction = new TestCombatAction();
}

bool TestDecisionMaker::MakeDecision( float DeltaSeconds )
{
    return true;
}
```

接下来，在游戏角色类中添加一个`IDecisionMaker`指针，并用它来修改BeginMakeDecision/MakeDecision函数：
```
// GameCharacter.h
public:
    IDecisionMaker* decisionMaker;

// GameCharacter.cpp
void UGameCharacter::BeginDestroy()
{
    Super::BeginDestroy();
    delete( this->decisionMaker );
}

void UGameCharacter::BeginMakeDecision()
{
    this->decisionMaker->BeginMakeDecision( this );
}

bool UGameCharacter::MakeDecision( float DeltaSeconds )
{
    return this->decisionMaker->MakeDecision( DeltaSeconds );
}

// 在重载的几个构造函数中，都添加下面这行代码：
this->decisionMaker = new TestDecisionMaker();
```

现在，可以为不同的角色赋予不一样的决策产生器了，并且很容易把要执行的战斗动作赋给这些决策产生器。然而，我们还要先对GameCharacter类做一个小修改。

#### 目标选择

我们要在GameCharacter类加一个字段`bool isPlayer;`来区分角色是玩家还是敌人。另外，我们还要加一个`UGameCharacter* SelectTarget();`方法，根据是玩家还是敌人，来选择当前战斗实例中的第一个存活角色。
GameCharacter.cpp 中，两个构造函数中对`isPlayer`字段进行不同的赋值：
```
UGameCharacter* CreateGameCharacter( FCharacterInfo* characterInfo, UObject* outer )
{
    //...
    character->isPlayer = true;
    return character;
}
UGameCharacter* CreateGameCharacter( FEnemyInfo* enemyInfo, UObject* outer)
{
    // ...
    character->isPlayer = false;
    return character;
}
```

从敌对阵营自动选择目标的`selectTarget`方法如下：
```
UGameCharacter* UGameCharacter::SelectTarget()
{
    UGameCharacter* target = nullptr;
    TArray<UGameCharacter*> targetList = this->combatInstance->enemyParty;
    if( !this->isPlayer )
    {
        targetList = this->combatInstance->playerParty;
    }
    for( int i = 0; i < targetList.Num(); i++ )
    {
        if( targetList[ i ]->HP > 0 )
        {
            target = targetList[i];
            break;
        }
    }
    if( target->HP <= 0 )
    {
        return nullptr;
    }
    return target;
}
```

#### 计算伤害

我们来让`TestCombatAction`类能造成伤害。添加角色引用和目标引用字段，以及一个带目标参数的构造函数：
```
protected:
    UGameCharacter* character;
    UGameCharacter* target;
public:
    TestCombatAction( UGameCharacter* target );
```

修改实现部分：
```
TestCombatAction::TestCombatAction( UGameCharacter* target )
{
    this->target = target;
}

void TestCombatAction::BeginExecuteAction( UGameCharacter* character )
{
    this->character = character;

    // target is dead, select another target
    if( this->target->HP <= 0 )
    {
        this->target = this->character->SelectTarget();
    }

    // no target, just return
    if( this->target == nullptr )
    {
        return;
    }
    UE_LOG( LogTemp, Log, TEXT( "%s attacks %s" ), *character->CharacterName, *target->CharacterName );

    target->HP -= 10;
    this->delayTimer = 1.0f;
}
```

下一步，修改`TestDecisionMaker`，选择一个目标并传递给`TestCombatAction`的构造函数：
```
void TestDecisionMaker::BeginMakeDecision( UGameCharacter* character )
{
    // pick a target
    UGameCharacter* target = character->SelectTarget();
    character->combatAction = new TestCombatAction( target );
}
```

至此，你应该能在测试时得到像下面这样的输出了：
```
LogTemp: Combat started
LogTemp: Kumo attacks Goblin
LogTemp: Goblin attacks Kumo
LogTemp: Kumo attacks Goblin
LogTemp: Player wins combat
```

接下来，我们将开始关联用户界面。

#### 使用UMG的战斗UI

在开始之前，我们要设置项目来正确地导入UMG和Slate相关类。

首先，打开RPG.Build.cs（或者[项目名].Build.cs），将构造函数的第一行改为下面的代码：
```
PublicDependencyModuleNames.AddRange( new string[] { "Core", "CoreUObject", "Engine", "InputCore", "UMG", "Slate", "SlateCore" } );
```

然后在RPG.h中加入如下几行代码：
```
#include "Runtime/UMG/Public/UMG.h"
#include "Runtime/UMG/Public/UMGStyle.h"
#include "Runtime/UMG/Public/Slate/SObjectWidget.h"
#include "Runtime/UMG/Public/IUMGModule.h"
#include "Runtime/UMG/Public/Blueprint/UserWidget.h"
```

现在花一些时间编译项目。

##### CombatUIWidget类和CombatUI蓝图

接着我们将创建一个战斗UI的基类。我们将通过这个基类来建立C++代码和蓝图UMG代码之间的通信，方法是在头文件中定义**蓝图实现函数**（BluePrint-implementable functions），这种函数由C++调用、由蓝图实现。

新建一个`CombatUIWidget`类，选择UserWidget作为父类：
```
#include "GameCharacter.h"
#include "Blueprint/UserWidget.h"
#include "CombatUIWidget.generated.h"

UCLASS()
class RPG_API UCombatUIWidget : public UUserWidget
{
    GENERATED_BODY()

public:
    UFUNCTION( BlueprintImplementableEvent, Category = "Combat UI" )
    void AddPlayerCharacterPanel( UGameCharacter* target );

    UFUNCTION( BlueprintImplementableEvent, Category = "Combat UI" )
    void AddEnemyCharacterPanel( UGameCharacter* target );
};
```

编译这些代码之后，回到编辑器中，新建一个Widget蓝图，名为CombatUI。打开这个蓝图，在File|ReparentBlueprint这里选择`CombatUIWidget`作为父类。

在Designer界面，创建两个Horizontal Box并命名为enemyPartyStatus和playerPartyStatus。它们将分别持有敌人和玩家的角色，并显示每个角色的状态。别忘了勾选Is Variable选项框，这关系到他们能不能作为蓝图的变量来使用。保存并编译蓝图。

我们要创建显示玩家和敌人角色状态的窗口，两者有着共同的父窗口。

##### 在CombatUI中创建角色状态面板

新建一个Widget蓝图，名为PlayerCharacterCombatPanel，设置它的父类为BaseCharacterCombatPanel。

在Designer界面，添加三个Text Block窗口。一个显示角色名，另一个显示HP，第三个显示MP。在细节面板的Text右边点击Bind，为每个Text Block创建一个新的binding。这会创建新的蓝图函数，负责生成Text Block。
以HP Text Block为例，可以执行以下步骤：
1. 把Character Target变量拖到图中，选择Get。
2. 拖出输出引脚，选择Variables|Character下的Get HP。
3. 新建一个Format Text节点，设置文本为HP：{HP}，然后连接Get HP的输出到Format Text节点的输入节点。
4. 把Format Text节点的输出连到Return节点的Return值上。

你可以用相似的步骤设置角色名和HP的binding。

EnemyCharacterCombatPanel的创建与PlayerCharacterCombatPanel类似，只不过没有MP Text Block（之前提过，敌人不消耗MP）。

现在我们有了玩家和敌人的窗口，可以来实现CombatUI蓝图中的`AddPlayerCharacterPanel`和`AddEnemyCharacterPanel`函数了。

首先，创建一个辅助的蓝图函数`SpawnCharacterWidget`，用来生成角色状态窗。这个函数接收下列参数：
- TargetCharacter，类型为Game Character的引用
- TargetPanel，类型为Panel窗口的引用
- Class，类型为BaseCharacterCombatPanel类

这个函数会执行下列步骤：
1. 根据给定类，构建新的窗口。
2. 把这个窗口转换为BaseCharacterCombatPanel类型。
3. 把这个窗口的Character Target设置为输入的TargetCharacter。
4. 把这个窗口添加到输入的TargetPanel的子窗口。

对应的蓝图如下：
![SpawnCharacterCombatPanel](http://m.qpic.cn/psc?/V11Tp57c2B9kPO/TmEUgtj9EK6.7V8ajmQrEI.gTSqU16RAXGeXc5lm1ueCmuLQco7iTdI0kznrrYPWxal0bjgwrRMXuTjuaZaw8xl4z.jVAKq*1ePWMCqfuTs!/b&bo=swInAQAAAAADJ5U!&rf=viewer_4)

然后在CombatUI的事件图表中，右键添加`EventAddPlayerCharacterPanel`和`EventAddEnemyCharacterPanel`事件，每个事件都连一个`SpawnCharacterWidget`函数，并设置输入如下图：
![CombatUI的事件图表](http://m.qpic.cn/psc?/V11Tp57c2B9kPO/TmEUgtj9EK6.7V8ajmQrEExTsxfvIWXnIb3F0y1rd4zTIWXePbfDLk0G8s4nhozaQI73nHM*bOaB0XGKPosYt1HDhOYP8vaRpjzDLQQT4RE!/b&bo=*QGtAQAAAAADJ1I!&rf=viewer_4)

##### 战斗UI生成和销毁

战斗开始时，我们可以从game mode生成UI，当战斗结束时销毁。在`RPGGameMode`头文件中，添加指向`UCombatUIWidget`的指针，以及一个能生成战斗UI的类（以便我们能选择一个继承自`CombatUIWidget`的Widget BluePrint）：
```
UPROPERTY()
UCombatUIWidget* CombatUIInstance;

UPROPERTY( EditDefaultsOnly, BlueprintReadOnly, Category = "UI" )
TSubclassOf<class UCombatUIWidget> CombatUIClass;
```

在我们的`TestCombat`函数中，我们可以生成这个窗口的新实例：
```
this->CombatUIInstance = CreateWidget<UCombatUIWidget>( GetGameInstance(), this->CombatUIClass );
this->CombatUIInstance->AddToViewport();

for( int i = 0; i < gameInstance->PartyMembers.Num(); i++ )
this->CombatUIInstance->AddPlayerCharacterPanel( gameInstance->PartyMembers[i] );

for( int i = 0; i < this->enemyParty.Num(); i++ )
this->CombatUIInstance->AddEnemyCharacterPanel( this->enemyParty[i] );
```

战斗结束后，我们从viewport删除窗口并设置它的引用为null，使其可以被垃圾回收：
```
this->CombatUIInstance->RemoveFromViewport();
this->CombatUIInstance = nullptr;
```

接下来，我们要让玩家角色通过UI来决策，而不是自动决策。

#### 以UI驱动做决策

我们可以让`CombatUIWidget`类来实现决策机制，并且仅在战斗开始时对其赋值（并在战斗结束时清空指针）。

我们将对`GameCharacter`类做一些改动。首先在玩家对`CreateGameCharacter`的重载中删除下面这行代码：
```
character->decisionMaker = new TestDecisionMaker();
```

然后，在`BeginDestroy`函数中，用if包住delete语句：
```
if( !this.isPlayer )
    delete( this->decisionMaker );
```

这样做的原因是我们不能手动删除UI（这样会使Unreal崩溃）。现在，只要没有UPROPERTY指向它，它就会自动被垃圾回收。

接着，在CombatUIWidget.h中我们会实现`IDecisionMaker`接口：
```
class RPG_API UCombatUIWidget: public UUserWidget, public IDecisionMaker
{
// ...
public:
    void BeginMakeDecision( UGameCharacter* target );
    bool MakeDecision( float DeltaSeconds );
}
```

我们加一些可以被UI蓝图图表调用的辅助函数：
```
public:
    UFUNCTION( BlueprintCallable, Category = "Combat UI" )
    TArray<UGameCharacter*> GetCharacterTargets(); //获取当前角色的可能的目标

    UFUNCTION( BlueprintCallable, Category = "Combat UI" )
    void AttackTarget( UGameCharacter* target ); //将给定目标的新TestCombatAction加到当前角色上
```

此外，我们还要添加一个蓝图实现函数，用来显示当前角色的一系列action：
```
UFUNCTION( BlueprintImplementableEvent, Category = "Combat UI" )
void ShowActionsPanel( UGameCharacter* target );
```

再添加一个如下的标记：
```
protected:
    bool finishedDecision; //是否已完成决策
```

直接给出四个函数的实现：
```
void UCombatUIWidget::BeginMakeDecision( UGameCharacter* target )
{
    this->currentTarget = target;
    this->finishedDecision = false;
    ShowActionsPanel( target );
}

bool UCombatUIWidget::MakeDecision( float DeltaSeconds )
{
    return this->finishedDecision;
}

void UCombatUIWidget::AttackTarget( UGameCharacter* target )
{
    TestCombatAction* action = new TestCombatAction( target );
    this->currentTarget->combatAction = action;
    this->finishedDecision = true;
}

TArray<UGameCharacter*> UCombatUIWidget::GetCharacterTargets()
{
    if( this->currentTarget->isPlayer )
    {
        return this->currentTarget->combatInstance->enemyParty;
    }
    else
    {
        return this->currentTarget->combatInstance->playerParty;
    }
}
```

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