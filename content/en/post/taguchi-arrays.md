---
title: "Taguchi Arrays - A better way to experiment?"
date: 2023-10-21T11:00:00-00:00
draft: true
tags: ["taguchi", "prototyping", "testing", "experiments"]
featured_image: '/images/frozen_pond.JPG'
disable_share: True
---

Anyone who's ever heard of the scientific method knows how you're "supposed" to do experiments. 
First, you decide what you want to test (e.g. how high your bread rises), then you figure out what variables you want to control (amount of yeast, rising time, additives like sugar or ginger, temperature of the dough before baking), you experiment with a few "levels" of that variable, and you record your results. 

You might think the best way to run this experiment is to test every combination of variables; this lets you 
However, there is a better way you may have heard of; the Taguchi method. If you haven't heard of it, you should watch [this excellent video demonstrating its use](https://www.youtube.com/watch?v=5oULEuOoRd0). There are other more academic videos, but I believe this one best demonstrates the purpose of the method.

In short, the benefit of using the Taguchi method is to significantly reduce the amount of experiments needed to determine the effect of any one of a large set of variables on the outcome of the experiment. 

While testing every combination, also called the "full factorial" method you might need to run seven experiments (including control) to determine the effect of three variables, each with two possible "levels", L and H, with the Taguchi method you would only need to run four (including control). This is well illustrated in the table below.

### Full Factorial method:

|          | A | B | C |
|----------|---|---|---|
| Run 1:   | L | L | L |
| Run 2:   | L | L | H |
| Run 3:   | L | H | L |
| Run 4:   | L | H | H |
| Run 5:   | H | L | L |
| Run 6:   | H | H | L |
| Run 7:   | H | L | H |
| Run 8:   | H | H | H |

### Taguchi method:

|          | A | B | C |
|----------|---|---|---|
| Run 1:   | L | L | L |
| Run 2:   | L | H | H |
| Run 3:   | H | L | H |
| Run 4:   | H | H | L |

The difference between these methods only grows more pronounced as the number of variables increases.

A sharp eye might notice that you can get a similar result by changing one variable at a time.
This is true, but the Taguchi method is slightly different, using what are called "orthogonal arrays", with some notable advantages over the "one at a time" method.
The difference is made a little more clear when you use variables with three different "levels". Let's show it off.

### One at a Time:

| Variable | A | N | C | D |
|----------|---|---|---|---|
| Run 1:   | N | N | N | N |
| Run 2:   | N | N | N | L |
| Run 3:   | N | N | N | H |
| Run 4:   | N | N | L | N |
| Run 5:   | N | N | H | N |
| Run 6:   | N | L | N | N |
| Run 7:   | N | H | N | N |
| Run 8:   | L | N | N | N |
| Run 9:   | H | N | N | N |

### Taguchi method:

| Variable | A | B | C | D |
|----------|---|---|---|---|
| Run 1:   | N | N | N | N |
| Run 2:   | N | L | L | L |
| Run 3:   | N | H | H | H |
| Run 4:   | L | N | L | H |
| Run 5:   | L | L | H | N |
| Run 6:   | L | H | N | L |
| Run 7:   | H | N | H | L |
| Run 8:   | H | L | N | H |
| Run 9:   | H | H | L | N |

In this case, while using the same number of experiment runs, the 'One at a Time' method keeps most of the variables at the low level most of the time, while the Taguchi method uses a much greater variety of levels in the same amount of experiment runs. 

It looks difficult to get data out of the Taguchi experiments, but when you quantify the results of the experiment, you can simply add up your quantitative results for each variable.
In order to see the effect of Variable A, you would add up all the results where that variable is at its L, N, and H levels respectively, and compare the sums. 
You can also average the values if that's more useful. 

# How It Works

There are two primary elements to Taguchi's method.

The first is common to all good experiments, but you should always reinforce the importance of measuring the quality of your experiment's outcome with some kind of objective number. 
Whether you're trying to control the force you need to move a plastic joint, the time it takes for a fuse to burn, or even [how easy it is to peel an egg](https://www.youtube.com/watch?v=5oULEuOoRd0), you must find a yardstick to measure by.

The second, more unique element is the orthogonal array.
Orthogonal arrays are just an organized way to test combinations of variables in an experiment, characterized by a simple rule: in an orthogonal array, *for each level of each variable, all levels of all other parameters are tested at least once*. 

This is an easy rule to say, but it can be pretty difficult to produce orthogonal arrays by hand.
In fact, it can be difficult to produce orthogonal arrays computationally once you get to enough variables and levels, because of the factorial nature of the problem.
Because of this, a number of sources provide pre-made orthogonal arrays for you to use; one great source is [here](https://www.me.psu.edu/cimbala/me345/Lectures/Taguchi_orthogonal_arrays.pdf).

However, I think it's important to understand how these tables are created.

# Computing Orthogonal Arrays

It turns out that orthogonal arrays have a number of uses in math, and there are a number of publications surrounding them.
However, they're very inefficient to calculate, which is why so many pre-made lists of orthogonal arrays exist.
Luckily, there is a [GitHub package](https://github.com/eendebakpt/oapackage) which can help us construct orthogonal arrays of a given class.

Orthogonal arrays are defined by four primary variables.
First, they're defined by how many factors (i.e. variables) they have, and second, by how many levels each of those factors has. 
Third variable is how many "runs" to perform, which directly translates to the number of rows the orthogonal arrays has.
This also directly translates to how many experiments you will perform.

Fourth and last is the "strength" of the array, which has a complicated definition; It's easiest to explain by showing how you might find the strength of an array.
Take any *n* columns and treat them as their own table for a moment.
Remember that each column refers to a variable, and has a given number of levels.
If every possible permutation of the levels shows up in this table at least once, for all combinations of *n* columns, then your orthogonal array has strength *n*.

Generally speaking for Taguchi arrays, we can leave the strength at 2.
However, there is often some difficulty in determining how many runs are needed for a given combination of factors and levels.
Especially for the library above, attempting to create orthogonal arrays with arbitrarily chosen factors, levels, and runs often fails.

The easiest way I've found to calculate the number of runs in an orthogonal array is to start by calculating how many possible permutations of your levels are possible.
For example, lets say I'm working with 7 factors, where the first factor has 4 levels, the second has 3 levels, and the rest each have 2 levels. 
This means total number of permutations are (4^1)\*(3^1)\*(2^5) = 384.
This is equivalent to the number of runs in a "full factorial" version of the experiment as mentioned above.
However, by removing some of the factors (i.e. reducing the exponents) we can find a smaller valid orthogonal array with, in this case, as few as 24 runs.

As a rule of thumb I suggest keeping each of the exponents at 1 or greater; however, your results may vary. 
I find this rule can break down as the numbers of levels increases.

In the end, due to the complexity of the problem and the relative simplicity of the alternative solution, I can only suggest that you use the preconstructed orthogonal arrays found from various sources online.

# My Own Experiment

Rather than simply lecturing on the benefits of Taguchi arrays, I figured I'd put my money where my mouth is and perform a Taguchi-based experiment myself.
In this section I will be optimizing the 

## Experiment and Variables



# Resources

[Penn State University Lecture Notes](https://www.me.psu.edu/cimbala/me345/Lectures/Taguchi_orthogonal_arrays.pdf)

[Explanatory Video by NightHawkInLight](https://www.youtube.com/watch?v=5oULEuOoRd0)

[Wikipedia page on orthogonal arrays](https://en.wikipedia.org/wiki/Orthogonal_array)

[Orthogonal Array Package on GitHub](https://github.com/eendebakpt/oapackage)
