# 2 系统总线（PCI Express）寄存器

此部分描述了使用PCI Express系统总线时PCI Express的寄存器值。当然，也可以使用其他系统总线，但如果使用的系统总线不是PCI派生，那么该部分不适用（n/a）。

本部分详细说明了NVMe控制器上PCI Header、PCI Capabilities和PCI Express Extended Capabilities的构建方式。显示的字段是从相应的PCI或PCI Express规范中复制的。PCI文档是这些寄存器的规范性规范，本部分详细说明了NVMe控制器的附加要求。

