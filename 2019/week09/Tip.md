# iOS系统结构

> 了解手机目录结构，有助于快速分析并解决问题

## 类UNIX操作系统常见的目录结构

> iOS系统由OSX演化，而OSX由Unix演化，而FHS（Filesystem Hierachy Standard）是Unix的文件目录的一套标准。

* /：根目录，以斜杠表示，其他所有文件和目录都在根目录下展开。
* /bin：“binary”的简写，存放提供用户基础功能的二进制文件，如ls、ps等
* /boot：存放能使系统成功启动的所有文件。iOS中此目录为空。
* /dev：“device”的简写，存放BSD设备文件。每个文件代表系统一块设备或字符设备，一般来说，“块设备”以快为单位传输数据，如硬盘；而“字符设备”以字符为单位传输数据，如调制解调器。
* /sbin：“system binaries”的简写，存放提供系统级基础功能的二进制文件，如netstat、reboot等。
* /etc：“Et Cetera”的简写，存放系统脚本以及配置文件，如passwd、hosts等。在iOS中，/etc是一个符号链接，实际指向/private/etc。
* /lib：存放系统库文件、内核模块以及设备驱动等。iOS中此目录为空。
* /mnt：“mount”的简写，存放临时的文件系统挂载点。iOS中此目录为空。
* /private：存放两个目录，分别是/private/etc和/private/var。
* /tmp：临时目录。在iOS中/tmp是一个符号链接，实际指向/private/var/tmp。
* /usr：包含大多数用户工具和程序。/usr/bin包含哪些/bin和/sbin中未出现的基础功能，如nm、killall等；/usr/include包含所有标准的C头文件；/usr/lib存放库文件
* /var：“variable”的简写，存放一些经常更改的文件，比如日志、用户数据、临时文件等等。其中/var/mobile和/var/root分别存放mobile用户和root用户的文件，是重点关注的目录。

## iOS操作系统独有的目录

* /Applications：存放所有的系统APP和来自于Cydia的App，不包括StoreApp。
* /Developer：如果一台设备链接XCode后被指定为调试用机，XCode就会在iOS中生成这个目录，其中会含有一些调试需要的工具和数据
* /Library：存放一些提供系统支持的数据，其中/Library/MobileSubstrate下存放了所有基于CydiaSubstrate的插件
* /System/Library：iOS文件系统中最重要的目录之一，存放大量系统组件。
* /System/Library/Frameworks和/System/Library/PrivateFrameworks：存放iOS中各种framework，其中出现在SDK文档里面的只是冰山一角，还有数不清的功能等待我们开发。
* /System/Library/CoreServices里的SpringBoard.app：iOS桌面管理器(类似于Windows里的explorer)，是用户与系统交流的最重要的中介
* /User：用户目录，实际指向/var/mobile。
* /var/mobile/Media/DCIM下存放照片
* /var/mobile/Media/Recordings下存放录音文件
* /var/mobile/Library/SMS下存放短信数据库
* /var/mobile/Library/Mail下存放邮件数据
* /var/mobile/Containers存放StoreApp，其中App的可执行文件在bundle与App中的数据目录分别存放在/var/mobile/Containers/Bundle和/var/mobile/Containers/Data两个不同的目录下。

## iOS文件权限简介

iOS是一个多用户操作系统，用户是一个抽象概念，代表它能对操作系统的所有权和使用权。例如一个mobile用户无法调用reboot命令重启iOS，而root用户却可以。“组”是用户的一种组织方式，一个组可以包含多个用户，一个用户可以属于多个组。

iOS中使用3位(bit)来表示文件的权限，从高到低分别是r(read),w(write),x(execute)权限。文件与用户的关系存在三种可能：1.用户属于主用户；2.用户不是属于主用户，但是属于主组；3.用户既不是主用户也不属于主组。

所以我们需要3*3位来表示一个文件的权限，如果这一位代表为1则代表权限有效，否则无效。例如：111101101代表rwxr-xr-x，其中前三位是主用户权限，中间三位是属于主组而不是主用户的权限，后面三位是不属于主组也不是主用户的权限。表示该文件`主用户`拥有rwx权限，而`属于主组而不是主用户`只有rx权限，`不属于主组也不是主用户`的只有rx权限。转换为八进制755也是一种常见的权限表示方式。

## 二进制文件类型

bundle的概念来源于NeXTSTEP，它不是一个文件，而是一个按某种标准结构来组织的目录，其中包含了二进制文件及运行所需的资源。正像开发中常见的App和framework都是以bundle的形式存在的；在越狱iOS中常见的PreferenctBundle可以看成是一种依附在Setting的App，结构与App相似，本质是bundle。 

Framework也是bundle，但framework的bundle中存放的是一个dylib，而不是一个可执行文件。相对来说frmakework的地位比App还高，因为一个App的大多数功能都是通过调用framework提供的接口来实现的。将某个bundle确立为逆向目标之后，绝大多数逆向线索都可以在bundle内找到，这大大降低了逆向工程的复杂度。

* Application
	* App目录结构
	* 系统App VS StoreApp
* Dynamic Library
	* 在iOS中，lib分为static和dynamic两种，其中static lib在编译阶段成为App可执行文件的一部分，会增加可执行文件的大小。因为App尺寸变大，启动时需要加载的内容变多，所以可能会导致App启动变慢。dylib则相对智能一些，它不会改变可执行文件的大小，只有当App需要用到这个dylib的时候，iOS才把它加载进内存，成为App进程的一部分。
* Daemon
	* Daemon即守护进程，为后台而生，给用户提供了各种守护，如imagent保障了iMessage得正确收放，mediaserverd处理了几乎所有的音频、视频，syslogd则用于记录系统日志等。iOS中daemon主要由一个可执行文件和一个plist文件构成。iOS的根进程是launchd，他会在开机时检查/Ststem/Library/LaunchDaemons和/Library/LaunchDaemons下所有格式符合规定的plist文件，然后启动对应的daemon。
	* daemon提供的功能相对底层，随意改动会造成严重后果，例如白苹果。

## App目录结构

* Info.plist。记录了App得基本信息，如bundle identifier、可执行文件名、图标文件名等。我们也可以通过XCode自带的命令行工具plutil查看他的值。
* 可执行文件。它是App目录下最核心的部分，也是逆向工程的主要目标。同样可以通过XCode和plutil两种方式来查看Info.plist，定位可执行文件。
* lproj目录。各种本地化的字符串（.strings），他是逆向工程的重要线索，也可以通过plutil查看。

## 系统App VS StoreApp

> /Application/目录下存放系统App和从Cydia下载的App（我们把来自Cydia的App视为系统App），而/var/mobile/Containers/目录下存放的则是StoreApp。虽然两者都为App，但是他们在如下方面存在差异。

1. 目录结构。两种App的bundle内部目录结构区别不大，都含有Info.plist、可执行文件、lproj目录等。但是数据目录的位置不同；StoreApp的数据目录在/var/mobile/Containers/Data/下，以mobile权限运行的系统App的数据目录在/var/mobile/下，而以root权限下运行的数据目录在/var/root/下。
2. 安装包的格式和权限。Cydia App的安装包格式一般为deb，StoreApp的安装包格式一般为ipa。其中deb是来自Debian得安装包格式，由Cydia得作者saurik移植到iOS中，他得属主用户和属主组一般是root和admin，能够以root权限运行；而ipa则是苹果为iOS推出的专属App安装包格式，属主用户和属主组都是mobile，只能以mobile权限运行。
3. 沙盒。沙盒会将App的文件访问方位限制在这个App内部，一个App一般不知道其他App的存在，更别说访问他们了；沙盒还会限制App的功能，例如对iCloud接口的调用必须经过沙盒的允许。


