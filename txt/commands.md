## 找system的dtb用的是哪一个

## 查看设备树

```
nvidia@orin:/sys/firmware/devicetree/base$ ls -l
nvidia@orin:/sys/firmware/devicetree/base$ cat compatible 

// 所有不是drwxr开头的都是可以用  cat 命令打开看的
```

## compile dtbs

```
export CROSS_COMPILE=/home/jiajia/l4t-gcc/aarch64--glibc--stable-2022.08-1/bin/aarch64-buildroot-linux-gnu-
export KERNEL_HEADERS=$PWD/kernel/kernel-jammy-src
make dtbs
```

## compile之后生成的dtb的存放路径：

cd ~/LinuxKernel/Linux_for_Tegra/source/nvidia-oot/device-tree/platform/generic-dts/dtbs

## 所有包含.dts文件的地址，修改并编译

jiajia@zjj:~/LinuxKernel/Linux_for_Tegra$ find | grep "tegra234-p3737-0000+p3701-0000.dts"
./source/hardware/nvidia/t23x/nv-public/tegra234-p3737-0000+p3701-0000.dts
./source/kernel/kernel-jammy-src/arch/arm64/boot/dts/nvidia/tegra234-p3737-0000+p3701-0000.dts
./source/out/nvidia-linux-header/scripts/dtc/include-prefixes/arm64/nvidia/tegra234-p3737-0000+p3701-0000.dts
./source/out/nvidia-linux-header/arch/arm64/boot/dts/nvidia/tegra234-p3737-0000+p3701-0000.dts

## 我们需要改变的dts文件是：

jiajia@zjj:~/LinuxKernel/Linux_for_Tegra/source/hardware/nvidia/t23x/nv-public/tegra234-p3737-0000+p3701-0000.dts

###### 这个 **dts** 文件改了并编译之后，去以下路径寻找生成的dtb

cd ~/LinuxKernel/Linux_for_Tegra/source/nvidia-oot/device-tree/platform/generic-dts/dtbs

###### 生成的是，不要忘了有  -nv 后缀：

tegra234-p3737-0000+p3701-0000-nv.dtb

## 在本地测试：

去生成的dtb文件里找添加的代码：

jiajia@zjj:~/LinuxKernel/Linux_for_Tegra/source/nvidia-oot/device-tree/platform/generic-dts/dtbs

```
$ strings tegra234-p3737-0000+p3701-0000.dtb|grep "jiajia"
```

## 上传到sam 和 orin 板上并重启

```
cd ~/LinuxKernel/Linux_for_Tegra/source/nvidia-oot/device-tree/platform/generic-dts/dtbs
scp tegra234-p3737-0000+p3701-0000-nv.dtb jiajia@sam:/tftpboot/orin/
ssh sam
cd /tftpboot/orin/
scp tegra234-p3737-0000+p3701-0000-nv.dtb nvidia@orin:/home/nvidia/

### 登陆nvidia 板子
taco target orin -s
### 将dtb文件移动到对的文件夹 /boot/dtb/
sudo mv /home/nvidia/tegra234-p3737-0000+p3701-0000-nv.dtb /boot/dtb/
### 进入文件夹
cd /boot/dtb
### 用新上传的dtb文件替换原本的 kernel_tegra234-p3737-0000+p3701-0000-nv.dtb
sudo cp tegra234-p3737-0000+p3701-0000-nv.dtb kernel_tegra234-p3737-0000+p3701-0000-nv.dtb
### 重启系统，L4T
sudo reboot
```

## 在orin板上reboot之后，再测试

```
### 在这里找有没有刚改变过的node名
cd /proc/device-tree/
ls
### 我们可以找到jiajia文件夹，里面有hello name, 我们看hello:
cd jiajia
cat hello
```

### 查看orin板上的内存

```
nvidia@orin:~$ sudo cat /proc/iomem
```

# 
