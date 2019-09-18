# Leak report

Running `valgrind` yielded 46 bytes in 6 blocks leaked.

`valgrind --leak-check=full ./check_whitespace` 

>==27823== Memcheck, a memory error detector
==27823== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==27823== Using Valgrind-3.15.0 and LibVEX; rerun with -h for copyright info
==27823== Command: ./check_whitespace
==27823== 
The string 'Morris' is clean.
The string '  stuff' is NOT clean.
The string 'Minnesota' is clean.
The string 'nonsense  ' is NOT clean.
The string 'USA' is clean.
The string '   ' is NOT clean.
The string '     silliness    ' is NOT clean.
==27823== 
==27823== HEAP SUMMARY:
==27823==     in use at exit: 46 bytes in 6 blocks
==27823==   total heap usage: 7 allocs, 1 frees, 1,070 bytes allocated
==27823== 
==27823== 46 bytes in 6 blocks are definitely lost in loss record 1 of 1
==27823==    at 0x483AB1A: calloc (vg_replace_malloc.c:762)
==27823==    by 0x4011F8: strip (check_whitespace.c:41)
==27823==    by 0x401264: is_clean (check_whitespace.c:62)
==27823==    by 0x4012EC: main (check_whitespace.c:87)
==27823== 
==27823== LEAK SUMMARY:
==27823==    definitely lost: 46 bytes in 6 blocks
==27823==    indirectly lost: 0 bytes in 0 blocks
==27823==      possibly lost: 0 bytes in 0 blocks
==27823==    still reachable: 0 bytes in 0 blocks
==27823==         suppressed: 0 bytes in 0 blocks
==27823== 
==27823== For lists of detected and suppressed errors, rerun with: -s
==27823== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)



