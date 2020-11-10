# 【Python】如何结束退出 py 脚本


<!--more-->

# 需求

当你运行脚本，在判断条件满足时，就退出脚本，结束本次执行。

# 方法

1. 使用 `sys.exit()`，直接退出程序，但是会引发一个 SystemExit 异常；

   - 该方法包含一个 status 参数

     - sys.exit(0)，0 表示支持退出
     - sys.exit(1)，1 表示异常退出

   - 这个SystemExit异常，是唯一一个不会被认为是错误的异常，当没有设置异常捕获的话，是不会有报错的，可以进行正常退出，当然也可以捕获这个异常进行一些其他操作。

     ```py
     import sys
     #sys.exit()
     #sys.exit(0)
     #sys.exit(1)
     try:
         sys.exit(1)
     except SystemExit as err:
         print(err) # 1
     ```

     

2. 使用 `os._exit()`，直接退出，不会抛出异常；

   - 但我们常用的Win32平台和基于UNIX的平台不会有所影响

3. 使用 `os.kill()`，用于直接 Kill 掉进程，只在 unix 平台上有效；


