### My steps
1. Listen ... look for clues. Restate what you think the problem is!
  a. And get the requirements. Listen and look for clues. Interviewers do not try to give you clues that do not apply.
  b. If your answer does not use the details maybe its suboptimal
2. Write out examples.
  a. Look at larger examples?? Because it will make your brain do some work and that can point out the optimal algorithm. If its a simple example you can see the answer right away. Do not give special cases to the example. If its not sorted do not give a sorted example.
3. Give the brute force. Slow and obvious. State the Space and Time Complexity of it.
4. Optimize the algorithm
  a. You might spend a good bit of your time optimizing
  b. Some interviewers might ask you to write out the brute force which is okay as well.
5. Walk through algo. *Disclaimer*: Make sure you know exactly what you're doing before you code. It does not have to be completely correct, but make sure its as correct as possible!
6. Write code. Expect to shoot for real correct code.
7. Verification Process
  a. Spend time looking for improvements: style, maintainability, etc.
  b. Show that you are a great coder who cares about writing quality code.

### For optimizing
- Look for bottlenecks, unnecessary work, and duplicated work.
- Optimizing with space and time.
  - What is the best conceivable runtime?
- Optimizing with DIY
  - Forget the computer stuff. Use a large and generic example and try to solve it intuitively
### Wait until you're ready to code.
- I understand the basic gist of what I'm going to write is not enough.
- Some candidates rush into code when they don't fully understand what they're going to do. This causes a lot of mistakes. Make sure you know exactly what the steps are.
- Interviewer might be like I see the basic idea, can i see the code, but do not start until you are ready!
- Wait until your interviewer is ready as well!
- *Never code until your interviewer says lets see the code"
- You should know things like:
  - Your input/output data types
  - Index math
  - Helper functions
  - Data structures and when they change
- **Wait until you and your interviewer are ready!**
  - Walk through the algorithm details, data structures, indices, etc.
### Coding
- Demonstrate that you're a great coder:
  - correctness
  - readability
  - maintainability
  - performance
- Use the neat features of your programming language!
  - Ask if you're not sure
  - Can often "make up" reasonable functions (or assume at first and implement later)
- Good style is bad, consistency!
- Think about error cases, boundary checks, etc...
  - Case by case basis not every problem needs this.
  - Ask your interviewer if there is a lot of boundary checks if we can validate it through helper function.
- Never write loose code. Make sure it is part of a function
- The interviewer wants to see good Signal that you are a good coder.
  - Ignore things that don't show good signal (but check with your interviewer first)
    - Get the biggest element in the array etc.
### Verification
- Do not use your original example to walk through your code. Because its likely a very large example and it will be time consuming to walk through it.
  - Also better to test your design with an example you did not use to come up with that design
1. Walk through a conceptual walkthrough. **FAST!**
2. Hot spots. Where are the high risk lines of code? **FAST!**
   a. Math
   b. Moving indicies
   c. Parameters when calling recursion
   d. Base cases and so on!
3. Test cases
  a. Small Test Cases **MEDIUM!**
  b. Verify some of edge cases.  **MEDIUM!**
    - Boring Edge Cases: Nulls, emptys, single element
    - Interesting Edge Cases: All punctuation, Sorted Strings, all Duplicates.
  c. Big test cases (if you have time) **SLOW!**
### Clean up
- Make sure you are actually running the lines of code with your example. Not just your assumption of processing it.
- Make sure as you are executing each line think about what is happening.
- Do not do quick sloppy fixes. Do not panic when you find a bug. Its normal.
  - Take my time find the right fix.
- When all is done, go through the code one last time for any improvements.

### Communication
- Use examples to explain your approach
- Be open about your mistakes.
- Admit if you've heard the question before?
- Interviewer might be giving hints and trying to lead you to an answer. Keep following their lead after they have given the hints.