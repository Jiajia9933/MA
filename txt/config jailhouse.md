######

## Nvidia 板子的信息：

##### 进入 cd /proc/ 文件夹

-  **iomem**：显示物理内存和设备的地址空间映射。

- **config.gz**：包含已压缩的内核配置文件，可以通过解压此文件来查看内核是如何配置的。

- **meminfo**：显示系统内存使用的详细信息。

- **device-tree**：指向系统设备树（device tree）的基本信息，这个目录用于表示硬件配置。









# 1. 设备树中为 Jailhouse 预留内存

##### 

##### 在以下文件里

jiajia@zjj:~/LinuxKernel/Linux_for_Tegra/source/hardware/nvidia/t23x/nv-public/tegra234-p3737-0000+p3701-0000.dts

##### 添加：

```
reserved-memory {
    jailhouse_mem: jailhouse_mem@70000000 {
        no-map;
        reg = <0x1 0x70000000 0x0 0x03000000>;
    	};
	}
```





# 2. 为 jailhouse 设置 Kernel 的 menuconfig

```
cd ~/LinuxKernel/Linux_for_Tegra/source/kernel/kernel-jammy-src
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- -j4 menuconfig
```
