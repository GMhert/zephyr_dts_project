.. _boards_custom:

自定义板级支持包说明
=====================

本目录为 Zephyr 项目中的 **自定义板级支持包（Board Support Package, BSP）**，用于支持特定硬件平台，如 ``STM32F103ZET6`` 和 ``STM32F429IGT6``。

架构设计
********

本 BSP 采用模块化结构，便于复用与扩展：

- ``common/``：存放多个板子共用的设备树片段（如 LED、UART 引脚映射、时钟配置等）。
- ``<board-name>/``：每个板子独立目录，包含：
  - ``<board>.dts``：主设备树文件，定义外设资源、引脚复用、内存布局等。
  - ``<board>.dtsi``：可选的设备树包含文件（如 SoC 级配置）。
  - ``pinmux.c`` / ``clock_control.c``：可选的 C 语言初始化代码（若需动态配置）。
- ``vendor/``：预留用于第三方开发板或商业产品支持。

当前支持板型
**************

### 📌 STM32F103ZET6

- 芯片：STMicroelectronics STM32F103ZET6（LQFP144 封装，72MHz，128KB RAM，512KB Flash）
- 主要外设支持：
  - USART1（PA9/TX, PA10/RX）→ 可作为 Shell 控制台
  - USART2（PA2/TX, PA3/RX）→ 应用串口
  - LED（PC13）→ 用户指示灯
  - 外部晶振 8MHz → PLL 倍频至 72MHz
- 设备树文件：`stm32f103zet6/stm32f103zet6.dts`

### 📌 STM32F429IGT6

- 芯片：STMicroelectronics STM32F429IGT6（LQFP176 封装，180MHz，256KB RAM，1MB Flash）
- 主要外设支持：
  - UART1~8（多路串口）
  - LTDC + DMA2D → 支持 RGB 屏幕驱动
  - SDIO → 支持 SD 卡
  - ETH → 支持以太网
- 设备树文件：`stm32f429igt6/stm32f429igt6.dts`

使用方法
**********

在你的应用工程中，通过以下方式指定目标板：

```bash
west build -b stm32f103zet6 your_app
