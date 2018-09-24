## os-dev : 操作系统开发的实验 An Experiment on OS Develop
### 代码组织结构 Code Structure

- **boot/**  
    此目录包含了所有与启动相关的文件。  
    This folder includes files for bootstrap.  
    - **bits16/**  
        包含启动阶段的全部16位函数。  
        Includes all 16-bit functions in start-up phase.  
        - **printer.inc**
        - **printer.S**  
            以直接读写显存的形式实现的字符串打印函数。  
            Printing string by directly writes vram. 
        - **read_disk.inc**
        - **read_disk.S**  
            通过操作端口实现的读取硬盘的函数。  
            Reading disk contents by communicating with ports. 
    - **bits32/**  
        功能同上的32位函数。  
        Ditto, but in 32-bit mode. 
        - **printer.inc**
        - **printer.S**
        - **read_disk.inc**
        - **read_disk.S**
    - **boot.inc**  
        启动阶段需要的几乎所有常数。  
        Almost all constants needed in start-up phase. 
    - **ld-loader.ld**
    - **ld-mbr.ld**
    - **loader.S**
    - **mbr.S**
- **helper/**  
    此目录中提供了`sign_mbr`程序，用于为链接好的mbr添加签名`0xaa55`。  
    Provides the program `sign_mbr`, which adds signature `0xaa55` for linked MBR. 
    - **sign_mbr.cpp**
- **kernel/**  
    此目录包含内核的相关文件。  
    Includes files of the kernel. 
    - **common.hpp**
    - **main.cpp**
    - **print.cpp**
    - **print.hpp**
- **_tools/_**  
    这个目录需要自己创建，里面应当包含一个可用的用于调试的Bochs版本，随后需要将可执行文件`bochs`的位置写入`Makefile`中的`BOCHS_D`变量，如`tools/bochs-2.6.9/bochs`；如果不需要调试（`make debug`），可以忽略这个目录，这不会影响编译（`make all`或`make build`），但应至少保证系统环境变量中有一个可用的Bochs，否则将无法运行此系统（`make run`）。  

    This directory is not uploaded, you have to create it yourself. If you would like to use the debug facility provided by Makefile (i.e. `make debug`), you should put a Bochs (with debugging enabled when compiled) in this folder, and change the value of variable `BOCHS_D` to `tools/path/to/bochs`. You can safely ignore this, so long as you don't need the debugging facility, for it will NOT affect the compilation (`make all`或`make build`). But you do need a working Bochs in your `$PATH`, or you will not be able to run this program (`make run`). 

- **_objects/_**
- **_binaries/_**  
    这两个目录是由Makefile自动生成的（通过`make prepare`），所有的编译结果都会存在于这两个目录中，你可以在任何时候选择删除这两个目录，这不会影响代码的完整性。  
    但是请注意，由于`bximage`程序的设计，我无法将自动生成磁盘镜像的命令插入Makefile中，因此一旦删除了`binaries/hd60M.img`，你必须手动生成这个文件，方法是使用`bximage`，根据交互式的引导，创建一个**flat**模式的**60M**的**硬盘**镜像。 
 
    These two directories are automatically generated by `make prepare` (a dependency of `make all`), all compilation results will be in these two directories. You may remove these two along with all their contents, and this will not affect the completeness of this program.  
    But, due to the design of the `bximage` program, I was not able to insert into the Makefile the command for automatically generating disk image. So once you removed `binaries/hd60M.img`, you will have to generate this file by hand, using `bximage`, following the instructions in the program. You should create a **flat** mode, **60MB** **hard drive** image. 
- **.clang-format**  
    我使用的C++代码自动格式化设置。  
    My configuration for auto-formating C++ codes.
- **.gitignore**
- **_bochs.log_**  
    Bochs自动生成的日志文件。  
    Log file automatically generated by Bochs. 
- **bochsrc**  
    虚拟机配置。  
    Configuration for the Bochs virtual machine. 
- **Makefile**  
    可用的命令（伪目标）：  
    Available commands (pseudo-targets):  
    - **all**: prepare & build
    - **build**: img
    - **img**:  
        生成镜像文件。  
        Generates the image file.
    - **clean**:  
        清除最终目标文件。  
        Clean the `.bin` files.
    - **clean-all**:  
        清除全部目标文件。  
        Clean `.bin` and `.o` files.
    - **refresh**: clean & build
    - **refresh-all**: clean-all & build
    - **prepare**:  
        生成所需的全部目录。  
        Create directories.
    - **debug**:  
        使用`BOCHS_D`指定的版本运行Bochs调试。  
        Run bochs for debug, using `BOCHS_D`.
    - **run**:  
        使用`BOCHS`指定的版本运行Bochs。  
        Run bochs, using `BOCHS`.
- **README.md**  
    这个文件。  
    This file. 

### 其他说明 Other Descriptions

这个项目主要是供个人学习之用，而本人是中国人，因此代码中的大部分注释都是用最习惯的现代汉语书写。英语母语者或其他的一些人很可能会建议所有人在项目中使用英语，因为“英语是当今世界上使用最广泛的语言”，但这个建议是毫无道理的。因为母语永远是个人最熟悉的，理解最清楚而不费力的语言，因此在任何情况下，为了代码作者自己的理解方便，应当在注释中使用母语。因此除了本文件，此份代码中的注释大部分为汉语，如遇理解不便，可参考各种翻译。

This project is mainly for personal study, and the author is Chinese, so most of the comments in the code is written in Chinese, while this file is an exception. Some English speakers and some other people suggests that everyone should use English in their project, because 'English is the most commonly used language all over the world'. But this suggestion, in my opinion, is non-sense, for mother language is always the most familiar language to the author, so for the sake of his own understanding, the author should always use his mother language. Once you cannot understand properly, please use the translator. 