# 简介
  这个项目是Marvell的88MW30X系列wifi芯片的解决方案，提供了各种文档及例子。
  QQ讨论群：5409372
  微信公众号：cauiot

# 使用方法
  购买开发板后，下载该项目，用``` git clone https://github.com/windsome/IotMarvellSolution ```
## 环境搭建(Ubuntu系统)
### 软件安装
  1，安装openocd，在ubuntu下可以直接运行命令 
    ``` sudo apt-get install openocd ```
  2，安装gdb-arm-none-eabi，在ubuntu下可以直接运行命令 
    ``` sudo apt-get install gdb-arm-none-eabi ```
    安装过程中可能根原有的gcc的gdb相冲突，可以将原gdb卸载

### openocd使用方法
  参考http://openocd.org/doc/html/GDB-and-OpenOCD.html#GDB-and-OpenOCD
  0，将开发板与电脑通过miniUSB口连接好，连接之后开发板即自动上电，运行 ``` ls /dev/ttyUSB* ```，如果看到有/dev/ttyUSB0和/dev/ttyUSB1则表示jtag连接成功，可以进行调试及后续工作。/dev/ttyUSB0是jtag口，/dev/ttyUSB1是与板子交互的串口终端。
  1，打开一个终端，在ubuntu上用root用户运行，将打开ubuntu主机的3333(gdb),4444(telnet),6666(tcl)端口
    ```
    $ cd IotMarvellSolution/OpenOCD
    $ su root #回车后输入密码进入root
    # openocd -s interface -f ftdi.cfg -f openocd.cfg
    ```
  2，打开一个新的终端页，使用arm-none-eabi-gdb进行调试
    ```
    $ cd sample_apps/hello_world/bin
    $ arm-none-eabi-gdb hello_world.axf
    ```
  3，在arm-none-eabi-gdb中调试
    ```
    target remote localhost:3333
    monitor reset halt
    load
    continue
    ```
### flashprog.sh使用
  0，将开发板与电脑通过miniUSB口连接好，连接之后开发板即自动上电，运行 ``` ls /dev/ttyUSB* ```，如果看到有/dev/ttyUSB0和/dev/ttyUSB1则表示jtag连接成功，可以进行调试及后续工作。/dev/ttyUSB0是jtag口，/dev/ttyUSB1是与板子交互的串口终端。
  1，用root运行flashprog.sh命令
    ```
    $ cd IotMarvellSolution/OpenOCD
    $ su root #回车后输入密码进入root
    # ./flashprog.sh
    ```
    运行后，会提示出命令界面
    ```
    * 1.Write boot2 image (with bootrom header) boot2.bin
    * 2.Write firmware image (mc200fw) pm_mc200_demo.bin
    * 3.Write file system image/s wm_demo.ftfs perf_demo.ftfs wlan_prov.ftfs
    * 4.Write wireless firmware image (wififw) uart_wifi_bridge.bin
    * 5.Partition information layout.txt
    * 6.Advanced Mode
    * q.Exit
    ```
  2，如果要烧写某个分区可以直接敲入那个分区对应的数字，比如烧入fw，敲入2，然后输入文件所在路径
  3，如果退出，直接按q
  
