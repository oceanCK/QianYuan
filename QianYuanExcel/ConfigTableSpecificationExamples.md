# 配置表规范样例

## 文档定位

这份文档用于定义项目首批核心配置表的 Excel/CSV 结构样例，供后续内容设计、程序读取、数值调整统一使用。

本规范基于 `QianYuanDoc` 现有资料整理，重点吸收以下信息来源：

- `PetCats.md`：家猫品种、体型、毛长、性格、象征
- `WildCats.md`：野猫科体型、栖息地、物种特征
- `CatFeatures.md`：折耳、卷耳、短尾、短腿、卷毛等特色品种
- `CatColor.md`：颜色、花纹、文化象征
- `CatBehavior.md`：行为、肢体语言、沟通方式、应激反应
- `CatLikes.md`：环境喜好、食物喜好、玩具喜好、生存需求
- `CatEnemy.md`：威胁分类、敌人来源、应对方式
- `CatCulture.md`：文化象征、地域意义、宗教与现代流行文化语义

## 一、推荐输出形式

建议建立一个工作簿：

- `QianYuanGameConfig.xlsx`

建议至少包含以下 6 个 Sheet：

1. `CharacterConfig`
2. `WeaponConfig`
3. `EnemyConfig`
4. `WaveConfig`
5. `ItemConfig`
6. `ShopConfig`

如果前期不直接维护 `.xlsx`，也可以先维护六张 CSV：

- `CharacterConfigTemplate.csv`
- `WeaponConfigTemplate.csv`
- `EnemyConfigTemplate.csv`
- `WaveConfigTemplate.csv`
- `ItemConfigTemplate.csv`
- `ShopConfigTemplate.csv`

## 二、统一字段规则

### 2.1 通用规则

- 所有配置表必须有唯一 `Id`
- 所有 `Id` 使用小写英文加下划线
- 布尔值统一使用 `true/false`
- 多标签字段统一使用 `|` 分隔
- 多引用字段统一使用 `|` 分隔
- 数值字段不混入说明文字
- 备注统一放在 `Remark`

### 2.2 通用字段约定

常用公共字段建议：

- `Id`：唯一主键
- `DisplayName`：中文显示名
- `InternalName`：英文内部名
- `Tags`：标签
- `IsEnabled`：是否启用
- `Remark`：备注

### 2.3 建议标签来源

以下标签建议优先复用 `QianYuanDoc` 中现有语义。

性格标签：

- `gentle`
- `friendly`
- `independent`
- `curious`
- `playful`
- `loyal`
- `calm`
- `alert`
- `sensitive`
- `stable`
- `agile`
- `wild`

行为标签：

- `rub`
- `knead`
- `belly_show`
- `tail_swing`
- `airplane_ears`
- `purr`
- `marking`
- `slow_blink`
- `hide`
- `patrol`

环境喜好标签：

- `high_place`
- `warm_place`
- `small_space`
- `quiet_corner`
- `observation_point`
- `water_like`
- `forest_like`
- `cold_resist`

文化标签：

- `lucky`
- `wealth`
- `mystic`
- `royal`
- `guardian`
- `purity`
- `wild_power`
- `healing`
- `tradition`
- `innovation`

威胁标签：

- `dog`
- `raptor`
- `wildcat`
- `reptile`
- `human_hostile`
- `noise`
- `stranger`
- `robot`
- `shadow`
- `mirror`

应对标签：

- `hide`
- `warning`
- `escape`
- `observe`
- `attack`

## 三、角色表字段定义

Sheet 名建议：`CharacterConfig`

### 3.1 角色表用途

角色表用于定义：

- 角色基础属性
- 猫咪品种来源
- 性格与行为标签
- 视觉与文化关键词
- 开局武器与被动

### 3.2 字段定义

| 字段名 | 类型 | 必填 | 说明 | 示例 |
|---|---|---|---|---|
| `Id` | string | 是 | 角色唯一 ID | `char_wellrounded` |
| `InternalName` | string | 是 | 英文内部名 | `WellRounded` |
| `DisplayName` | string | 是 | 中文显示名 | `全能者` |
| `CatCategory` | enum | 是 | `pet_cat` / `wild_cat` / `color_cat` / `feature_cat` | `pet_cat` |
| `BreedId` | string | 是 | 对应品种或猫科来源 | `european_shorthair` |
| `BreedDisplayName` | string | 是 | 品种中文名 | `欧洲短毛猫` |
| `FurType` | enum | 否 | `short` / `long` / `hairless` / `partial_hairless` | `short` |
| `ColorType` | string | 否 | 对应颜色资料 | `gray_cat` |
| `PatternType` | string | 否 | 对应花纹资料 | `tabby` |
| `FeatureType` | string | 否 | 对应特色品种标签 | `fold_ear` |
| `BodyType` | enum | 否 | `small` / `medium` / `large` / `giant` | `medium` |
| `TemperamentTags` | string | 是 | 性格标签，来自品种/颜色/特色资料 | `gentle|independent|stable` |
| `BehaviorTags` | string | 否 | 行为标签，来自行为学资料 | `marking|slow_blink|hide` |
| `PreferenceTags` | string | 否 | 喜好与环境标签 | `high_place|quiet_corner|warm_place` |
| `CultureTags` | string | 否 | 文化象征标签 | `tradition|guardian` |
| `RoleClass` | enum | 是 | `balanced` / `melee` / `ranged` / `support` / `summon` / `special` | `balanced` |
| `StarterWeaponId` | string | 是 | 开局武器 ID | `wpn_stick_01` |
| `BaseHp` | int | 是 | 初始生命 | `10` |
| `BaseMoveSpeed` | float | 是 | 初始移速 | `5.0` |
| `BaseMeleeDamage` | int | 是 | 初始近战伤害修正 | `0` |
| `BaseRangedDamage` | int | 是 | 初始远程伤害修正 | `0` |
| `BaseAttackSpeed` | float | 是 | 初始攻速修正 | `0.0` |
| `BaseCritChance` | float | 是 | 初始暴击率 | `0.0` |
| `BaseArmor` | int | 是 | 初始护甲 | `0` |
| `BaseDodge` | float | 是 | 初始闪避率 | `0.0` |
| `BaseLuck` | float | 是 | 初始幸运 | `0.0` |
| `BaseHarvesting` | int | 是 | 初始收获 | `0` |
| `PassiveDesc` | string | 是 | 被动描述 | `属性均衡成长，无明显短板。` |
| `PassiveKey` | string | 是 | 被动逻辑键 | `passive_balanced_growth` |
| `VisualKeywords` | string | 否 | 美术关键词 | `round_face|short_fur|steady_pose` |
| `AudioKeywords` | string | 否 | 音效关键词 | `calm|soft|low_purr` |
| `UnlockCondition` | string | 否 | 解锁条件描述或键 | `default` |
| `IsEnabled` | bool | 是 | 是否启用 | `true` |
| `Remark` | string | 否 | 备注 | `首批可玩角色` |

### 3.3 角色字段设计说明

- `BreedId` 主要来自 `PetCats.md`、`WildCats.md`、`CatFeatures.md`
- `ColorType`、`PatternType` 主要来自 `CatColor.md`
- `TemperamentTags` 主要来自品种性格描述
- `BehaviorTags` 主要来自 `CatBehavior.md`
- `PreferenceTags` 主要来自 `CatLikes.md`
- `CultureTags` 主要来自 `CatCulture.md`

## 四、武器表字段定义

Sheet 名建议：`WeaponConfig`

### 4.1 武器表用途

武器表用于定义：

- 武器基础攻击属性
- 武器主题来源
- 对应猫行为、猫喜好、文化或身体结构
- 武器稀有度与升级关系

### 4.2 字段定义

| 字段名 | 类型 | 必填 | 说明 | 示例 |
|---|---|---|---|---|
| `Id` | string | 是 | 武器唯一 ID | `wpn_claw_01` |
| `InternalName` | string | 是 | 英文内部名 | `Claw` |
| `DisplayName` | string | 是 | 中文显示名 | `猫爪` |
| `WeaponClass` | enum | 是 | `melee` / `ranged` / `throw` / `summon` / `support` | `melee` |
| `WeaponSubType` | string | 是 | 子类 | `claw` |
| `ThemeSource` | enum | 是 | `behavior` / `body_part` / `food` / `toy` / `culture` / `enemy_response` | `body_part` |
| `ThemeTags` | string | 否 | 主题标签 | `claw|scratch|marking` |
| `BreedAffinityTags` | string | 否 | 适配角色或品种标签 | `wild|tabby|agile` |
| `Rarity` | enum | 是 | `common` / `uncommon` / `rare` / `epic` / `legendary` | `common` |
| `DamageType` | enum | 是 | `physical` / `bleed` / `magic` / `poison` / `shock` | `physical` |
| `TargetingType` | enum | 是 | `nearest` / `front` / `random` / `area` / `self_center` | `nearest` |
| `BaseDamage` | float | 是 | 基础伤害 | `12` |
| `DamageScale` | float | 是 | 伤害成长系数 | `1.0` |
| `Cooldown` | float | 是 | 攻击冷却秒数 | `0.8` |
| `Range` | float | 是 | 攻击距离或半径 | `1.2` |
| `ProjectileSpeed` | float | 否 | 弹体速度，近战填 `0` | `0` |
| `PierceCount` | int | 否 | 穿透次数 | `0` |
| `BounceCount` | int | 否 | 弹跳次数 | `0` |
| `Knockback` | float | 否 | 击退强度 | `0.5` |
| `CritBonus` | float | 否 | 暴击修正 | `0.0` |
| `LifeStealBonus` | float | 否 | 吸血修正 | `0.0` |
| `AttackAnimKey` | string | 否 | 动画键 | `attack_claw_slash` |
| `VfxKey` | string | 否 | 特效键 | `vfx_slash_small` |
| `SfxKey` | string | 否 | 音效键 | `sfx_hit_claw_01` |
| `MergeToId` | string | 否 | 合成后的下一阶 ID | `wpn_claw_02` |
| `ShopWeight` | int | 是 | 商店权重 | `100` |
| `UnlockCondition` | string | 否 | 解锁条件 | `default` |
| `IsEnabled` | bool | 是 | 是否启用 | `true` |
| `Remark` | string | 否 | 备注 | `基础近战武器` |

### 4.3 武器字段设计说明

- `ThemeSource` 和 `ThemeTags` 可以直接借用 `CatBehavior.md` 与 `CatLikes.md`
- 例如玩具系武器可以来自 `逗猫棒`、`激光笔`、`纸团`
- 食物系武器或道具关联可来自 `小鱼干`、`猫薄荷`
- 敌应对系武器可借用 `哈气`、`炸毛`、`标记领地` 的语义

## 五、敌人表字段定义

Sheet 名建议：`EnemyConfig`

### 5.1 敌人表用途

敌人表用于定义：

- 敌人来源与威胁类别
- 基础数值与行为
- 是否可作为精英或 Boss
- 对应猫咪世界观中的威胁语义

### 5.2 字段定义

| 字段名 | 类型 | 必填 | 说明 | 示例 |
|---|---|---|---|---|
| `Id` | string | 是 | 敌人唯一 ID | `enemy_dog_stray_01` |
| `InternalName` | string | 是 | 英文内部名 | `StrayDog` |
| `DisplayName` | string | 是 | 中文显示名 | `流浪犬` |
| `ThreatCategory` | enum | 是 | `physical` / `human` / `environment` / `relationship` / `imaginary` / `technology` / `natural` | `physical` |
| `ThreatSource` | string | 是 | 威胁来源 | `dog` |
| `ThreatLevel` | enum | 是 | `low` / `medium` / `high` / `extreme` | `high` |
| `HabitatTag` | string | 否 | 环境标签 | `street` |
| `BehaviorType` | enum | 是 | `chase` / `charge` / `range` / `ambush` / `swarm` / `zone_control` | `chase` |
| `BehaviorTags` | string | 否 | 细分行为标签 | `territory|pressure|close_range` |
| `CounterReactionTags` | string | 否 | 猫面对该敌人的典型应对 | `hide|warning|escape` |
| `BaseHp` | float | 是 | 生命值 | `25` |
| `MoveSpeed` | float | 是 | 移速 | `3.5` |
| `ContactDamage` | float | 是 | 接触伤害 | `3` |
| `AttackInterval` | float | 否 | 攻击间隔 | `1.2` |
| `AggroRange` | float | 否 | 仇恨范围 | `8.0` |
| `DropMaterial` | int | 是 | 击杀掉落材料 | `5` |
| `EliteWeight` | int | 否 | 精英权重，0 表示不可出精英 | `0` |
| `BossWeight` | int | 否 | Boss 权重，0 表示不可出 Boss | `0` |
| `VisualKeywords` | string | 否 | 美术关键词 | `dirty|brown|pressure` |
| `AudioKeywords` | string | 否 | 音效关键词 | `bark|heavy_step` |
| `PrefabKey` | string | 否 | 预制体键 | `Enemy_Dog_Stray` |
| `UnlockWave` | int | 否 | 最早出现波次 | `1` |
| `IsEnabled` | bool | 是 | 是否启用 | `true` |
| `Remark` | string | 否 | 备注 | `基础追击型敌人` |

### 5.3 敌人字段设计说明

- `ThreatCategory` 和 `ThreatSource` 主要来自 `CatEnemy.md`
- `CounterReactionTags` 可直接复用猫咪应对方式，如 `hide`、`warning`、`escape`
- 如果后面做主题地图，`HabitatTag` 可以和街区、森林、屋顶、湿地等场景标签联动

## 六、波次表字段定义

Sheet 名建议：`WaveConfig`

### 6.1 波次表用途

波次表用于定义：

- 每一波持续时间
- 敌人池
- 刷新预算
- 精英和 Boss 规则
- 奖励与商店节奏

### 6.2 字段定义

| 字段名 | 类型 | 必填 | 说明 | 示例 |
|---|---|---|---|---|
| `Id` | string | 是 | 波次唯一 ID | `wave_01` |
| `StageIndex` | int | 是 | 所属章节或地图组 | `1` |
| `WaveIndex` | int | 是 | 第几波 | `1` |
| `DisplayName` | string | 否 | 显示名 | `第1波` |
| `Duration` | float | 是 | 持续秒数 | `20` |
| `NormalEnemyPool` | string | 是 | 普通敌人池，多个用 `|` 分隔 | `enemy_dog_stray_01|enemy_robot_vacuum_01` |
| `EliteEnemyPool` | string | 否 | 精英敌人池 | `` |
| `BossEnemyPool` | string | 否 | Boss 敌人池 | `` |
| `SpawnBudget` | float | 是 | 刷新预算 | `100` |
| `SpawnInterval` | float | 是 | 刷新间隔 | `1.0` |
| `MaxAliveCount` | int | 是 | 同屏最大敌人数 | `25` |
| `ThreatThemeTags` | string | 否 | 本波主题标签 | `dog|territory|street` |
| `EnvironmentPressureTags` | string | 否 | 环境压力标签 | `noise|stranger` |
| `RewardMaterial` | int | 是 | 过波奖励材料 | `30` |
| `RewardChestCount` | int | 否 | 宝箱数 | `0` |
| `ShopRefreshBonus` | int | 否 | 商店刷新补偿 | `0` |
| `BgmKey` | string | 否 | BGM 键 | `BGM_Battle_01` |
| `MapTag` | string | 否 | 地图主题标签 | `street_night` |
| `IsEliteWave` | bool | 是 | 是否精英波 | `false` |
| `IsBossWave` | bool | 是 | 是否 Boss 波 | `false` |
| `Remark` | string | 否 | 备注 | `教学波次` |

### 6.3 波次字段设计说明

- `ThreatThemeTags` 推荐复用 `CatEnemy.md` 中的威胁语义
- `EnvironmentPressureTags` 可以映射 `噪音污染`、`陌生环境`、`陌生动物`、`机器人`
- 如果后续要做更强节奏控制，可以扩充 `SpawnBudgetCurveKey` 或 `DifficultyTier`

## 七、道具表字段定义

Sheet 名建议：`ItemConfig`

### 7.1 道具表用途

道具表用于定义：

- 战斗中的被动物品
- 经济、恢复、幸运、文化祝福类效果
- 来自猫咪喜好、护理、行为、文化象征的主题道具
- 叠加规则与商店出现权重

### 7.2 字段定义

| 字段名 | 类型 | 必填 | 说明 | 示例 |
|---|---|---|---|---|
| `Id` | string | 是 | 道具唯一 ID | `item_catnip_bundle_01` |
| `InternalName` | string | 是 | 英文内部名 | `CatnipBundle` |
| `DisplayName` | string | 是 | 中文显示名 | `猫薄荷香包` |
| `ItemCategory` | enum | 是 | `food` / `toy` / `care` / `tool` / `culture` / `blessing` / `environment` | `food` |
| `ItemSubType` | string | 是 | 细分类 | `catnip` |
| `ThemeSource` | enum | 是 | `likes` / `care` / `behavior` / `culture` / `color` / `feature` / `enemy_response` | `likes` |
| `ThemeTags` | string | 否 | 主题标签 | `catnip|excited|playful` |
| `BreedAffinityTags` | string | 否 | 适配角色或品种标签 | `curious|playful|agile` |
| `Rarity` | enum | 是 | `common` / `uncommon` / `rare` / `epic` / `legendary` | `uncommon` |
| `EffectType` | enum | 是 | `stat_bonus` / `trigger` / `economy` / `healing` / `defense` / `utility` / `curse` / `conversion` | `trigger` |
| `TriggerType` | enum | 是 | `passive` / `on_wave_start` / `on_wave_end` / `on_hit` / `on_kill` / `on_dodge` / `on_take_damage` / `on_shop_enter` | `on_wave_start` |
| `AffectedStats` | string | 否 | 影响属性，多个用 `|` 分隔 | `attack_speed|move_speed` |
| `ValueA` | float | 否 | 主效果值 | `0.15` |
| `ValueB` | float | 否 | 辅助效果值 | `4` |
| `ValueC` | float | 否 | 备用效果值 | `0` |
| `Duration` | float | 否 | 持续时间秒数，常驻填 `0` | `4` |
| `StackRule` | enum | 是 | `additive` / `multiplicative` / `unique` / `replace` | `additive` |
| `MaxStack` | int | 是 | 最大叠加数 | `5` |
| `BasePrice` | int | 是 | 基础价格 | `20` |
| `ShopWeight` | int | 是 | 商店权重 | `80` |
| `IconKey` | string | 否 | 图标键 | `Icon_Item_CatnipBundle` |
| `VfxKey` | string | 否 | 特效键 | `VFX_Buff_Catnip` |
| `SfxKey` | string | 否 | 音效键 | `SFX_Item_Catnip_01` |
| `UnlockCondition` | string | 否 | 解锁条件 | `default` |
| `IsEnabled` | bool | 是 | 是否启用 | `true` |
| `Remark` | string | 否 | 备注 | `喜好系攻速道具样例` |

### 7.3 道具字段设计说明

- `ItemCategory` 主要对应 `CatLikes.md`、`CatCare.md`、`CatCulture.md`
- `ThemeSource` 用于说明道具创意来源，方便后续做风格统一
- `EffectType` 与 `TriggerType` 用于程序侧快速分流逻辑
- `BreedAffinityTags` 不是强绑定，仅用于推荐和池权重扩展
- `BasePrice` 建议保留在道具表中，方便不同商店按倍率调整最终价格

## 八、商店表字段定义

Sheet 名建议：`ShopConfig`

### 8.1 商店表用途

商店表用于定义：

- 不同类型商店的出现时机
- 每次商店展示槽位
- 刷新成本与折扣
- 物品池偏向
- 稀有度权重

### 8.2 字段定义

| 字段名 | 类型 | 必填 | 说明 | 示例 |
|---|---|---|---|---|
| `Id` | string | 是 | 商店唯一 ID | `shop_normal_01` |
| `InternalName` | string | 是 | 英文内部名 | `NormalShop` |
| `DisplayName` | string | 是 | 中文显示名 | `普通补给铺` |
| `ShopType` | enum | 是 | `normal` / `special` / `event` / `boss_reward` / `test` | `normal` |
| `OpenConditionType` | enum | 是 | `post_wave` / `boss_clear` / `manual` / `test_only` | `post_wave` |
| `OpenConditionValue` | string | 否 | 条件值，如波次或事件键 | `default` |
| `UnlockWaveMin` | int | 否 | 最早出现波次 | `1` |
| `UnlockWaveMax` | int | 否 | 最晚出现波次，`0` 表示不限制 | `0` |
| `ThemeTags` | string | 否 | 商店主题标签 | `supplies|street_market|basic` |
| `AllowedItemCategories` | string | 否 | 允许售卖的类别 | `food|toy|care|culture` |
| `GuaranteedOfferTags` | string | 否 | 保底出现标签 | `starter|healing` |
| `BannedOfferTags` | string | 否 | 禁止出现标签 | `legendary_only` |
| `WeaponSlotCount` | int | 是 | 武器槽位数量 | `2` |
| `ItemSlotCount` | int | 是 | 道具槽位数量 | `4` |
| `SupportSlotCount` | int | 否 | 支援/特殊槽位数量 | `0` |
| `FreeRefreshCount` | int | 是 | 免费刷新次数 | `0` |
| `BaseRefreshCost` | int | 是 | 初始刷新价格 | `2` |
| `RefreshCostGrowth` | int | 是 | 单次刷新成长值 | `1` |
| `DiscountRate` | float | 否 | 全局折扣比例 | `0.0` |
| `PriceMultiplier` | float | 是 | 价格倍率 | `1.0` |
| `LockEnabled` | bool | 是 | 是否允许锁定 | `true` |
| `SellEnabled` | bool | 是 | 是否允许回收/卖出 | `true` |
| `RarityWeightCommon` | int | 是 | 普通权重 | `70` |
| `RarityWeightUncommon` | int | 是 | 精良权重 | `22` |
| `RarityWeightRare` | int | 是 | 稀有权重 | `6` |
| `RarityWeightEpic` | int | 是 | 史诗权重 | `2` |
| `RarityWeightLegendary` | int | 是 | 传说权重 | `0` |
| `BackgroundKey` | string | 否 | 背景资源键 | `UI_Shop_Background_Normal` |
| `BgmKey` | string | 否 | 商店 BGM 键 | `BGM_Shop_01` |
| `IsEnabled` | bool | 是 | 是否启用 | `true` |
| `Remark` | string | 否 | 备注 | `默认波后商店样例` |

### 8.3 商店字段设计说明

- `AllowedItemCategories` 让商店可以控制卖什么，不需要直接写死具体池
- `GuaranteedOfferTags` 适合做新手保护、主题商店、幸运商店
- `RarityWeight*` 建议由商店决定，而不是完全写死在全局
- `PriceMultiplier` 让后续特殊商店、折扣商店、黑市商店更容易做

## 九、推荐基础枚举

### 9.1 角色相关

- `CatCategory`: `pet_cat`, `wild_cat`, `color_cat`, `feature_cat`
- `FurType`: `short`, `long`, `hairless`, `partial_hairless`
- `BodyType`: `small`, `medium`, `large`, `giant`
- `RoleClass`: `balanced`, `melee`, `ranged`, `support`, `summon`, `special`

### 9.2 武器相关

- `WeaponClass`: `melee`, `ranged`, `throw`, `summon`, `support`
- `ThemeSource`: `behavior`, `body_part`, `food`, `toy`, `culture`, `enemy_response`
- `DamageType`: `physical`, `bleed`, `magic`, `poison`, `shock`
- `TargetingType`: `nearest`, `front`, `random`, `area`, `self_center`
- `Rarity`: `common`, `uncommon`, `rare`, `epic`, `legendary`

### 9.3 敌人与波次相关

- `ThreatCategory`: `physical`, `human`, `environment`, `relationship`, `imaginary`, `technology`, `natural`
- `ThreatLevel`: `low`, `medium`, `high`, `extreme`
- `BehaviorType`: `chase`, `charge`, `range`, `ambush`, `swarm`, `zone_control`

### 9.4 道具相关

- `ItemCategory`: `food`, `toy`, `care`, `tool`, `culture`, `blessing`, `environment`
- `EffectType`: `stat_bonus`, `trigger`, `economy`, `healing`, `defense`, `utility`, `curse`, `conversion`
- `TriggerType`: `passive`, `on_wave_start`, `on_wave_end`, `on_hit`, `on_kill`, `on_dodge`, `on_take_damage`, `on_shop_enter`
- `StackRule`: `additive`, `multiplicative`, `unique`, `replace`

### 9.5 商店相关

- `ShopType`: `normal`, `special`, `event`, `boss_reward`, `test`
- `OpenConditionType`: `post_wave`, `boss_clear`, `manual`, `test_only`

## 十、落地建议

对当前项目，建议按以下顺序开始实际配表：

1. 先保证 `CharacterConfig`、`WeaponConfig`、`EnemyConfig`、`WaveConfig` 四张核心表跑通
2. 再接入 `ItemConfig` 和 `ShopConfig`，形成完整经济与构筑闭环
3. 先保证每张表字段稳定，再开始批量填内容
4. 先做 8-12 个角色、12-20 把武器、20-40 个道具、6-10 个敌人、20 波以内的可玩闭环
5. 后面再扩展 `Localization`、`AudioConfig`、`DropConfig`

## 十一、已同步输出的模板文件

当前目录同时输出了六张 CSV 模板：

- `CharacterConfigTemplate.csv`
- `WeaponConfigTemplate.csv`
- `EnemyConfigTemplate.csv`
- `WaveConfigTemplate.csv`
- `ItemConfigTemplate.csv`
- `ShopConfigTemplate.csv`

它们可以直接用 Excel 打开后继续填充。
