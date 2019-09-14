# Stack and Call Stack

The word “stack” has very different meanings in different contexts.  It’s use ranges from a “unit of measure especially for firewood that is equal to 108 cubic feet” in the United Kingdom to a [competitive sport][a]. This article will summarize the concept of stack in computation science and software with specific to programming language (i.e. Node.js / Javascript).

## What is stack?

The word without context can be confusing within the technology and computer science world:

-	 “technology or solution stack”[<sup>1</sup>][1] refers to collection(s) of software to support an application;
-	“protocol stack”[<sup>2</sup>][2] is discussed in relations to computer networking;
-	“stack(s)” in the Apple ecosystem may be the folder view in macOS[<sup>3</sup>][3] or collection of documents in HyperCard[<sup>4</sup>][4];
-	“stack-based memory”[<sup>5</sup>][5] is about method to add or remove data in computing architectures; and
-	“call stack”[<sup>6</sup>][6] is the management of subroutines in (computer) programming languages.

While the other examples are interesting, only the last two are directly relevant applications of the common computational concept – “stack”.  For scope of this article, I will focus on exploring the call stack in programming language (I will briefly touch on hardware memory but not directly speaking to stack-based memory).

Stack is one of abstract data type – it is essentially a linear method to manage collections of elements.  Stack’s behaviour is best described as

>“last-in, first-out” (LIFO)

based on the combination of its two principle operations: **Push** and **Pop**. You can read more on the academic explanation on both abstract data types and their axioms and theorems [here][b].  The two most important take away from the academic discussion surrounding stacks are:

> a stack is **empty** when created.

> a push followed by a pop to the stack means the stack **remains the same**.

A visual example of stack from Wikipedia:
![Stack operation](https://upload.wikimedia.org/wikipedia/commons/b/b4/Lifo_stack.png)

 
You can find the concept of stack in every day life with the classic example of trays in cafeteria.  [Stacked parking][c] is a more modern example.

The credit for earliest formative application of stack in computation belongs to Alan M. Turing[<sup>7</sup>][7].  Many of today’s common computational concepts (e.g. using call stack to track subroutines) were discussed in his 1945 paper.  Other common computational applications of the stack include:
-	backtracking (in gaming, web browsers, word processors, spoilers: hooks and class in React);
-	hardware memory allocations;
-	prefix and postfix calculations;
-	check for matching tags in markup languages (e.g. HTML and XML); and
-	control stack in compiler to translate high-level languages[<sup>8</sup>][8].

## How to build a stack?

Stacks can be easily implemented with an **array** or **linked lists** in most programming languages and there is no single correct way.  The chief challenge with implementation is enforcing stack’s principle functionalities, namely:

1)	restricting users push and pop operations to the top-most element; and
2)	track the stack’s size and capacity to determine if it maybe empty or full.

Other non-essential functionality includes “peeking” the first element[<sup>9</sup>](#footnote).  Many common software programming languages like C/C++, Java, Javascript, etc. has classes to implement stack using array, linked lists, or custom algorithms.  For example, stack can be easily implemented in *Javascript* using built-in functions (```slice```, ```concat```, and ```length```) in the Array class.

```
class arrayStack {
  constructor(max) {
    this.max = max; // this may not apply if stack is assumed to have dynamic size
    this.stack = [];
  }
  ...// Getters

  ...// Methods
}
```

```
// Getters
get getMax() { return this.max; } // this may not apply if stack is assumed to have dynamic size
get getSize() { return this.stack.length; }  // this is a non-essential functionality
get getStack() { return this.stack; }
get getHead() { return this.stack[this.getSize() – 1]; } // this is a non-essential functionality
```

```
// Methods
push (element) {
  if (!this.isFull()) {
  this.stack = this.stack.concat(element);
  }
return this;
}

pop () { 
  if (!this.isEmpty()) {
  this.stack = this.stack.slice(0, this.getSize() – 1);
  }
  return this;
}

isEmpty() { this.getSize() === 0; } // this is a non-essential functionality

isFull() { this.getSize() > this.size; } // this may not apply if stack is assumed to have dynamic size
```

Stack, in it’s purest form, allows only access to the **top-most** element.  This concept becomes very powerful (and exponentially more complex) when applied to solving the joint problems of

> allocate hardware memory appropriately

> process software routines.

his is where the call stack comes in.

## What is call stack?
Imagine you are working with the tapes in the original Turing machine.  How can you best allot time and amount of tape to each program?

A simple way would be to dedicate equal amount of tape and time to each program.  You soon observed two facts:
*	not all programs run in equal time; and
*	you cannoy re-use the tape until the program is completed.

This lead to lack of tape for storage as programs demand grew while some tapes sit idle.
Fortunately, you also noticed program and its sub-routines are invoked linearly with the last sub-routine always processed first.  This linear call and LIFO processing of sub-routines behaves like a stack:
1.	A sub-routine push itself onto a stack after being called; then
2.	Popped itself out of the stack after it completes.

The same extends to variables within a sub-routine (i.e. variables and functions in Javascript).
 
The application of applying the stack concept to sub-routine is appropriately named “**call stack**”.

[Here][d] is a fun activity to illustrate how call stack works.

Applying stack to temporary data storage in memory ensures sub-routines do not destroying data from sub-routines invoked earlier.  Solving the need to protect data in temporary data storage introduced numerois new computational capacities to programming, including:

*	Recursive or nested sub-routines (e.g. one function calling another);
*	Pass values between functions (e.g. parameters and closure in Javascript);
*	Modularize commonly used sub-routines (e.g. call same function multiple times in different part of the program); and
* Significantly more compact programs.

Repeatable use of temporary data storage significantly reduced the need for hardware and improved execution speed[<sup>10</sup>][10].  Both factors increased the viability to commercialize computational products – opening the door to eras of software advancement.

## How is call stack work in programming languages?
Call stack is used when program is processed by an interpreter.  The concept and fundamental operation of a call stack is similar across most programming languages.  *Fun fact: some programming language, like COBOL, do not use call stack.*

The call stack is also assigned a register by the hardware to track where the program last executed.  The call stack is notified when the sub-routine is completed (i.e. via a return statement in *Javascript*), the sub-routine is popped with the pointer returning to its previous location (i.e. the memory location of the next topmost item in the call stack).  There are other memory allocation process (i.e. memory heap) and sub-routine entry (i.e. stack frame) technical name involve but the idea can be generalized here.

See this Javascript [example][e] on basic operation of call stack in Javascript.

Note in the above example ```console.log(“Hello “ + who);``` is not a sub-routine but a side-effect. ```Undefined``` is automatically added to the stack when ```return``` is not stated or stated without an explicit value.

## Call stack and requests to outside of the program.
Call stack becomes interesting when it interacts with the hosting environment (i.e. Javascript engine or Yet Another Ruby Virtual machine and interpreters). Each programming language take a different approach, so we will focus on Javascript.

Javasript runs on a “single-thread” - it only has one call stack and it reacts based on events occurrences.  This is generally sufficient until a program needs to send or received data outside of the program (e.g. host machine hard drive, audio / visual inputs, another server).  These data transmission often requires time to process and holds up the stack and the website – *not the best user experience*.

There are different ways for programming languages to handle this problem: Javascript resolves this problem with the event loop (you can ignore the Web API and Callback Queue in the mean time for a generalized example).
![Stack operation](https://scotch-res.cloudinary.com/image/upload/w_1050,q_auto:good,f_auto/media/4974/xCkAPcmuQNqQCGpO2avR_Event-loop.png.jpg)

The event loop has a single task of monitoring the Call stack and the call back when the data retrieval or sending is completed:
1.	When an asynchronous call is made the engine makes the appropriate requests to outside of the program;
2.	Call stack pops the sub-routine;
3.	Call stack continues to work through the stack;
4.	In the mean time, the event loop is notified when the asynchronous call is completed;
5.	When the calls tack becomes empty, the event loop pushes the returned call back to the Call stack; and
6.	Call stack executes the last sub-routine.
Below is an example with setTimeOut() in Javascript

![Asynchronous call](https://miro.medium.com/max/808/1*TozSrkk92l8ho6d8JxqF_w.gif)

# Conclusion
"**Stack**" (click [here][f] for another fun example) can mean multiple things. This article walked through the concept of stack with general explanations on it's application in computation science and software (e.g. Call stack). An example to implement it in Javascript was provide (e.g. arrayStack). Stack was one of the key concepts leading to explosive growth in computational power which enabled rapid commercialization of computational products.  It is still highly relevant in today’s world.

## Corrections
Please leave any error corrections, constructive feedback, and/or recommendations in the comments.

### Footnote
<sup>9</sup> Non-essential operations and derivative concepts include Peak of the top element and Size. These are non-essential as they can be achieved by varying operations of Push and Pop. For example, pop all items out of stack to count the size then push them all back.

# Further Learnings

## Debugging
[Chrome developer tools](https://developers.google.com/web/tools/chrome-devtools/javascript/reference) comes with the option to view the current call stack.  Alternatively, you can pause the script and review the Call stack to determine what steps were executed to this point.  It also comes with the Async checkbox option to enable async call stacks.
![Chrome developer tools](https://developers.google.com/web/tools/chrome-devtools/javascript/imgs/call-stack.svg)  

## Concurrency and Parallelism
Alternative solutions to the aforementioned wait time problem from invoking calls to resources outside of the program can be solved by working with more than one Call stack.  Ruby, for example, allowing separate process (i.e. new memory using ```fork()```) and thread (i.e. ```Thread.new``` to create a child stack to main Call stack).  Similar to Javascript, the limitation with Ruby will be at the interpreter level.  Here is a [practical discussion](https://www.toptal.com/ruby/ruby-concurrency-and-parallelism-a-practical-primer).

## References
- SpeedStacks Inc., 2014, December 17., Episode 1 – Introduction – Learn To Stack [Video file]. Retrieved from https://www.youtube.com/watch?v=F89vHYoM8XM
- Meriam-Webster., n.d., Stack, accessed 13 September 2019, https://www.merriam-webster.com/dictionary/stack
- Wikipedia, n.d., Solution stack, accessed 13 September 2019, https://en.wikipedia.org/wiki/Solution_stack
- Wikipedia, n.d., Protocol stack, accessed 13 September 2019, https://en.wikipedia.org/wiki/Protocol_stack
- Wikipedia, n.d., Stacks (Mac OS), accessed 13 September 2019, https://en.wikipedia.org/wiki/Stacks_(Mac_OS)
- Wikipedia, n.d., HyperCard, accessed 13 September 2019, https://en.wikipedia.org/wiki/HyperCard
- Wikipedia, n.d., Stack-based memory allocation, accessed 13 September 2019, https://en.wikipedia.org/wiki/Stack-based_memory_allocation
- Wikipedia, n.d., Call stack, accessed 13 September 2019, https://en.wikipedia.org/wiki/Call_stack
- Wikipedia, n.d., Stack (abstract data type), accessed 13 September 2019, https://en.wikipedia.org/wiki/Stack_(abstract_data_type)
- R.C. Lacher, 1999 – 2012, 5. Abstract Data Types: Stacks and Queues, accessed 13 September 2019, http://www.cs.fsu.edu/~lacher/lectures/Output/adts/index.html?$$$toc.html$$$
- Wikipedia, n.d., Stack [Image file], accessed 13 September 2019, https://upload.wikimedia.org/wikipedia/commons/b/b4/Lifo_stack.png
- B. E. Carpenter, R. W. Doran, January 1977, The other Turing machine, The Computer Journal, Volume 20, Issue 3, accessed 13 September 2019., https://academic.oup.com/comjnl/article/20/3/269/751954
- R. Farney, 23 May 2018, Breaking Down the Call Stack, accessed 13 September 2019, https://medium.com/@ryanfarney/breaking-down-the-call-stack-e68b5633fbad
- K. Kontogiannis, R. Solis-Oba, CS 1027b: Computer Science Fundamentals II. Winter 2019 [PDF course notes], accessed 13 September 2019, https://www.csd.uwo.ca/Courses/CS1027b/notes/CS1027-003-Stacks-W17.pdf
- P. Sharma, Compiler Design | Runtime Environments, accessed 13 September 2019, https://www.geeksforgeeks.org/compiler-design-runtime-environments/
- P. Koopman, 1989, 1.2 What is a stack?,
https://users.ece.cmu.edu/~koopman/stack_computers/sec1_2.html#122
- Mozilla contributors, 18 March 2019, Call Stack, accessed 13 September 2019, https://developer.mozilla.org/en-US/docs/Glossary/Call_stack
- M. Haverbeke, 4 December 2018, Eloquent Javascript, 3rd Edition, accessed 13 September 2019, https://eloquentjavascript.net/03_functions.html#h_D2Yui+mx6D
- C. Freeborn, 11 January 2018, Understanding the Javascript call stack,  accessed 13 September 2019, https://www.freecodecamp.org/news/understanding-the-javascript-call-stack-861e41ae61d4/

[a]: https://www.youtube.com/watch?v=F89vHYoM8XM
[b]: http://www.cs.fsu.edu/~lacher/lectures/Output/adts/index.html?$$$toc.html$$$
[c]: http://www.youtube.com/watch?feature=player_embedded&v=qpOmV4EKF3U
[d]: https://backend.turing.io/module1/lessons/call_stack
[e]: https://eloquentjavascript.net/03_functions.html#h_D2Yui+mx6D
[f]: https://www.youtube.com/watch?v=YEQUrqEt-NM


[1]: https://en.wikipedia.org/wiki/Solution_stack
[2]: https://en.wikipedia.org/wiki/Protocol_stack
[3]: https://en.wikipedia.org/wiki/Stacks_(Mac_OS)
[4]: https://en.wikipedia.org/wiki/HyperCard
[5]: https://en.wikipedia.org/wiki/Stack-based_memory_allocation
[6]: https://en.wikipedia.org/wiki/Call_stack
[7]: https://academic.oup.com/comjnl/article/20/3/269/751954
[8]: https://www.geeksforgeeks.org/compiler-design-runtime-environments/
[10]: https://users.ece.cmu.edu/~koopman/stack_computers/sec1_2.html#122
