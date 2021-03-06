> 如何运作

引擎使用以下技术:

- 时间校正位置Verlet积分器
- 自适应网格宽相检测
- AABB中期检测
- SAT窄相检测
- 迭代序列脉冲求解器和位置求解器
- 使用静止约束休息碰撞（Erin Catto，GDC08）
- 时间相干冲动缓存和变暖
- 由配对管理器维护的碰撞对、接触和脉冲
- 使用摩擦约束的近似库仑摩擦模型
- 使用Gauss-Siedel方法解决约束
- 固定或可变时间步长
- 一种基本的睡眠策略

有关更多信息，请参阅[有关初学者的游戏物理学](http://brm.io/game-physics-for-beginners/)文章。
