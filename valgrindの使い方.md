### valgrindの使い方

```bash
valgrind --tool=memcheck --leak-check=full --show-reachable=yes --track-origins=yes ./<ターゲットプログラム名>
```

```c
#include <stdio.h>

void function1()
{
  int x; // 初期化されていない変数

  printf("#----------------------------------\n");
  printf("# %s\n", __FUNCTION__);
  printf("#----------------------------------\n");

  if (x > 10)
  {
    printf("x is greater than 10\n");
  }
}

void function2()
{
  char *ptr; // 初期化されていないポインタ

  printf("#----------------------------------\n");
  printf("# %s\n", __FUNCTION__);
  printf("#----------------------------------\n");

  printf("INFO: 初期化されていないポインタを比較する\n");
  if (ptr == NULL)
  {
    printf("Pointer is NULL\n");
  }

  printf("INFO: 初期化されていないポインタを間接参照する\n");
  if (*ptr == 'a')
  {
    printf("Value is 'a'\n");
  }

  printf("INFO: 初期化していないポインタを文字列として使用する。\n");
  printf("String: %s\n", ptr);

  return;
}



int main()
{
  function1();
  function2();

  return 0;
}
```

これをコンパイルして以下の様に実行すると、未定義値の使用を検出してくれる。

```bash
[<ユーザー名>@<サーバ―名>:~]$ valgrind --tool=memcheck --leak-check=full --show-reachable=yes --track-origins=yes ./test2
==45760== Memcheck, a memory error detector
==45760== Copyright (C) 2002-2012, and GNU GPL'd, by Julian Seward et al.
==45760== Using Valgrind-3.8.1 and LibVEX; rerun with -h for copyright info
==45760== Command: ./test2
==45760== 
#----------------------------------
# function1
#----------------------------------
==45760== Conditional jump or move depends on uninitialised value(s)
==45760==    at 0x40053B: function1 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005E9: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400504: function1 (in /home/<ユーザー名>/test2)
==45760== 
#----------------------------------
# function2
#----------------------------------
INFO: 初期化されていないポインタを比較する
==45760== Conditional jump or move depends on uninitialised value(s)
==45760==    at 0x40058B: function2 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005F3: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400549: function2 (in /home/<ユーザー名>/test2)
==45760== 
INFO: 初期化されていないポインタを間接参照する
==45760== Use of uninitialised value of size 8
==45760==    at 0x4005A5: function2 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005F3: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400549: function2 (in /home/<ユーザー名>/test2)
==45760== 
INFO: 初期化していないポインタを文字列として使用する。
==45760== Conditional jump or move depends on uninitialised value(s)
==45760==    at 0x4E750E0: vfprintf (in /lib64/libc-2.12.so)
==45760==    by 0x4E7F069: printf (in /lib64/libc-2.12.so)
==45760==    by 0x4005D8: function2 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005F3: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400549: function2 (in /home/<ユーザー名>/test2)
==45760== 
==45760== Use of uninitialised value of size 8
==45760==    at 0x4E77D0C: vfprintf (in /lib64/libc-2.12.so)
==45760==    by 0x4E7F069: printf (in /lib64/libc-2.12.so)
==45760==    by 0x4005D8: function2 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005F3: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400549: function2 (in /home/<ユーザー名>/test2)
==45760== 
==45760== Conditional jump or move depends on uninitialised value(s)
==45760==    at 0x4EA1788: _IO_file_xsputn@@GLIBC_2.2.5 (in /lib64/libc-2.12.so)
==45760==    by 0x4E7806F: vfprintf (in /lib64/libc-2.12.so)
==45760==    by 0x4E7F069: printf (in /lib64/libc-2.12.so)
==45760==    by 0x4005D8: function2 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005F3: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400549: function2 (in /home/<ユーザー名>/test2)
==45760== 
==45760== Use of uninitialised value of size 8
==45760==    at 0x4EA178E: _IO_file_xsputn@@GLIBC_2.2.5 (in /lib64/libc-2.12.so)
==45760==    by 0x4E7806F: vfprintf (in /lib64/libc-2.12.so)
==45760==    by 0x4E7F069: printf (in /lib64/libc-2.12.so)
==45760==    by 0x4005D8: function2 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005F3: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400549: function2 (in /home/<ユーザー名>/test2)
==45760== 
==45760== Conditional jump or move depends on uninitialised value(s)
==45760==    at 0x4EA17A3: _IO_file_xsputn@@GLIBC_2.2.5 (in /lib64/libc-2.12.so)
==45760==    by 0x4E7806F: vfprintf (in /lib64/libc-2.12.so)
==45760==    by 0x4E7F069: printf (in /lib64/libc-2.12.so)
==45760==    by 0x4005D8: function2 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005F3: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400549: function2 (in /home/<ユーザー名>/test2)
==45760== 
==45760== Use of uninitialised value of size 8
==45760==    at 0x4EA17AD: _IO_file_xsputn@@GLIBC_2.2.5 (in /lib64/libc-2.12.so)
==45760==    by 0x4E7806F: vfprintf (in /lib64/libc-2.12.so)
==45760==    by 0x4E7F069: printf (in /lib64/libc-2.12.so)
==45760==    by 0x4005D8: function2 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005F3: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400549: function2 (in /home/<ユーザー名>/test2)
==45760== 
==45760== Use of uninitialised value of size 8
==45760==    at 0x4EA1830: _IO_file_xsputn@@GLIBC_2.2.5 (in /lib64/libc-2.12.so)
==45760==    by 0x4E7806F: vfprintf (in /lib64/libc-2.12.so)
==45760==    by 0x4E7F069: printf (in /lib64/libc-2.12.so)
==45760==    by 0x4005D8: function2 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005F3: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400549: function2 (in /home/<ユーザー名>/test2)
==45760== 
==45760== Conditional jump or move depends on uninitialised value(s)
==45760==    at 0x4EA1840: _IO_file_xsputn@@GLIBC_2.2.5 (in /lib64/libc-2.12.so)
==45760==    by 0x4E7806F: vfprintf (in /lib64/libc-2.12.so)
==45760==    by 0x4E7F069: printf (in /lib64/libc-2.12.so)
==45760==    by 0x4005D8: function2 (in /home/<ユーザー名>/test2)
==45760==    by 0x4005F3: main (in /home/<ユーザー名>/test2)
==45760==  Uninitialised value was created by a stack allocation
==45760==    at 0x400549: function2 (in /home/<ユーザー名>/test2)
==45760== 
String: 1I^HHPTI
==45760== 
==45760== HEAP SUMMARY:
==45760==     in use at exit: 0 bytes in 0 blocks
==45760==   total heap usage: 0 allocs, 0 frees, 0 bytes allocated
==45760== 
==45760== All heap blocks were freed -- no leaks are possible
==45760== 
==45760== For counts of detected and suppressed errors, rerun with: -v
==45760== ERROR SUMMARY: 53 errors from 11 contexts (suppressed: 8 from 6)

```

このような事が無いことを確認したので、<ターゲットプログラム名>.cに別の未定義値は存在しないと考えられる。

```bash
[<ユーザー名>@<サーバ―名>:~]$ valgrind --tool=memcheck --leak-check=full --show-reachable=yes --track-origins=yes ./test2
==45768== Memcheck, a memory error detector
==45768== Copyright (C) 2002-2012, and GNU GPL'd, by Julian Seward et al.
==45768== Using Valgrind-3.8.1 and LibVEX; rerun with -h for copyright info
==45768== Command: ./test2
==45768== 
#----------------------------------
# function1
#----------------------------------
==45768== Conditional jump or move depends on uninitialised value(s)
==45768==    at 0x40053B: function1 (in /home/<ユーザー名>/test2)
==45768==    by 0x4005F1: main (in /home/<ユーザー名>/test2)
==45768==  Uninitialised value was created by a stack allocation
==45768==    at 0x400504: function1 (in /home/<ユーザー名>/test2)
==45768== 
#----------------------------------
# function2
#----------------------------------
INFO: 初期化されていないポインタを比較する
Pointer is NULL
INFO: 初期化されていないポインタを間接参照する
==45768== Invalid read of size 1
==45768==    at 0x4005AD: function2 (in /home/<ユーザー名>/test2)
==45768==    by 0x4005FB: main (in /home/<ユーザー名>/test2)
==45768==  Address 0x0 is not stack'd, malloc'd or (recently) free'd
==45768== 
==45768== 
==45768== Process terminating with default action of signal 11 (SIGSEGV): dumping core
==45768==  Access not within mapped region at address 0x0
==45768==    at 0x4005AD: function2 (in /home/<ユーザー名>/test2)
==45768==    by 0x4005FB: main (in /home/<ユーザー名>/test2)
==45768==  If you believe this happened as a result of a stack
==45768==  overflow in your program's main thread (unlikely but
==45768==  possible), you can try to increase the size of the
==45768==  main thread stack using the --main-stacksize= flag.
==45768==  The main thread stack size used in this run was 8388608.
==45768== 
==45768== HEAP SUMMARY:
==45768==     in use at exit: 0 bytes in 0 blocks
==45768==   total heap usage: 0 allocs, 0 frees, 0 bytes allocated
==45768== 
==45768== All heap blocks were freed -- no leaks are possible
==45768== 
==45768== For counts of detected and suppressed errors, rerun with: -v
==45768== ERROR SUMMARY: 2 errors from 2 contexts (suppressed: 8 from 6)
Segmentation fault (core dumped)

```

修正版で問題ないことを確認

```bash
==45801== 
==45801== HEAP SUMMARY:
==45801==     in use at exit: 0 bytes in 0 blocks
==45801==   total heap usage: 7 allocs, 7 frees, 3,976 bytes allocated
==45801== 
==45801== All heap blocks were freed -- no leaks are possible
==45801== 
==45801== For counts of detected and suppressed errors, rerun with: -v
==45801== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 8 from 6)
```

