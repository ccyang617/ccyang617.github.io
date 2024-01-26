---
layout: essay
type: essay
title: "Asking Questions The Right Way"
# All dates must be YYYY-MM-DD format!
date: 2024-1-25
published: true
labels:
  - Questions
  - Answers
  - StackOverflow
---

## What makes a question a "Smart Question"?

A smart question is often determined by whether the question asker has tried to find the answer themselves. This doesn't mean that all questions asked without research are bad questions, but instead means that it takes a baseline level of knowledge to be able to clearly communicate what exactly is being misunderstood. Often on the web, many users often are unable to clearly describe exactly where the area of confusion is. Often times you will see questions posed that follow the form, "This is program is not working, why is it not doing what I want it to." followed by a large block of code. Often times these types of questions go unanswered because many people who are able to help and understand simply do not want to have to trace the entire block of code to give you the answer. When the question asker is able to clearly show what they have tried so far, explain what exactly they are confused about, and somewhat show their level of understanding in the topic, then the question is able to be answered.

## What’s a smart question?

On Stack Overflow, an online resource for asking programming questions, there are thousands of asked questions on varying topics. Often many of these questions go unanswered but the ones that do are typically "Smart Questions". This is because it is easier to answer a question with all of the relevant information available. Users answering questions do not want to go back and forth online asking the asker what level of competence do they have with this subject. The questions that get answered can be answered immediately without much thought besides how they should explain the topic to a user at a certain level.

Here is an example I found on Stack Overflow of what I believe is a "Smart Question"

```
Q: Why is processing a sorted array faster than processing an unsorted array?

In this C++ code, sorting the data (before the timed region) makes the primary loop ~6x faster:

#include <algorithm>
#include <ctime>
#include <iostream>

int main()
{
    // Generate data
    const unsigned arraySize = 32768;
    int data[arraySize];

    for (unsigned c = 0; c < arraySize; ++c)
        data[c] = std::rand() % 256;

    // !!! With this, the next loop runs faster.
    std::sort(data, data + arraySize);

    // Test
    clock_t start = clock();
    long long sum = 0;
    for (unsigned i = 0; i < 100000; ++i)
    {
        for (unsigned c = 0; c < arraySize; ++c)
        {   // Primary loop.
            if (data[c] >= 128)
                sum += data[c];
        }
    }

    double elapsedTime = static_cast<double>(clock()-start) / CLOCKS_PER_SEC;

    std::cout << elapsedTime << '\n';
    std::cout << "sum = " << sum << '\n';
}

    Without std::sort(data, data + arraySize);, the code runs in 11.54 seconds.
    With the sorted data, the code runs in 1.93 seconds.

(Sorting itself takes more time than this one pass over the array, so it's not actually worth doing if we needed to calculate this for an unknown array.)

Initially, I thought this might be just a language or compiler anomaly, so I tried Java:

import java.util.Arrays;
import java.util.Random;

public class Main
{
    public static void main(String[] args)
    {
        // Generate data
        int arraySize = 32768;
        int data[] = new int[arraySize];

        Random rnd = new Random(0);
        for (int c = 0; c < arraySize; ++c)
            data[c] = rnd.nextInt() % 256;

        // !!! With this, the next loop runs faster
        Arrays.sort(data);

        // Test
        long start = System.nanoTime();
        long sum = 0;
        for (int i = 0; i < 100000; ++i)
        {
            for (int c = 0; c < arraySize; ++c)
            {   // Primary loop.
                if (data[c] >= 128)
                    sum += data[c];
            }
        }

        System.out.println((System.nanoTime() - start) / 1000000000.0);
        System.out.println("sum = " + sum);
    }
}

With a similar but less extreme result.

My first thought was that sorting brings the data into the cache, but that's silly because the array was just generated.

    What is going on?
    Why is processing a sorted array faster than processing an unsorted array?

The code is summing up some independent terms, so the order should not matter.
```

The heading of this question could use a few adjustments such as what programming language their question is concerned with but otherwise is very clear and concise about their confusion. The user starts off by asking the question in the heading and then showing the code they are testing, what languages they have tested the program in, and where exactly the confusion stems from. It can be seeen that the user explains that they believed it to be a compiler issue, then tested the same program in javascript, and received a similar result. They explained what their thoughts on the topic originally were before being proven wrong and why they are confused. This question was able to show previous steps they tried, what level of programmer they might be, and their overall thought process throughout problem solving.

```
A: 
You are a victim of branch prediction fail.
What is Branch Prediction?

Consider a railroad junction:

Now for the sake of argument, suppose this is back in the 1800s - before long-distance or radio communication.

You are a blind operator of a junction and you hear a train coming. You have no idea which way it is supposed to go. You stop the train to ask the driver which direction they want. And then you set the switch appropriately.

Trains are heavy and have a lot of inertia, so they take forever to start up and slow down.

Is there a better way? You guess which direction the train will go!

    If you guessed right, it continues on.
    If you guessed wrong, the driver will stop, back up, and yell at you to flip the switch. Then it can restart down the other path.

If you guess right every time, the train will never have to stop.
If you guess wrong too often, the train will spend a lot of time stopping, backing up, and restarting.

Consider an if-statement: At the processor level, it is a branch instruction:

if(x >= 128) compiles into a jump-if-less-than processor instruction.

You are a processor and you see a branch. You have no idea which way it will go. What do you do? You halt execution and wait until the previous instructions are complete. Then you continue down the correct path.

Modern processors are complicated and have long pipelines. This means they take forever to "warm up" and "slow down".

Is there a better way? You guess which direction the branch will go!

    If you guessed right, you continue executing.
    If you guessed wrong, you need to flush the pipeline and roll back to the branch. Then you can restart down the other path.

If you guess right every time, the execution will never have to stop.
If you guess wrong too often, you spend a lot of time stalling, rolling back, and restarting.

This is branch prediction. I admit it's not the best analogy since the train could just signal the direction with a flag. But in computers, the processor doesn't know which direction a branch will go until the last moment.

How would you strategically guess to minimize the number of times that the train must back up and go down the other path? You look at the past history! If the train goes left 99% of the time, then you guess left. If it alternates, then you alternate your guesses. If it goes one way every three times, you guess the same...

In other words, you try to identify a pattern and follow it. This is more or less how branch predictors work.

Most applications have well-behaved branches. Therefore, modern branch predictors will typically achieve >90% hit rates. But when faced with unpredictable branches with no recognizable patterns, branch predictors are virtually useless.

Further reading: "Branch predictor" article on Wikipedia.
As hinted from above, the culprit is this if-statement:

if (data[c] >= 128)
    sum += data[c];

Notice that the data is evenly distributed between 0 and 255. When the data is sorted, roughly the first half of the iterations will not enter the if-statement. After that, they will all enter the if-statement.

This is very friendly to the branch predictor since the branch consecutively goes the same direction many times. Even a simple saturating counter will correctly predict the branch except for the few iterations after it switches direction.

Quick visualization:

T = branch taken
N = branch not taken

data[] = 0, 1, 2, 3, 4, ... 126, 127, 128, 129, 130, ... 250, 251, 252, ...
branch = N  N  N  N  N  ...   N    N    T    T    T  ...   T    T    T  ...

       = NNNNNNNNNNNN ... NNNNNNNTTTTTTTTT ... TTTTTTTTTT  (easy to predict)

However, when the data is completely random, the branch predictor is rendered useless, because it can't predict random data. Thus there will probably be around 50% misprediction (no better than random guessing).

data[] = 226, 185, 125, 158, 198, 144, 217, 79, 202, 118,  14, 150, 177, 182, ...
branch =   T,   T,   N,   T,   T,   T,   T,  N,   T,   N,   N,   T,   T,   T  ...

       = TTNTTTTNTNNTTT ...   (completely random - impossible to predict)

What can be done?

If the compiler isn't able to optimize the branch into a conditional move, you can try some hacks if you are willing to sacrifice readability for performance.

Replace:

if (data[c] >= 128)
    sum += data[c];

with:

int t = (data[c] - 128) >> 31;
sum += ~t & data[c];

This eliminates the branch and replaces it with some bitwise operations.

(Note that this hack is not strictly equivalent to the original if-statement. But in this case, it's valid for all the input values of data[].)

Benchmarks: Core i7 920 @ 3.5 GHz

C++ - Visual Studio 2010 - x64 Release
Scenario 	Time (seconds)
Branching - Random data 	11.777
Branching - Sorted data 	2.352
Branchless - Random data 	2.564
Branchless - Sorted data 	2.587

Java - NetBeans 7.1.1 JDK 7 - x64
Scenario 	Time (seconds)
Branching - Random data 	10.93293813
Branching - Sorted data 	5.643797077
Branchless - Random data 	3.113581453
Branchless - Sorted data 	3.186068823

Observations:

    With the Branch: There is a huge difference between the sorted and unsorted data.
    With the Hack: There is no difference between sorted and unsorted data.
    In the C++ case, the hack is actually a tad slower than with the branch when the data is sorted.

A general rule of thumb is to avoid data-dependent branching in critical loops (such as in this example).

Update:

    GCC 4.6.1 with -O3 or -ftree-vectorize on x64 is able to generate a conditional move, so there is no difference between the sorted and unsorted data - both are fast. This is called "if-conversion" (to branchless) and is necessary for vectorization but also sometimes good for scalar.

    (Or somewhat fast: for the already-sorted case, cmov can be slower especially if GCC puts it on the critical path instead of just add, especially on Intel before Broadwell where cmov has 2-cycle latency: gcc optimization flag -O3 makes code slower than -O2)

    VC++ 2010 is unable to generate conditional moves for this branch even under /Ox.

    Intel C++ Compiler (ICC) 11 does something miraculous. It interchanges the two loops, thereby hoisting the unpredictable branch to the outer loop. Not only is it immune to the mispredictions, it's also twice as fast as whatever VC++ and GCC can generate! In other words, ICC took advantage of the test-loop to defeat the benchmark...

    If you give the Intel compiler the branchless code, it just outright vectorizes it... and is just as fast as with the branch (with the loop interchange).

    Clang also vectorizes the if() version, as will GCC 5 and later with -O3, even though it takes quite a few instructions to sign-extend to the 64-bit sum on x86 without SSE4 or AVX2. (-march=x86-64-v2 or v3). See Why is processing an unsorted array the same speed as processing a sorted array with modern x86-64 clang?

This goes to show that even mature modern compilers can vary wildly in their ability to optimize code...


```
 
The question asker was able to generate many responses from this question all very similar but different. The first and most voted answer is the one I have published with this essay since to me it shows the understanding the answerer had of the experience level of the asker. It is clear that although the question asker has a decent understanding of programming, they are most likely a student or someone with beginner/no knowledge of computer engineering concepts. Here the answerer is able to try and create a metaphor for how branch-prediction works in a computer processing unit to easily explain the concept. This is for the benefit of the question ansker and was genuinely a great example of how it works. In this example the question asker was able to give the answerer enough information about themself and their experience so that the answerer could tailor the best method of explanation for them.

## How to get ghosted online.

Some questions are not very informative, and sometimes users can ask too broad of a question leading to no answers. In this example a user is asking about Kotlin, an android development language based on java. This looks like the first step to a kotlin tutorial and the user is confused why it is not working.

```
Q:

I have the following program:

//Main.kt
import kotlinx.coroutines.*
import java.util.concurrent.*

fun main() {
        val scope = CoroutineScope(Job() + Dispatchers.Default)

        scope.launch{
                println("hello")
        }
}

I tried to run it in a command line:

i@LAPTOP:/mnt/d/PROJECTS$ ls
Main.kt  annotations-23.0.0.jar  kotlin-stdlib-1.9.21.jar  kotlinx-coroutines-core-jvm-1.8.0-RC2.jar
i@LAPTOP:/mnt/d/PROJECTS$ kotlinc Main.kt -cp annotations-23.0.0.jar:kotlin-stdlib-1.9.21.jar:kotlinx-coroutines-core-jvm-1.8.0-RC2.jar
i@LAPTOP:/mnt/d/PROJECTS$ java -cp annotations-23.0.0.jar:kotlin-stdlib-1.9.21.jar:kotlinx-coroutines-core-jvm-1.8.0-RC2.jar:. MainKt
i@LAPTOP:/mnt/d/PROJECTS$

I expected to see hello in the command line but for some reason my program prints nothing. Why?

```
This question received no answers and I can see why. They simply show what they have input, and explain nothing happens. The question that follows is "Why?" or more precisely "Why is hello not showing up in the command line." but the answerer has almost nothing to work with. Perhaps the code will be enough for a user to go through and figure out what is wrong. Luckily in this case the block of code is very short and would not be too hard to trace, however this question is still posed in a bad way.

## Conclusion

When expecting others, especially kind-hearted strangers, to assist us it is also our duty to make it easy for them to provide an answer. Nobody wants to do unneccesary work, especially when volunteering their time to assist a stranger on the internet. The best way to go about asking questions is clearly to provide enough context, information, and overall scope of the issue as bestas possible. This way the answerers of the world can easily help us without devoting a large amount of time simply understanding the problem.
