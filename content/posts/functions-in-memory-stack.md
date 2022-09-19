+++
ShowToc = false
date = 2022-09-18T18:30:00Z
description = "Explanation of how function behave in stack."
tags = ["memory", "functions", "stack", "dsa"]
title = "Functions in Memory (Stack)"
[cover]
alt = "blog cover"
image = "/blog/uploads/function_in_stack.webp"

+++
Checkout my twitter [thread ](https://twitter.com/CeleronCoder/status/1571901815616344065?s=20&t=LbPT98o7oYfORc-HDz63iA)explaining how functions work in memory (stack).

Things to keep in mind to understand functions behavior

* When a function is called, it gets in the stack, and stays there until its execution.
* The function completes executions, it gets off the stack and returns to the program flow.

Here's a program with a main function that call some other functions and that function call some other function. As they get called, they are loaded into the memory stack.

![](/blog/uploads/function_in_stack.webp)

When the function, completes execution the functions gets off the stack and the program returns to the previous program flow, aka to the previous function that called that function.![](/blog/uploads/stack_get_off_1.webp)![](/blog/uploads/stack_get_off_2.webp)

Now, there is nothing left in the stack, even the main function that is loaded by default, gets off the stack, the program finishes execution.![](/blog/uploads/empty_stack_1.webp)

**Takeaways:**

* When the function is called, it is loaded on the stack with its arguments.
* It completes execution, it gets off the stack, and the program returns to its previous program flow.

> If "A" function calls "B" function, "A" function will remain in the stack (and in scope also) until the "B" function returns. It means that not only the currently executing function remains in the function but also all the parent function that called it.