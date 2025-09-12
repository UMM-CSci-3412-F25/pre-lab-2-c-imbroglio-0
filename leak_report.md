# Leak report

There was a memory leak in check_whitespace.c that had to be fixed using free((void *)cleaned).
Originally, I thought that free(cleaned) would be enough, but I ended up getting an error for 
converting between void and constant void variables. So, I needed to cast cleaned using (void *).
Additionally, I had to use an if statement to make sure that empty strings weren't being freed,
as I learned that that is something that you cannot do. So the if statement made sure that that
would not happen. And with that, the memory leak in check_whitespace.c was fixed.

The memory leak in check_whitespace_test.cpp gave me a bit more trouble, as I was unsure as to
whether I should edit the test file or not. But there was definitely a memory leak there. After
a bit of research, I found out that if I assigned the result variable to the desired result of
the test, I would be able to free that variable after the test passed. Because is_clean was
already patched up, I only added the free() lines to the strip tests. Finally, I realized that
the first test looks for an empty string, and so it would be pointless to free the empty string
and would also make things not work. Then, the memory leak in check_whitespace_test.cpp was fixed.

