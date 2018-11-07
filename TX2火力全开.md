# nvidia jetson TX2 火力全开
TX2开发板启动后，开发板上的小风扇是不会旋转的，估计是因为TX2开发板默认的功率不大，还不需要小风扇散热。说到功率，NVIDIA的新的命令工具Nvpmodel，提供了5种模式，供使用者调整CPU与GPU的运行状态。

| 模式     | 模式名 | Denver 2|频率|ARM A57|Frequency|GPU 频率|
| ---     | --- | --- | --- | --- | ---|---|
|0|Max-N    |2|2.0 GHz|4|2.0 GHz|1.30 Ghz|
|1|Max-Q|0  | |4|1.2 Ghz|0.85 Ghz|
|2|Max-P Core-All|2|1.4 GHz|4|1.4 GHz|1.12 Ghz|
|3|Max-P ARM|0| |4|2.0 GHz|1.12 Ghz
|4|Max-P Denver|2|2.0 GHz|0| |1.12 Ghz

## NVIDIA提供了命令对进行切换和查询。

* 查询当前模式
```sh
sudo nvpmodel -q –verbose
```
* 切换当前模式（例如切换至模式0）
```sh
sudo nvpmodel -m 0
```
## NVIDIA提供脚本文件进行切换

TX2开发板的主文件夹也就是 __/home/nvidia__ 下面会有一个名为 **jetson_clocks.sh** 的文件，直接用命令行运行就能切换到最高功率、火力全开：
```sh
sudo sh ~/jetson_clocks.sh
```
***遇到的问题*：** 不过我在TX2上运行这个脚本时出现错误，经过排查，我的判断是在385行的判断语句对384行的 **grep "nvidia,tegra210" /proc/device-tree/compatible &>/dev/null** 的判断逻辑错误，本意是只要找到TX1开发板的标志就判断是TX1开发板，否则是TX2，但逻辑错误导致无论如何都会判断为TX1。

***解决方案:*** 只要删掉384行的 **&>/dev/null** 就可以运行了
