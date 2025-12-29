# tosu-obs-overlay

给朋友做的 tosu obs overlay。
没有公开计划。
版本为 rolling。

这个 README 只是 todo list。

## 目标

- 尽量让观众看得有趣（假设观众无音游经验）
- 尽量平铺 debug 级别的信息
- 尽量吸收其他音游的东西

## 实现

- [x] 普通的内容实现（比如，所有难度种类及其图表）
- [x] focus session：显式声明难点，让观众知道难在哪里
- [x] 精确 acc（五位数）：手动计算，避免 mania 的四舍五入 "95.00%" 但还是 A 的情况

## 灵感

### standard

- [x] focus session：最开始就是为 std 设计的，只是后来给其他模式用上了

### taiko

- [x] 摆正 taiko 在图表的位置，而不是单在左侧

### mania

#### iidx-like

- [x] 新结算界面：beatoraja 结算页面下的三个图表 <https://youtu.be/DhfmOhBDf0I>
- [ ] beatoraja 占位零：`PF: 0194` 里面的 `0` 可见度调低点
  - 不实现理由：一张 om 谱可能有 10k+ notes，但是 iidx 谱面只有 10k-
- [x] `判定傾向`：平均误差 <https://youtu.be/DhfmOhBDf0I>
- [ ] `MAX-`, `SS-`, ...

#### etterna

- [ ] flags/cleartypes
- [ ] grades 动画（不停 rolling，最后 rolling 到真的等级）
  - 不实现理由：osu! 的评级并不是黑箱，不过实现起来非常简单，无所谓
- [ ] 新结算界面：散点图

### catch

- [ ] 有人在意
