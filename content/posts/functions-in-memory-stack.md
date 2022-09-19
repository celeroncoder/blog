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
Checkout my twitter thread explaining how functions work in memory (stack).

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Here's how functions behave in the stack (aka memory)<br><br>A thread 🧵<a href="[https://twitter.com/hashtag/programming?src=hash&ref_src=twsrc%5Etfw](https://twitter.com/hashtag/programming?src=hash&ref_src=twsrc%5Etfw "https://twitter.com/hashtag/programming?src=hash&ref_src=twsrc%5Etfw")">#programming</a> <a href="[https://twitter.com/hashtag/DSA?src=hash&ref_src=twsrc%5Etfw](https://twitter.com/hashtag/DSA?src=hash&ref_src=twsrc%5Etfw "https://twitter.com/hashtag/DSA?src=hash&ref_src=twsrc%5Etfw")">#DSA</a> <a href="[https://twitter.com/hashtag/ALGO?src=hash&ref_src=twsrc%5Etfw](https://twitter.com/hashtag/ALGO?src=hash&ref_src=twsrc%5Etfw "https://twitter.com/hashtag/ALGO?src=hash&ref_src=twsrc%5Etfw")">#ALGO</a> <a href="[https://twitter.com/hashtag/recursions?src=hash&ref_src=twsrc%5Etfw](https://twitter.com/hashtag/recursions?src=hash&ref_src=twsrc%5Etfw "https://twitter.com/hashtag/recursions?src=hash&ref_src=twsrc%5Etfw")">#recursions</a> <a href="[https://twitter.com/hashtag/stack?src=hash&ref_src=twsrc%5Etfw](https://twitter.com/hashtag/stack?src=hash&ref_src=twsrc%5Etfw "https://twitter.com/hashtag/stack?src=hash&ref_src=twsrc%5Etfw")">#stack</a> <a href="[https://twitter.com/hashtag/memory?src=hash&ref_src=twsrc%5Etfw](https://twitter.com/hashtag/memory?src=hash&ref_src=twsrc%5Etfw "https://twitter.com/hashtag/memory?src=hash&ref_src=twsrc%5Etfw")">#memory</a> <a href="[https://twitter.com/hashtag/functions?src=hash&ref_src=twsrc%5Etfw](https://twitter.com/hashtag/functions?src=hash&ref_src=twsrc%5Etfw "https://twitter.com/hashtag/functions?src=hash&ref_src=twsrc%5Etfw")">#functions</a></p>— Khushal Bhardwaj (@CeleronCoder) <a href="[https://twitter.com/CeleronCoder/status/1571901815616344065?ref_src=twsrc%5Etfw](https://twitter.com/CeleronCoder/status/1571901815616344065?ref_src=twsrc%5Etfw "https://twitter.com/CeleronCoder/status/1571901815616344065?ref_src=twsrc%5Etfw")">September 19, 2022</a></blockquote> <script async src="[https://platform.twitter.com/widgets.js](https://platform.twitter.com/widgets.js "https://platform.twitter.com/widgets.js")" charset="utf-8"></script>

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