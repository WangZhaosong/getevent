# getevent
移植Android工具getevent到Linux，相应的文件位于system/core/toolbox中。

修改步骤如下：

1. getevent.c引用的头文件'input.h-labels.h'是用脚本'generate-input.h-labels.py'自动生成的，参数是'bionic/libc/kernel/uapi/linux/input-event-codes.h'（参考Android.mk)，在命令行下输入下列命令

```
$ ./generate-input.h-labels.py /usr/include/linux/input-event-codes.h > input.h-labels.h
```

2. 注释掉引用的'sys/limits.h>'

```
//#include <sys/limits.h>
```

3.添加头文件'time.h'，解决`CLOCK_MONOTONIC`定义问题，并修改`getevent_main`为`main`

```
#include <time.h>
```

4. 生成可执行文件

```
$ gcc -o getevent getevent.c
```

5. sendevent可以直接将`sendevent_main`修改为`main`，不依赖其他文件，可以直接编译通过，此处不多做介绍。
