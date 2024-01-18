
## 1.6 术语解释
### 1.6.1 Admin队列

Admin队列是ID为0的提交队列和完成队列。Admin提交队列用于提交管理命令，相应的Admin完成队列用于接收这些管理命令的回应。

Admin提交队列与Admin完成队列唯一关联。

### 1.6.2 管理控制器

控制器会对外公开允许主机管理NVM子系统的能力。管理控制器不需要实现IO队列，以及提供NVM存储介质上的用户数据和元数据，或是支持关联到管理控制器上的namespace。

### 1.6.3 仲裁突发(arbitration burst)

仲裁机制一次可以从提交队列中获取的最大命令数。

### 1.6.4 仲裁机制（arbitration mechanism）

用于确定接下来选择哪个SQ的命令以供控制器执行的方法，有三种机制：

- round robin
- weighted round robin with urgent priority class
- 厂商自定义

### 1.6.5 缓存（cache）

指NVM子系统使用的数据存储区域。对主机不可见，可能包含一部分存储在NVM介质的用户数据，也可能包含未提交到NVM介质的用户数据。

### 1.6.6 候选命令（**candidate command**）

候选命令指的是已传输到控制器，并且控制器认为已经准备好进行处理。

### 1.6.7 命令完成（**command completion**） 

当控制器已经处理完该命令，并且更新了完成队列条目的状态信息，并将完成队列条目提交到了完成队列。

### 1.6.8 命令提交（**command submission**）

对于NVMe over PCIe的实现，当提交队列的Tail DoorBell寄存器更新完成，也就代表了

对于NVMe over Fabrics的实现，请参考NVMe over Fabrics specification 的1.4.14章节。

### 1.6.9 控制器（**controller**）

控制器是指主机与NVM子系统之间的接口，其有三种类型：

- I/O控制器 I/O controllers
- 发现控制器 Discovery controllers
- 管理控制器 Administrative controllers

控制器运行主机提交到提交队列的命令，并在完成队列中发布完成消息。所有控制器实现一个Admin提交队列和一个Admin完成队列。根据具体的控制器类型，控制器还可以实现一个或者多个IO的提交队列和完成队列。当PCI Express作为传输层时，控制器就是PCI Express function。

### 1.6.10 指令（directive）

主机与NVM子系统或控制器之间信息交换的方法。信息通过使用Directive Send和Directive Receive命令传输。I/O命令的子集可以包含Directive Type和Directive Specific字段以传递关联I/O命令的更多特定信息。参考第九章。

### 1.6.11 discovery controller

Discovery controller 表明具备允许主机接收Discovery 日志页的能力。Discovery controller 不实现I/O队列，也不访问NVM介质。参考NVMe over Fabrics specification获取更多信息。

### 1.6.12 仿真控制器（emulated controller）

软件定义的 NVM Express 控制器。仿真控制器可能有也可能没有底层物理 NVMe 控制器（例如，物理的 PCIe function）。

### 1.6.13 Endurance Group

NVM 子系统中 NVM 的一部分，其耐久性作为一个组进行管理。请参考8.17章节。

### 1.6.14 扩展LBA（Extended LBA）

扩展LBA是一种较大的LBA，当元数据相关的LBA与LBA数据连续传输时，扩展LBA被创建。参考图453。





