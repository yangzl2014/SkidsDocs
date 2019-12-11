.. _neuibituse:

Skids的使用
============================

Skids连接PC
----------------------------

1. Skids无需额外的调试器， Skids开发板的USB接口在侧面，通过USB线连接至PC即可。
#. Skids通过USB线连接至PC后，开启电源开关（向上拨开关），设备上电启动，屏幕点亮。
#. Skids连接至PC后，会自动进行驱动安装，无需人为操作。
#. 安装完驱动后，在设备管理器中会出现相应的串口。

Skids开发环境
----------------------------

- Skids集成了Python解释器和驱动库，开发简单、使用方便，无需搭建复杂的交叉开发环境，可实现学生的快速入门。
- Skids只需要一个名为uPyCraft的工具即可进行代码编辑、下载和运行。
- uPyCraft是一个可运行在Windows/MacOS平台的Python IDE，界面简洁，操作便利，适合新手的学习和使用。
- uPyCraft内置了许多基础操作库，为众多的Python爱好者提供了一个简单实用的集成开发环境。

.. image:: img/upycraft.png
    :alt: uPyCraft
    :width: 640px

通过uPyCraft访问Skids
----------------------------

1. 通过USB将Skids连接到PC。
#. 在uPyCraft的主菜单上，选择Tools -> Serial，选中对应的串口即可。
#. 连接成功后，串口号前面会出现一个对号。
#. 在左侧目录树中的Device选项，前面会出现小箭头，点击可显示Skids中的文件列表。
#. 在下侧终端框中按CTRL+C健可对Skids进行软复位。

Skids的固件
----------------------------

- 为了确保Skids正常运行，需要为Skids烧录固件。
- Skids出厂时会统一烧录固件。但如果升级或者修复固件，则需要通过uPyCraft重新为Skids烧录固件。
- Skids的固件为二进制文件，通常命名为firmware.bin。

通过uPyCraft烧录固件
----------------------------

1. 在uPyCraft的主菜单上，选择Tools -> BurnFireware。
#. 烧录固件对话框将被弹出，在burn_addr选项中选择0x1000，在Firmware Choose选项中选中Users，点击Choose按钮，从本地目录中选择要烧录的固件。
#. 选中待烧录的固件后，点击OK将开始烧录固件，并弹出如下窗口显示进度。
#. 固件烧录完成后，该窗口自动关闭，返回uPyCraft主界面；同时，Skids设备将自动重启。
#. Skids重启后会与uPyCraft断开连接，用户重新在主菜单Tool s-> Serial中选择对应的串口进行连接。

.. image:: img/upycraft_burn.png
    :alt: uPyCraft
    :width: 320px

运行Python文件
----------------------------

- 如果要执行Skids上的某个Python文件，在uPyCraft中选中该文件后，点击鼠标右键，在弹出菜单中选择Run，即可执行该文件。
- 如果要执行PC本地的某个Python文件，选中该文件后，点击右侧工具栏的DownloadAndRun按钮即可。
- 如果要执行PC本地的某个Python文件，选中该文件后，也可以直接将文件拖拽至device列表中，则该文件会被自动下载到Skids。
- 如果终止正在运行的Python程序，则点击右侧工具栏的Stop按钮即可。

Skids文件结构
----------------------------

- boot.py ：开发板启动时将执行这个该脚本，通常在该脚本中设置开发板的主要参数。
- main.py ：python 主程序的脚本文件，在 boot.py 运行后被执行。如果main.py不存在，则boot.py执行完成后，MCU处于空闲状态。
- 其它Python文件 ：python 程序文件，由main.py调用运行或者通过uPyCraft手动运行。
