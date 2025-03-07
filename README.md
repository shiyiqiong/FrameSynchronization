# 帧同步
**帧同步实现流程：**
- 确定逻辑运行固定帧率；
- 由服务器推帧运行；
- 将客户端输入发送至服务器；
- 由服务器将客户端输入同步给各服客户端；
- 各客户端收到输入后再做逻辑运算；
- 最终保证在相同帧和相同输入情况下，各个客户端模拟出相同逻辑结果。
***
**预测机制**
- 客户端逻辑和表现分离；
- 客户端预测：未收到服务器数据，在已知状态下预测下一步动作；
- 服务器确认：收到服务器结果进行比较；
- 逻辑层回滚重计算：客户端回滚到最后已知正确状态，根据实际后端数据，重新计算下一步动作；
- 表现层：通过插值平滑，从预测状态平滑过渡到时间正确状态。
***
**对抗外挂**
- 服务器做输入验证；
- 关键状态，所有客户端同步状态给服务器，状态不一致，少数服从多数；
- 服务器做核心逻辑计算，关键判断又服务器做出。
***
**可靠UDP**
- 接收方收到数据后，需要回复发送方，确认收到；
- 如果接收方未收到确认，需要重发信息；
- 发送数据是需要带上序号；
- 接收方收到序号是，需要在进行排序处理。
***
**遇到问题和对应解决方案：**
- 保证所有客户端计算结果一致：
  - 逻辑和显示分离。
  - 浮点数处理：改用定点数，小数部分用整数存储。
  - 随机数处理：伪随机，设置统一随机种子，确保各个客户端随机数调用顺序和调用次数一致。
  - 容器遍历，确保顺序一致（字典以key排序后，进行遍历）。
  - 三角函数计算，使用查表法，读取预计算结果。
  -  物理引擎：改用定点数物理引擎（BEUP Physicsint）。
- 卡顿和延迟：
  - 缓存帧，开业降低卡顿，但会导致延迟；
  - 不设置卡顿帧，将逻辑和表现分离（逻辑帧率：20FPS，表现帧率：60FPS），逻辑层保证及时性避免延迟，表现层做插值平滑处理降低卡顿。  
- 优化客户端操作手感：
  - 帧预测；
  - 插值平滑；
  - 结果校正。
- 不同步问题总结：
  - 浮点数；
  - 随机数；
  - 顺序不确定的容器遍历；
