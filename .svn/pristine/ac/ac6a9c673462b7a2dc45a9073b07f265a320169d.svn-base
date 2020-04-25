---
title: 深入STM32开发
abbrlink: ba752eb9
tags:
---

makefile文件

为了简单上手，我所知的有两种方法获得makefile文件。

    使用STM32CubeMX，生成代码，选择生成Toolchain/IDE为Makefile
    这种方法优缺点，主要体现在，它的源文件和Inc目录需要自己手动维护，在代码较多时比较麻烦，而且只支持HAL/LL库，如果自己用STD库，需要额外维护。
    但是用法上配合STM32CubeMX，可以在生成代码时直接更新makefile，相对来说并不麻烦。尤其是可以通过STM32CubeMX精简代码，做到用什么，代码中就只保留什么源文件。
    为了自己方便，尤其是懒得每增加一个源文件都去改makefile，我参照网上的资料，修改了如下makefile脚本。使用起来相对舒服一些，其中的参数可以按照自己的喜好调整。目录结构也可以调整为适合自己的。

#工程的名称及最后生成文件的名字
TARGET = project_name
# Build path
BUILD_DIR = build


#######################################
# 编译器参数设置
#######################################
# 选择自己的编译器版本
# GCC_PATH=
PREFIX = arm-none-eabi-
# The gcc compiler bin path can be either defined in make command via GCC_PATH variable (> make GCC_PATH=xxx)
# either it can be added to the PATH environment variable.
ifdef GCC_PATH
CC = $(GCC_PATH)/$(PREFIX)gcc
AS = $(GCC_PATH)/$(PREFIX)gcc -x assembler-with-cpp
CP = $(GCC_PATH)/$(PREFIX)objcopy
SZ = $(GCC_PATH)/$(PREFIX)size
else
CC = $(PREFIX)gcc
AS = $(PREFIX)gcc -x assembler-with-cpp
CP = $(PREFIX)objcopy
SZ = $(PREFIX)size
endif
HEX = $(CP) -O ihex
BIN = $(CP) -O binary -S


#读取当前工作目录
TOP=$(shell pwd)

#设定包含文件目录
INC_FLAGS= -I $(TOP)/Core/CMSIS/Include                 \
					 -I $(TOP)/Core/CMSIS/Device/ST/STM32F10x/Include	\
           -I $(TOP)/Hardware    \
           -I $(TOP)/Core/STM32F10x_StdPeriph_Driver/inc             \
           -I $(TOP)/Driver        \
           -I $(TOP)/User

#######################################
# ASM sources
# 注意，如果使用.c的startup文件，请把下面两句注释掉，避免编译出现错误
#######################################
ASM_SOURCES =  \
startup_stm32f103xe.s

#######################################
# CFLAGS
#######################################
# debug build?
DEBUG = 1

# 代码优化级别
OPT = -Og

# cpu
CPU = -mcpu=cortex-m3

# fpu
# NONE for Cortex-M0/M0+/M3

# float-abi

# mcu
MCU = $(CPU) -mthumb $(FPU) $(FLOAT-ABI)

# C defines
# 这个地方的定义根据实际情况来，如果在代码里面有定义(如#define STM32F10X_HD)，这里可以不写
C_DEFS =  \
-D USE_STDPERIPH_DRIVER \
-D STM32F10X_HD

# PreProcess
CFLAGS = $(MCU) $(C_DEFS) $(INC_FLAGS) $(OPT) -std=gnu11 -W -Wall -fdata-sections -ffunction-sections

ifeq ($(DEBUG), 1)
CFLAGS += -g -gdwarf-2
endif
# 在$(BUILD_DIR)目录下生成依赖关系信息，依赖关系以.d结尾
CFLAGS += -MMD -MP -MF"$(addprefix $(BUILD_DIR)/, $(notdir $(@:%.o=%.d)))"

#######################################
# LDFLAGS
#######################################
# LD文件  
LDSCRIPT = STM32F103ZETx_FLASH.ld

# libraries
LIBS = -lc -lm -lnosys 
LIBDIR = 
LDFLAGS = $(MCU) -specs=nano.specs -T$(LDSCRIPT) $(LIBDIR) $(LIBS) -Wl,-Map=$(BUILD_DIR)/$(TARGET).map,--cref -Wl,--gc-sections


#######################################
# C/CPP源代码
#######################################
C_SRC = $(shell find ./ -name '*.c')
C_SRC += $(shell find ./ -name '*.cpp')

C_OBJ = $(addprefix $(BUILD_DIR)/,$(notdir $(C_SRC:%.c=%.o)))
vpath %.c $(sort $(dir $(C_SRC)))
C_OBJ += $(addprefix $(BUILD_DIR)/,$(notdir $(ASM_SOURCES:.s=.o)))
vpath %.s $(sort $(dir $(ASM_SOURCES)))


.PHONY: all clean update


all: $(BUILD_DIR)/$(TARGET).elf $(BUILD_DIR)/$(TARGET).hex $(BUILD_DIR)/$(TARGET).bin

# 从vpath中读取所有的.c文件，编译成.o文件
$(BUILD_DIR)/%.o: %.c | $(BUILD_DIR)
	$(CC) -c $(CFLAGS) -Wa,-a,-ad,-alms=$(BUILD_DIR)/$(notdir $(<:.c=.lst)) -o $@ $<

# 从vpath中u读取所有的.s文件，编译成.o文件
$(BUILD_DIR)/%.o: %.s | $(BUILD_DIR)
	$(AS) -c $(CFLAGS) $< -o $@

# 将所有的.o文件依据.ld文件，编译组成.elf文件
$(BUILD_DIR)/$(TARGET).elf: $(C_OBJ)
	$(CC) $(C_OBJ) $(LDFLAGS) -o $@
	$(SZ) $@

# 将.elf文件转为.hex格式
$(BUILD_DIR)/%.hex: $(BUILD_DIR)/%.elf
	$(HEX) $< $@
	
# 将.elf文件转为.bin格式
$(BUILD_DIR)/%.bin: $(BUILD_DIR)/%.elf
	$(BIN) $< $@	

# 用于生成BUILD_DIR目录
$(BUILD_DIR):
	mkdir $@		

clean:
	rm -f $(shell find ./ -name '*.o')
	rm -f $(shell find ./ -name '*.d')
	rm -f $(shell find ./ -name '*.lst')
	rm -f $(shell find ./ -name '*.map')
	rm -f $(shell find ./ -name '*.elf')
	rm -f $(shell find ./ -name '*.bin')
	rm -f $(shell find ./ -name '*.hex')

update:
	openocd -f interface/jlink.cfg  -f target/stm32f1x.cfg -c init -c halt -c "flash write_image erase $(TOP)/$(TARGET).hex" -c reset -c shutdown
 

ld文件

ld文件也就是编译链，这个在GCC交叉编译中比较常见。如下给出一个例子，需要修改的地方不多，主要是修改Memory这一块，要根据自己的芯片来。对于STM32芯片，只要少量修改都能做到通用

/*
*****************************************************************************
**

**  File        : LinkerScript.ld
**
**  Abstract    : Linker script for STM32F103ZETx Device with
**                512KByte FLASH, 64KByte RAM
**
**                Set heap size, stack size and stack location according
**                to application requirements.
**
**                Set memory bank area and size if external memory is used.
**
**  Target      : STMicroelectronics STM32
**
**
**  Distribution: The file is distributed as is, without any warranty
**                of any kind.
**
**  (c)Copyright Ac6.
**  You may use this file as-is or modify it according to the needs of your
**  project. Distribution of this file (unmodified or modified) is not
**  permitted. Ac6 permit registered System Workbench for MCU users the
**  rights to distribute the assembled, compiled & linked contents of this
**  file as part of an application binary file, provided that it is built
**  using the System Workbench for MCU toolchain.
**
*****************************************************************************
*/

/* Entry Point */
ENTRY(Reset_Handler)

/* Highest address of the user mode stack */


/*
* 这个地方需要自己修改，_estack也就是RAM的最高地址，是RAM.ORIGIN+RAM.ORIGIN.LENGTH相加的结果
* 在这里表示为  0x20000000+(Dec2Hex)64*1024=0x20010000
*/
_estack = 0x20010000;    /* end of RAM */
/* Generate a link error if heap and stack don't fit into RAM */
_Min_Heap_Size = 0x200;      /* required amount of heap  */
_Min_Stack_Size = 0x400; /* required amount of stack */

/* Specify the memory areas */

/*
* Memory的大小和具体的芯片相关，可以看STM的选型手册
*/
MEMORY
{
RAM (xrw)      : ORIGIN = 0x20000000, LENGTH = 64K
FLASH (rx)      : ORIGIN = 0x8000000, LENGTH = 512K
}

/* Define output sections */
/*
* 这里是ld脚本的SECTIONS命令，指明了各个.o文件在单片机中如何存放，如果不了解，不要修改
*/
SECTIONS
{
  /* The startup code goes first into FLASH */
  .isr_vector :
  {
    . = ALIGN(4);
    KEEP(*(.isr_vector)) /* Startup code */
    . = ALIGN(4);
  } >FLASH

  /* The program code and other data goes into FLASH */
  .text :
  {
    . = ALIGN(4);
    *(.text)           /* .text sections (code) */
    *(.text*)          /* .text* sections (code) */
    *(.glue_7)         /* glue arm to thumb code */
    *(.glue_7t)        /* glue thumb to arm code */
    *(.eh_frame)

    KEEP (*(.init))
    KEEP (*(.fini))

    . = ALIGN(4);
    _etext = .;        /* define a global symbols at end of code */
  } >FLASH

  /* Constant data goes into FLASH */
  .rodata :
  {
    . = ALIGN(4);
    *(.rodata)         /* .rodata sections (constants, strings, etc.) */
    *(.rodata*)        /* .rodata* sections (constants, strings, etc.) */
    . = ALIGN(4);
  } >FLASH

  .ARM.extab   : { *(.ARM.extab* .gnu.linkonce.armextab.*) } >FLASH
  .ARM : {
    __exidx_start = .;
    *(.ARM.exidx*)
    __exidx_end = .;
  } >FLASH

  .preinit_array     :
  {
    PROVIDE_HIDDEN (__preinit_array_start = .);
    KEEP (*(.preinit_array*))
    PROVIDE_HIDDEN (__preinit_array_end = .);
  } >FLASH
  .init_array :
  {
    PROVIDE_HIDDEN (__init_array_start = .);
    KEEP (*(SORT(.init_array.*)))
    KEEP (*(.init_array*))
    PROVIDE_HIDDEN (__init_array_end = .);
  } >FLASH
  .fini_array :
  {
    PROVIDE_HIDDEN (__fini_array_start = .);
    KEEP (*(SORT(.fini_array.*)))
    KEEP (*(.fini_array*))
    PROVIDE_HIDDEN (__fini_array_end = .);
  } >FLASH

  /* used by the startup to initialize data */
  _sidata = LOADADDR(.data);

  /* Initialized data sections goes into RAM, load LMA copy after code */
  .data : 
  {
    . = ALIGN(4);
    _sdata = .;        /* create a global symbol at data start */
    *(.data)           /* .data sections */
    *(.data*)          /* .data* sections */

    . = ALIGN(4);
    _edata = .;        /* define a global symbol at data end */
  } >RAM AT> FLASH

  
  /* Uninitialized data section */
  . = ALIGN(4);
  .bss :
  {
    /* This is used by the startup in order to initialize the .bss secion */
    _sbss = .;         /* define a global symbol at bss start */
    __bss_start__ = _sbss;
    *(.bss)
    *(.bss*)
    *(COMMON)

    . = ALIGN(4);
    _ebss = .;         /* define a global symbol at bss end */
    __bss_end__ = _ebss;
  } >RAM

  /* User_heap_stack section, used to check that there is enough RAM left */
  ._user_heap_stack :
  {
    . = ALIGN(8);
    PROVIDE ( end = . );
    PROVIDE ( _end = . );
    . = . + _Min_Heap_Size;
    . = . + _Min_Stack_Size;
    . = ALIGN(8);
  } >RAM

  

  /* Remove information from the standard libraries */
  /DISCARD/ :
  {
    libc.a ( * )
    libm.a ( * )
    libgcc.a ( * )
  }

  .ARM.attributes 0 : { *(.ARM.attributes) }
}


startup文件

startup文件是一段用汇编写的文件，极少去修改它。目前也有用C语言写的版本，大同小异。一般不用管就行。只要在编译的时候.s编译成了.o文件，剩下的事情交给编译器按照ld文件的描述来安排就行了。（详情请见ld文件中的“.isr_vector"和startup中的"isr_vector"
程序运行过程中，按照ld的设计，先运行startup的代码进行初始化，然后在starup中跳转到main函数。如果希望在main函数前进行一些别的初始化操作，那就修改startup文件吧

这里还需要注意的是，不同的编译器startup文件会有所不同，一定要找对应编译器所支持的startup文件。startup文件和ld文件配合使用。这个在官网有提供
openocd部分

如果遇到无法调试的问题，可以参考我的文章
openocd的使用问题汇总
如果仍然有问题无法调试，可以给我留言
vscode部分

使用vscode开发，需要关注几个点

    插件
    我推荐的插件有
    C/C++
    进行C代码的支持
    C++ Intellisense
    可以用这个插件来进行代码提示，效果还不错。此外如果需要查看函数的被引用情况，这个插件可以提供，算是C/C++的一个互补
    Cortex-Debug
    这个插件可以满足在vscode上调用openocd进行芯片的调试。（总算可以告别arm-none-eabi-gdb的TUI界面了）。变量监视，断点都有，也可以检测Cortex-M3的寄存器，如果用gdb调试比较熟悉，可以不考虑这个。
    需要一些额外配置，具体的可以去插件给的链接里面找。我这里给出一个例子

{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Cortex Debug",
            "cwd": "${workspaceRoot}",
            "executable": "./build/ZET6-base-develop.elf", //注意修改自己的elf文件
            "request": "launch",
            "type": "cortex-debug",
            "servertype": "openocd",
            "device": "STM32F103ZE", //这个不是很重要，写不写应该无所谓
            "configFiles": [
                "interface/jlink.cfg",
                "target/stm32f1x.cfg"
            ]
        }
    ]
}


PlatformIO IDE
这个看个人喜好吧，目前我不是很习惯
2. C/C++插件的配置文件
这里面主要关注的是includePath，compilePath

includePath，是为了让C++ Intellisense代码提示工作正常，避免编译的时候知道位置，但是vscode不知道库函数在哪。

compilePath，指定使用的编译器，仍然是为了让C++ Intellisense运行正常，有时候一些类型定义在gcc和arm-none-eabi-gcc中有差异，如果还用gcc，那么vscode满屛红字达成
补充1 GCC下printf函数重定向问题

重定向函数不再是fputc，在GCC下，重写函数_write

int _write(int fd, char *pBuffer, int size)
{
  for (int i = 0; i < size; i++)
  {
    while (!(USART1->SR & USART_SR_TXE))
    {
    }
    USART_SendData(USART1, pBuffer[i]);
  }
  return size;
}

前言

使用openocd，可以适配大批的调试器，真正做到一个软件驱动所有。但是现阶段的使用，如果没有仔细阅读官方的使用说明，或者对自己用的芯片不熟悉，会产生大量的问题。
最好的办法是先阅读一遍openocd官方的文档，有了一定的基础以后，再结合自己所用的芯片进行调整。

一条原则：出了问题，不要完全归结于官方给出的Script有问题。主动去检查错误发生的根本原因
Permission Deny

基本上只出现在linux上，windows是不会有这个的
这个很明显是权限问题。也就是openocd没有权限看到、操作调试器。
解决方法（两种）：

    使用root来执行openocd -f interface/xxx.cfg -f target/xxx.cfg
    打开目录openocd/contrib，linux:/usr/share/openocd/contrib，将dd-openocd.rules拷贝到/etc/udev/rules.d/下。解释一下，该*.rules文件是一系列设备描述和权限配置的文件。官方已经给配好调试器的权限。只需要把这个文件放进udev的rules.d目录下，令其生效即可。有些时候，需要把调试器重新插上去，按照新规则进行挂载。

Error: JTAG scan chain interrogation failed: all ones

错误信息如下

[XXX@XXX ~]$ openocd -f interface/jlink.cfg -f target/stm32f1x.cfg
Open On-Chip Debugger 0.10.0
Licensed under GNU GPL v2
For bug reports, read
        http://openocd.org/doc/doxygen/bugs.html
Info : auto-selecting first available session transport "jtag". To override use 'transport select <transport>'.
adapter speed: 1000 kHz
adapter_nsrst_delay: 100
jtag_ntrst_delay: 100
none separate
cortex_m reset_config sysresetreq
Info : No device selected, using first device.
Info : J-Link ARM V8 compiled Nov 28 2014 13:44:46
Info : Hardware version: 8.00
Info : VTarget = 3.313 V
Info : clock speed 1000 kHz
Error: JTAG scan chain interrogation failed: all ones
Error: Check JTAG interface, timings, target power, etc.
Error: Trying to use configured scan chain anyway...
Error: stm32f1x.cpu: IR capture error; saw 0x0f not 0x01
Warn : Bypassing JTAG setup events due to errors
Error: Invalid ACK (7) in DAP response
Error: JTAG-DP STICKY ERROR
Error: Invalid ACK (7) in DAP response
Error: JTAG-DP STICKY ERROR

原因可能是多方面的，但是注意看提示。Error: JTAG scan chain interrogation failed: all ones。这个意思是，当前使用jtag进行扫描，没有发现任何一个符合的目标。

解决方法：

    连接的设备平台是不支持jtag的，请检查自己设备的设置，是否使用的jtag的transport协议。
    解决方法很简单。openocd命令在没有指定的情况下默认会选择jtag协议。如果自己的设备使用了swd协议。需要单独指定。cp一份openocd的interface/XX.cfg文件，比如我使用的是j-link，我cp一份jlink.cfg并命名为jlnk-swd.cfg，文本内容如下

        interface jlink
        transport select swd

注：openocd支持的transport可以使用命令查看
openocd -c 'transport list'

    检查自己引出的调试线是否有故障。链接牢固与否
    调试器自身的故障 （补充：2018/11/23 ）
    可能这就是脸黑吧。jlink进行jtag调试没问题。我遇到的故障原因在于jlink固件掉了。刷固件修复以后，使用jtag也是没有问题的，这个也可以作为故障查找的方向吧

关于PlatformIO IDE使用openocd调试的一些补充内容

    platformio使用openocd进行调试需要先在platfromio.ini中进行相关的设置

    ; PlatformIO Project Configuration File
    ;
    ; Build options: build flags, source filter
    ; Upload options: custom upload port, speed and extra flags
    ; Library options: dependencies, extra library storages
    ; Advanced options: extra scripting
    ;
    ; Please visit documentation for the other options and examples
    ; https://docs.platformio.org/page/projectconf.html
    [env:genericSTM32F103C8]
    platform = ststm32
    board = genericSTM32F103C8
    framework = stm32cube
    upload_protocol = jlink
    debug_tool = jlink
    debug_port = localhost:3333

参考资料：https://docs.platformio.org/en/latest/projectconf.html

    platformio使用的openocd，在linux上是自动下载的gnuarmeclipse-openocd。存放在
    /home/XX/.platformio/packages/tool-openocd/
    platformio使用openocd进行对应芯片调试的时候，会自行设置使用的transport

启用Trace Debug实现处理器全速运行下的调试

在调试程序时，有些bug是无法简单地使用调试器强行打断CPU的运行找出。
全速运行时出现bug，使用调试软件对其进行交互式调试时，bug又不见了，如此反复。甚是折腾。

跟踪调试机制正是为了解决这个问题。

在ARM中典型的是ITM，如下给出openocd连接STM32，使用ITM进行调试的示例
