## anchor

- 是什么：固定的参考框
- 作用：首先预设一组不同尺度不同位置的固定参考框，覆盖几乎所有位置和尺度，**每个参考框负责检测与其交并比大于阈值 (训练预设值，常用0.5或0.7) 的目标**，anchor技术将问题转换为**"这个固定参考框中有没有认识的目标，目标框偏离参考框多远"**，不再需要多尺度遍历滑窗，真正实现了又好又快。

