# Skin

## Elements

研究一下怎么设计模块（AKA 轮子），from code 来生成一个 Skin。概念出自 Iosevka。

### 目标

- 小工具集合（DOTADIW）
- 无 `skin.ini` 魔法

### 模块设计

以下每个元素都是使用单个脚本文件完成的。

简单模块：

- [x] `combos-*` - 不强制等宽
- [x] `default-*` - 不强制等宽
- [x] `numbers-*` - 在程序上强制
- [ ] `scoreentry-*` - 不强制等宽

多功能脚本：
- hitcircle
    - Overlay
        - Border size
        - Overlay shadow
            - No shadow
            - Shadow out
            - Shadow in/out
            - Shadow blur
            - Shadow color
            - Shadow opacity
- Slider
    - Same slider circles than hitcircles
    - Overlay
        - Border size
        - Overlay color
        - Overlay shadow
            - No shadow
            - Shadow out
            - Shadow in/out
            - Shadow blur
            - Shadow color
            - Shadow opacity
- `hit-*-*`
  - [x] 简单形状：胶囊、点、三角形、正方形、圆形、 交叉
  - 设计力场来设计 glow

## Judgments

判定实际上是一种确定语义色的活。目前 ppy 在这上面做的功夫就是标准化。我对此有不同看法，并且我想使用 <https://github.com/Ks4four/moe-palettes-ksfour>。

### 存值

在 stable，ppy 采用已有的计数器来存值，而不是各个 mode 都有独立的存值。

| 判定 | Standard | Taiko | Catch | Mania |
| :--- | :--- | :--- | :--- | :--- |
| Geki (激) | Combo 结尾（300） | 大饼（良） | Combo 结尾 | 320 (MAX) |
| 300 | 300 | 良 | Fruits | 300 |
| Katu (喝) | Combo 结尾 （100） | 大饼（可） | Missed Droplets | 200 |
| 100 | 100 | 可 | Drops | 100 |
| 50 | 50 | - | Droplets | 50 |
| Miss | Miss | 不可 | Missed Fruits & Drops | Miss |

### 配色

| osu! mania | 颜色 |
| :--- | :--- |
| 320 | 彩色 |
| 300 | 金色 |
| 200 | 绿色 |
| 100 | 蓝色 |
| 50 | 灰色 |
| 0 | 红色 |

| lazer! standard | 颜色 |
| :--- | :--- |
| 300 | 明蓝色 |
| 200 | 暗绿色 |
| 50 | 黄色 |
| 0 | 红色 |

| lazer! mania | 颜色 |
| :--- | :--- |
| 320 | 明蓝色 |
| 300 | 暗蓝色 |
| 200 | 明绿色 |
| 100 | 暗绿色 |
| 50 | 黄色 |
| 0 | 红色 |



|  SM5 | 颜色 |
| :--- | :--- |
| flawless | 明蓝色 |
| perfect | 金色 |
| great | 绿色 |
| good | 暗蓝色 |
| bad | 紫色 |
| miss | 红色 |

SM5 可参看：<https://github.com/stepmania/stepmania/blob/d55acb1ba26f1c5b5e3048d6d6c0bd116625216f/Themes/default/Graphics/Judgment%20Normal%202x6.png>。

| iidx 31 | 颜色 |
| :--- | :--- |
| pg | 青色 |
| gr | 金色 |
| gd | 暗黄色 |
| bd | 暗橙色 |
| pr | 深红色 |
| cb | 外围是红色的白色 |
| fast | 亮蓝色 |
| slow | 亮红色 |

# tosu-obs-overlay

给朋友做的 tosu obs overlay。

## 目标

- 尽量让观众看得有趣（假设观众无音游经验）
- 尽量平铺 debug 级别的信息
- 尽量吸收其他音游的东西
- **Modular by design, batteries-included by default**

## 功能

- [x] 「**电池包含在内**」级别的普通内容展示
- [x] focus session：当谱面进行到难点时，就显式显示
- [x] 精确 acc（五位数）：手动计算，避免 mania 的四舍五入 "95.00%" 但还是 A 的情况
- [ ] **模块化。**

## 灵感

许多实现是我后来发现没什么用的，所以就不用 TODO List 了。

### standard

- [x] focus session：最开始就是为 std 设计的，只是后来给其他模式用上了

### taiko

- [x] 摆正 taiko 的 graph 的位置，而不是单在左侧

### mania

#### iidx-like

- [x] 新结算界面：beatoraja 结算页面下的三个图表 <https://youtu.be/DhfmOhBDf0I>
- [x] `判定傾向`：平均误差 <https://youtu.be/DhfmOhBDf0I>
- [ ] 数字血量
- beatoraja 占位零：`PF: 0194` 里面的 `0` 可见度调低点
  - 不实现理由：一张 om 谱可能有 10k+ notes，但是 iidx 谱面只有 10k-
- `MAX-`, `SS-`, ...

#### etterna

- [ ] 新结算界面：散点图
- flags/cleartypes
  - 不实现理由：感觉没什么人在意
- grades 动画（不停 rolling，最后 rolling 到真的等级）
  - 不实现理由：osu! 的评级并不是黑箱，不过实现起来非常简单，无所谓

### catch

- [ ] 有人在意

## 设计

虽然 tosu 中的 metadata 将 layout 分为 obs 和 ingame，但是在我看来实际情况是 ingame 和 stream。
- ingame layout 是侵入式的。trade off 是它要求 osu! 原本空白的地方。欲设计一个这样的 layout，实际上空白部分很少：[/asset/osu.svg](/asset/osu.svg)，并且观众还无法看到精心设计的 private skin（如果有）。
- stream layout 是非侵入式的。它可以摆放许多个性设计，比如说可以摆放各种动漫角色，或者 l2d。trade off 就是你无法获得源大小的游戏界面。

我第一个项目是 ingame layout，因为我使用 2180p 的屏幕，我没有兴趣使用真正的 stream layout。在这上面我使用了激进的设计：
1. 把我认为观众不感兴趣的地方全部用上了，比如说 Sort by、Group by、playfield 的上方、排行榜、血条、结算页面。
2. 改进和替换不可替换的电池：hit error、key overlay、acc、分数、mod、combo。
3. 没有挡住 fps 和 帧生成时间。这部分留着放手元。

如果你想做 stream layout 方向，我觉得可以实现（但我懒得做或者我试过）的是动画。这方面的审美参考我觉得是像现实中的パチンコ机器那种。立刻可以工作的方案是使用 iidx 素材。
1. fail 动画：播放一个 mp4，然后使用 canva 滤除蓝幕。gif 是备选项。搜索「iidx 閉店演出」。
2. 结算动画：可以做评分的动画，也可以做 flags/cleartypes 的动画（比如 fc）。立刻可以工作的方案有两种：Stepmania 3.9 滚动的评分，和进度条样式（lazer! 做了）。
3. 画面切り替え：玩家在不同界面中切换时（除了暂停，因为暂停不算独立的界面）播放动画。这个很有可能效果不好，因为它有体感延迟（无法处理）。
4. 直接照抄各种游戏的 layout，这需要各个游戏的 assets。我是懒得弄，不过纯网页的上限很高，不至于不能做。

### 程序

我的推流现在全是 learn bait，点进去一看又是老生常谈话题就为了换个名词解释。为了好玩，这里收集一些套话。如果你有意让你的项目变得更好（营销上的），可以参看。

| 类 | 问题 | 包含着 |
| - | - | - |
| 边界与耦合 | 我刚下载的东西去哪了 | 边界清晰、可替换性、接口稳定性、数据所有权、安全边界、依赖数量 |
| 渐进性 | 更新的都什么大便 | 渐进采用、可学习性、向后兼容、schema演进 |
| 可观测性 | 你看又卡 | 可观测性、错误信息、调试体验 |
| 失败模型 | 按下重试了之后帧数又掉了，给我气笑了 | 失败模式、幂等性 |
| 状态模型 | 我的数据呢？？？ | 状态位置、数据所有权、schema演进 |
| 资源模型 | 一个 js 能用 8g 内存谁家 java | 资源边界、性能可预测、并发模型、生命周期 |
| 默认行为 | 打开之后啥也没显示 | 默认值、最小惊讶、安全边界 |
| 可逃逸性 | 这个字体好丑还改不了 | 逃生舱、可测试性 |

一个更好的问题是，一个更好的问题究竟是由谁提出的，而谁提出的问题是否是一个更好的问题并不是一个更好的问题。

# 推测地图类型

- 此目标为 tosu-obs-overlay 的支线任务。
- 此目标为**基础设施**。

Etterna 可以推测谱面类型。没看过它的实现，因为 std 模式更重要一点。对现有工具进行猜测：

- [ ] 0. 等待 User tags（<https://osu.ppy.sh/wiki/en/Beatmap/Beatmap_tags>）成为事实标准
- 1. 准备训练数据（[Osynicite/osynic_serializer](https://github.com/Osynicite/osynic_serializer) + [Osynicite/osynic_downloader](https://github.com/Osynicite/osynic_downloader), 还有自己弄的 api 获取对应 tags 的 beatmaps）
- 2. 训练（[OliBomby/CM3P](https://github.com/OliBomby/CM3P)）
- 3. 推理（`FastAPI`/`AutoModel` + `AutoProcessor`/`Flask`）
- 4. 中间件
- 5. 模型部署（`onnxruntime`, Node.js）
- 6. 前端（Node.js, tosu）
- 7. 缓存（`Redis`）

目前阶段卡在 0.。我希望有一个下载 Beatmap sets 之后删除其余 beatmap (chart/diff/file) 的功能。目前现有的工具似乎还做不到。

## 推测玩家喜欢的谱面

这个是最实际的。我们现在有 CM3P，所以只需要随便搞几十张图，然后训练出向量就完事了。

# Booru-like previewing in web

- 此目标为 tosu-obs-overlay 的支线任务。

这游戏十几年了，我还是不懂为什么不能预览谱面，这可能是因为滑条数学吧。无论如何，从零手搓似乎复杂度不高。

我打算做一个 Booru 一样的网站实例（而不是真的可行网站）。左边是 tags，右边是谱面预览。谱面预览一定要能按时间滑动。

理论上可以纯静态实现。动态只是方便添加 tags 和 beatmaps。但是我不在乎。

# lazer 哲学

we get what we deserve

## stable 调试

根据 tosu 发现 stable 有奇怪的现象。暂且不向 tosu 仓库提出 issue。

- 无论玩家游玩的是否为转谱，stable 客户端总是有概率将游玩模式切回 standard。
- 被注入时分数存储的地方改变，会变为负数。未知是否有其他地方存储分数。
