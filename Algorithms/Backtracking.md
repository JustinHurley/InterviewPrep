---
tags: [algorithm, backtracking, recursion]
aliases: [backtracking]
---
# Backtracking
Backtracking is a great algorithm to use when you need to make a permutation of something. As backtracking is a recursive approach to generating multiple options given a single input, without mutating the output in a way that makes it unusable in different cases. When you need to permute multiple answers from a "seed" value, permutation is a great approach.

#### General Idea
The general idea behind backtracking is that we can change the data, call a recursive function with the changed data, then remove the change we just made and now have the option to make different changes.
Say for example we wanted to make all 3-long permutations of the letters `a,b,c`.
We could make a function that looks like this:
```python
currAns = []

backtrack(len):
    if len == 3:
        stop as we made an answer
    else:
        currAns.append('a')
        backtrack(len+1)
        currAns.pop()

        currAns.append('b')
        backtrack(len+1)
        currAns.pop()

        currAns.append('c')
        backtrack(len+1)
        currAns.pop()
```
In the above code, when `len < 3`, we generate answers for all possibilities of the next step, adding `a, b, or c` to the answer. 
This approach allows us to choose what options we want to explore at each length, and could use if statements to dictate logic about the problem. Say for example, we wanted the 3rd spot to never be `a`. To do this would be easy, we just wrap an if check around the block of code that adds a to the `currAns` and checks to see if `len == 2` and if it does, then don't consider that option. 
The below code generalizes this approach a little more, but basically, [[Topics/Backtracking|backtracking]] allows you to have as many permutations as you want, and you can control when you want to run those permutations. Like all recursive approaches, there is a base case, which is just when you have built a solution.

#### Psuedocode
```python
ansSet = []
currAns = []

backtracking(params to keep track of progress to soln):
    if currAns is valid:
        ansSet.add(currAns)
    else:
        # Option 1
        if option1 is valid:
            currAns.addOption1()
            backtracking(params with update)
            currAns.popOption1()
        if option2 is valid:
            currAns.addOption2()
            backtracking(params with update)
            currAns.popOption2()
        .
        .
        .
        if optionN is valid:
            currAns.addOptionN()
            backtracking(params with update)
            currAns.popOptionN()

backtracking(seed params)
return ansSet
```