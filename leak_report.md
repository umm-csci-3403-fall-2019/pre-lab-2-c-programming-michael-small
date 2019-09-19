# Leak report

Running `valgrind` yielded 46 bytes in 6 blocks leaked.

`valgrind --leak-check=full ./check_whitespace` 

>==27823== Memcheck, a memory error detector
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

Placing `free(cleaned)` in line 70 frees everything up, but there is an error. 

>'USA' is clean.
==29778== Invalid free() / delete / delete[] / realloc()
==29778==    at 0x4839A0C: free (vg_replace_malloc.c:540)
==29778==    by 0x40129A: is_clean (check_whitespace.c:70)
==29778==    by 0x401308: main (check_whitespace.c:88)
==29778==  Address 0x402010 is in a r-- mapped file /home/small203/Practicum/pre-lab-2-c-programming-michael-small/check_whitespace segment
==29778== 
The string '   ' is NOT clean.
The string '     silliness    ' is NOT clean.

Looking at the array, that `invalid free` is on the empty string. 
However, changing `return ""` to `return strdup("")` in strip's `num_spaces >= size` clause
prevents this error. Now all valid strings are freed, and empty strings are to my understanding 
either not freed because they don't have to or are handled specially by some property
of strdup that I don't have the time to look deeper into in 3 mins before this hand in is
(though I will look into it right after submission.)