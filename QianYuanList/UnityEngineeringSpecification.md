# Unity 工程规范

## 文档定位

这份文档用于统一当前项目的 Unity 工程组织方式，避免后续在开发过程中出现目录混乱、命名不统一、代码耦合过高、配置难维护的问题。

当前项目前提：

- 开发引擎：`Unity`
- 开发语言：`C#`
- 目标平台：`PC`
- 发布平台：`Steam`
- 项目类型：独立游戏

这份规范主要覆盖四部分：

- 目录结构
- 代码分层
- 配置表格式
- 命名规范

## 一、工程总体原则

### 1.1 核心原则

整个工程必须遵循以下原则：

- 目录按职责划分，不按个人习惯随意堆放
- 代码按层次划分，不把所有逻辑塞进 MonoBehaviour
- 配置尽量数据驱动，不把数值写死在脚本里
- 资源命名统一，可搜索、可批量处理、可维护
- 所有临时内容都必须可被替换，不污染正式结构

### 1.2 工程目标

这套工程规范要解决四个实际问题：

- 后续增加角色、武器、敌人时不需要重构底层
- 新资源导入后能快速归类
- 新脚本写完后别人能立刻找到位置
- 配置、逻辑、资源可以独立迭代

## 二、Unity 目录结构规范

## 2.1 推荐根目录结构

`Assets` 下推荐固定为以下结构：

```text
Assets/
  Art/
  Audio/
  Data/
  Fonts/
  Materials/
  Plugins/
  Prefabs/
  Resources/
  Scenes/
  Scripts/
  Settings/
  Shaders/
  Sprites/
  StreamingAssets/
  UI/
  VFX/
  Editor/
  Gizmos/
  ThirdParty/
```

说明：

- `Art/`：原画、角色立绘、概念图等非运行时主资源
- `Audio/`：BGM、SFX、语音
- `Data/`：配置表、ScriptableObject、CSV/JSON 数据
- `Fonts/`：字体资源
- `Materials/`：材质资源
- `Plugins/`：Unity 插件或原生插件
- `Prefabs/`：所有可复用预制体
- `Resources/`：仅放必须通过 `Resources.Load` 加载的内容
- `Scenes/`：场景文件
- `Scripts/`：业务代码
- `Settings/`：URP/HDRP、Input、全局配置资源
- `Shaders/`：自定义 Shader
- `Sprites/`：运行时 2D 贴图、图集源文件
- `StreamingAssets/`：必须原样打包的外部资源
- `UI/`：UI 图集、UI 相关预制体、界面资源
- `VFX/`：特效资源
- `Editor/`：仅编辑器工具脚本
- `Gizmos/`：Gizmos 资源
- `ThirdParty/`：第三方资源包的隔离目录

## 2.2 业务资源目录细化

### `Scenes/`

建议结构：

```text
Scenes/
  Boot/
  Menu/
  Battle/
  Test/
```

建议场景职责：

- `Boot`：启动初始化
- `Menu`：主菜单、角色选择、设置等
- `Battle`：正式战斗
- `Test`：功能测试、单模块验证

### `Prefabs/`

建议结构：

```text
Prefabs/
  Characters/
  Enemies/
  Weapons/
  Projectiles/
  Pickups/
  UI/
  VFX/
  Environment/
```

### `Sprites/`

建议结构：

```text
Sprites/
  Characters/
  Enemies/
  Weapons/
  Items/
  UI/
  Icons/
  Environment/
```

### `Audio/`

建议结构：

```text
Audio/
  BGM/
  SFX/
  UI/
  Voice/
```

### `Data/`

建议结构：

```text
Data/
  ConfigTables/
  ScriptableObjects/
  Localization/
  Balancing/
```

## 2.3 严格限制

必须遵守：

- 不允许把测试资源直接丢在 `Assets/` 根目录
- 不允许把脚本和美术混放在同一目录
- 不允许把第三方插件拆散到项目各处
- 不允许把临时 Prefab 与正式 Prefab 混在一起
- 不允许大量依赖 `Resources/` 作为默认加载方式

## 三、代码分层规范

## 3.1 Scripts 目录结构

建议结构：

```text
Scripts/
  Runtime/
    Core/
    Boot/
    Framework/
    Gameplay/
      Characters/
      Enemies/
      Weapons/
      Items/
      Projectiles/
      Waves/
      Battle/
    UI/
    Data/
    Save/
    Audio/
    Utilities/
  Editor/
  Tests/
```

## 3.2 运行时代码分层

### `Core`

职责：

- 全局常量
- 通用枚举
- 公共接口
- 基础抽象

不要放：

- 具体业务逻辑
- 具体角色或敌人实现

### `Boot`

职责：

- 游戏启动流程
- 初始场景调度
- 全局系统初始化

### `Framework`

职责：

- 事件系统
- 状态机
- 定时器
- 对象池
- 服务定位或依赖注入封装
- 通用模块基类

### `Gameplay`

职责：

- 核心战斗逻辑
- 玩家角色
- 敌人
- 武器
- 弹道
- 道具
- 波次
- 战斗结算

这是项目核心业务层。

### `UI`

职责：

- 页面逻辑
- HUD 逻辑
- 面板控制
- UI 数据展示适配

必须注意：

- UI 只负责展示与交互
- UI 不直接承载战斗核心逻辑

### `Data`

职责：

- 配置读取
- 数据映射
- 表结构定义
- 配置缓存

### `Save`

职责：

- 本地存档
- 设置项存储
- 解锁数据
- 运行记录

### `Audio`

职责：

- BGM 播放控制
- SFX 播放控制
- 音量组管理
- 音频事件映射

### `Utilities`

职责：

- 纯工具函数
- 扩展方法
- 与具体业务弱耦合的通用帮助类

## 3.3 MonoBehaviour 使用规则

必须注意：

- MonoBehaviour 主要用于生命周期接入、场景绑定和表现层联动
- 不要让 MonoBehaviour 成为数据、逻辑、状态、配置的总容器
- 复杂逻辑优先拆为纯 C# 类，再由 MonoBehaviour 持有和驱动

建议模式：

- `View` 负责表现
- `Controller` 或 `System` 负责调度
- `Model` 或配置对象负责数据

## 3.4 模块划分建议

对当前项目，建议尽早固定这些系统：

- `CharacterSystem`
- `WeaponSystem`
- `ItemSystem`
- `EnemySystem`
- `WaveSystem`
- `BattleSystem`
- `ShopSystem`
- `ProgressionSystem`
- `SaveSystem`
- `AudioSystem`
- `UISystem`

## 3.5 依赖方向规则

建议遵循以下依赖方向：

- `Core` 可被所有层依赖
- `Framework` 可被业务层依赖
- `Gameplay` 不依赖具体 UI 实现
- `UI` 可以读取 Gameplay 状态，但不反向控制核心数据结构
- `Data` 不依赖 UI
- `Save` 不依赖具体战斗实现细节

禁止出现：

- UI 页面直接改战斗底层数据
- 武器逻辑直接引用具体 UI 组件
- 敌人逻辑直接读写存档文件

## 四、配置表格式规范

## 4.1 配置驱动原则

优先数据驱动的内容：

- 角色属性
- 武器属性
- 道具效果参数
- 敌人属性
- 波次刷新
- 掉落规则
- 商店权重
- 本地化文案

不建议一开始就完全配置驱动的内容：

- 极其复杂的独特行为树
- 高度个性化 Boss 特殊机制

这些可以采用“配置 + 特化代码”的混合模式。

## 4.2 推荐配置载体

建议采用组合方式：

- `CSV/TSV` 或 `JSON`：适合批量数值配置
- `ScriptableObject`：适合 Unity 内关联资源的配置

推荐策略：

- 纯数值表：优先 `CSV`
- 需要挂资源引用的表：优先 `ScriptableObject`
- 本地化文本：单独表结构管理

## 4.3 配置表目录建议

```text
Data/
  ConfigTables/
    Character/
    Weapon/
    Item/
    Enemy/
    Wave/
    Shop/
  ScriptableObjects/
    Definitions/
    RuntimeConfigs/
  Localization/
```

## 4.4 配表字段规范

统一要求：

- 每张表必须有唯一 `Id`
- 枚举字段统一使用明确字符串或整数映射
- 布尔字段统一使用 `true/false`
- 数值字段禁止混用文本描述
- 引用字段必须可追踪到目标资源或目标表
- 备注字段单独保留，不混入业务字段

推荐通用字段：

- `Id`
- `Name`
- `DisplayName`
- `Description`
- `Icon`
- `Prefab`
- `Rarity`
- `Tags`
- `IsEnabled`
- `Remark`

## 4.5 示例表结构

### 角色表示例

```text
Id,Name,DisplayName,BaseHp,BaseSpeed,BaseDamage,PassiveDesc,StartWeaponId,Tags,IsEnabled,Remark
char_wellrounded,WellRounded,全能者,10,5,0,均衡成长,wpn_stick,balanced|starter,true,
```

### 武器表示例

```text
Id,Name,DisplayName,WeaponType,Damage,Cooldown,Range,ProjectileSpeed,Rarity,MergeTo,Tags,IsEnabled,Remark
wpn_claw_01,Claw,猫爪,melee,12,0.8,1.2,0,common,wpn_claw_02,melee|bleed,true,
```

### 敌人表示例

```text
Id,Name,DisplayName,Hp,MoveSpeed,Damage,DropMaterial,EnemyType,Tags,IsEnabled,Remark
enemy_rat_01,Rat,鼠盗,18,3.5,1,3,normal,fast|swarm,true,
```

### 波次表示例

```text
WaveId,Duration,EnemyPool,SpawnRate,ElitePool,BossPool,RewardType,Remark
wave_01,20,enemy_rat_01|enemy_bug_01,1.0,,,material,
```

## 4.6 配置表维护规则

必须遵守：

- 任何新增角色、武器、敌人都先加配置，再写接入逻辑
- 配置字段修改必须同步更新读取代码
- 表头命名一旦定下，不随意改动
- 配置版本变更要记录

## 五、命名规范

## 5.1 总体命名原则

命名必须满足：

- 一眼知道用途
- 可全文检索
- 可批量排序
- 不依赖个人缩写习惯

统一要求：

- 英文命名优先
- 使用 ASCII
- 使用清晰前缀
- 不使用拼音作为资源主命名
- 不使用“New”“Final”“Test2”这类临时词作为正式名称

## 5.2 C# 代码命名

### 类型命名

- 类、结构体、接口、枚举：`PascalCase`
- 接口以 `I` 开头

示例：

- `CharacterStats`
- `WeaponConfig`
- `IBattleSystem`
- `EnemyType`

### 字段与变量命名

- 公共属性：`PascalCase`
- 私有字段：`_camelCase`
- 局部变量与参数：`camelCase`

示例：

```csharp
private float _moveSpeed;
public float MoveSpeed { get; private set; }

public void Initialize(CharacterConfig config, float bonusDamage)
{
    var finalDamage = config.BaseDamage + bonusDamage;
}
```

### 常量命名

- 常量使用 `PascalCase`
- 避免全大写风格

示例：

- `DefaultMoveSpeed`
- `MaxWeaponSlots`

### 事件命名

- 事件使用过去式或 `OnXxx`

示例：

- `OnHpChanged`
- `BattleStarted`
- `WaveEnded`

## 5.3 文件命名

规则：

- 一个主要类对应一个同名文件
- 文件名与类名完全一致
- 编辑器脚本带 `Editor` 后缀

示例：

- `CharacterStats.cs`
- `WeaponController.cs`
- `EnemySpawner.cs`
- `WaveConfigEditor.cs`

## 5.4 Prefab 命名

建议格式：

`Category_Name_Variant`

示例：

- `Char_WellRounded_Default`
- `Enemy_Rat_Normal`
- `Wpn_Claw_T1`
- `UI_ShopPanel_Main`
- `VFX_Hit_Melee_Small`

## 5.5 Sprite 与图标命名

建议格式：

`Type_Name_State`

示例：

- `Icon_Item_FishBone`
- `Icon_Stat_AttackSpeed`
- `Spr_Char_Ragdoll_Idle`
- `Spr_Enemy_Rat_Run`

## 5.6 音频命名

建议格式：

`AudioType_Name_Variant`

示例：

- `BGM_Battle_01`
- `BGM_Shop_01`
- `SFX_Hit_Melee_01`
- `SFX_UI_Click_01`

## 5.7 配置 Id 命名

建议全部小写，下划线分隔，带类别前缀。

示例：

- `char_wellrounded`
- `char_lucky`
- `wpn_claw_01`
- `item_catnip_rare`
- `enemy_rat_01`
- `wave_01`

## 5.8 场景命名

建议格式：

`Scene_Category_Name`

示例：

- `Scene_Boot_Init`
- `Scene_Menu_Main`
- `Scene_Battle_Core`
- `Scene_Test_WeaponSandbox`

## 六、版本与临时内容规范

## 6.1 临时资源规范

临时资源必须带明确前缀：

- `Tmp_`
- `Dev_`
- `Test_`

示例：

- `Tmp_EnemyDummy`
- `Dev_DamagePanel`
- `Test_WaveSandbox`

临时资源必须定期清理，不进入正式发布结构。

## 6.2 禁止命名

禁止使用：

- `New Prefab`
- `Final`
- `Final2`
- `NewNew`
- `xxx_copy`
- 中文夹杂空格和随机编号

## 七、当前项目的推荐执行方案

对当前项目，建议立即按下面方式落地：

1. 先按本规范建立 `Assets` 主目录
2. 先建立 `Scripts/Runtime` 分层
3. 先定义 `Character/Weapon/Enemy/Wave` 四张核心配置表
4. 先统一 Prefab、Sprite、Audio、Scene 的命名规则
5. 先限制临时测试内容全部放进 `Test` 范围

## 八、当前阶段建议优先补的文档

接下来建议继续补：

1. `配置表规范样例`
2. `角色设计表`
3. `武器设计表`
4. `敌人与波次设计表`
5. `Unity 首个可玩原型开发清单`

这些文档补齐后，工程层和内容层就能开始同步推进。
