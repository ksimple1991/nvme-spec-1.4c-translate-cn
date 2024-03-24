# 4 数据结构

本章描述了NVM Express中使用的数据结构。

## 4.1 SQ 和 CQ 的定义

第 4.1 节、第 4.1.1 节和第 4.1.2 节仅适用于 NVMe over PCIe 实现。对于 NVMe over Fabrics 实现，请参阅第 2.4 节以及 NVMe over Fabrics 规范中的该节小节。

队列条目的提交者使用当前的 Tail entry pointer 来指示队列的下一个空位，将新条目放置到空位后，提交者将增加移动 Tail entry pointer 。如果 Tail entry pointer 增加超过队列大小，则 Tail entry 应该回滚到零。提交者可以继续在空位中放置条目，除非队列已满（参见第4.1.2节）。

注：提交者应考虑排队条件。

队列条目的消费者使用当前 Head entry pointer 来指示下一个要被消费的条目位置。消费者在从队列中消费下一个条目后，增加 Head entry pointer 。如果 Head entry pointer 增加超过队列大小，则 Head entry pointer 应回滚到零。只要不满足空队列条件（参见第4.1.1节），消费者就可以继续从队列中消费条目。

注：消费者应考虑排队条件。

主机软件需要正确排序 SQ 和对应 CQ 的创建和删除命令。主机软件应该先创建完成队列，再创建与之关联的提交队列，提交队列可以随时创建。与之相反，主机软件若要删除完成队列，则需要先删除所有关联的提交队列。如果要终止提交到提交队列的所有命令，主机软件可以直接发送 Delete I/O SQ 命令。（参见第 7.4.3 节）。

主机软件写 Submission Queue Tail Doorbell （参见第3.1.25节）和 Completion Queue Head Doorbell （参见第3. 1.26节）寄存器来与控制器传递相应条目指针的新值。如果主机软件将无效值写入 Submission Queue Tail Doorbell 或 Completion Queue Head Doorbell 寄存器，并且还有 Asynchronous Event Request 命令未完成，那么这个 Asynchronous Event Request 将被发到 Admin CQ，同时状态码被标记为Invalid Doorbell Write Value。主机软件应删除并重新创建相关队列。对于出现此错误的提交队列，控制器可能（may）需要完成之前的消费命令；不会消耗任何其他命令。该错误可能由主机软件试图在已满的提交队列添加条目或从空完成队列中删除条目引起。









