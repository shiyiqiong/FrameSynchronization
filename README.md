# 帧同步
**帧同步实现流程：**
- 确定逻辑运行固定帧率；
- 由服务器推帧运行；
- 将客户端输入发送至服务器；
- 由服务器将客户端输入同步给各服客户端；
- 各客户端收到输入后再做逻辑运算；
- 最终保证在相同帧和相同输入情况下，各个客户端模拟出相同逻辑结果。
***
**遇到问题和对应解决方案：**
- 保证所有客户端计算结果一致：
  - 逻辑和显示分离。
  - 浮点数处理：改用定点数，小数部分用整数存储。
  - 随机数处理：伪随机，设置统一随机种子，确保各个客户端随机数调用顺序和调用次数一致。
  - 三角函数计算，使用查表法，读取预计算结果。
  -  物理引擎：改用定点数物理引擎（BEUP Physicsint）。
- 优化客户端操作手感：
  - 帧预测；
  - 插值平滑；
  - 结果校正。
- 不同步问题总结：
  - 浮点数；
  - 随机数；
  - 顺序不确定的容器遍历；
