# Introduction
The Viterbi algorithm makes use of dynamic programming methods to find the most likely and optimal path through hidden states, called the "Viterbi Path".

*Implementation*

In our implementation the first observation is G, and the possible hidden states are I and G. That is going to be from our input. We implemented the log-probabilities, so it added these scores along a path. A higher score means a more probable path.

For this project, we used the example observation sequence and probability dictionaries provided in the assignment rather than real genomic data. The example uses two hidden states, I (CpG island) and G (genomic background), to demonstrate how the algorithm works. Our implementation is intended to work with other observation sequences and hidden states as well, although we have primarily tested it using the provided example.

The Viterbi algorithm follows 4 major steps.

Initialization - At index 0, observation is G. For each possible hidden state we are adding its initial log-probability and its log-probability of emitting the observed G. Previous state is None since that is the first position.

Recursion - At every later index, it calculates the best score for ending in each hidden state. To end in G at index 3, we would need to compare the I → G transition and G → G transition. Each candidate adds the previous path's score, the transition log-probability, and the log-probability that G emits the observation at index 3. Since both candidates end in G, they use the same emission term. The output shows that the G → G candidate wins, so Backpointer[3]["G"] is G. Then we repeat the comparison for paths ending in I.

Termination - At the last index, 8, both scores represent paths that cover all nine observations. We pick the state with the higher final score, which in our case was G → -13.9291 against I → -17.2249. This shows G as the last state of the best path, and the score of the complete path.

Backtracking - It starts at G at index 8 and follows one backpointer at a time. At each step, backpointer[index][state] gives the state at the previous index. When we reach G at index 3, Backpointer[3]["G"] tells us to put G at index 2. The I score at index 2 was higher for the first three observations, but the best path through all nine traces back through G there. Reverse the collected states to return them in observation order.

The final output on the project input is:

['G', 'G', 'G', 'G', 'G', 'G', 'G', 'G', 'G']

# Pseudocode
```
FUNCTION _log(probability)
    IF probability is 0
        RETURN negative infinity
    RETURN log(probability)

CLASS Hidden Markov Model
 
    CONSTRUCTOR(initial, transitions, emissions)
        derive states from the initial-probability keys
        store the probabilities
 
METHOD traceback(observations)
 
    IF observations is empty
        RETURN empty path, log probability 0
 
    VALIDATION (validation)
        derive alphabet from the emission-dictionary keys
        IF any initial, transition, or emission table does not sum to 1
            REPORT invalid probabilities
        IF any observation is outside the model's alphabet
            REPORT an invalid observation
 
    INITIALIZATION (_initialization)
        create score table, initially negative infinity
        create previous-state table, initially NONE
        FOR each state
            score[0][state] =
                _log(initial[state])
                + _log(emission[state][observations[0]])
 
    RECURSION (_recursion)
        FOR each position t after the first
            FOR each current state
                FOR each possible preceding state
                    candidate =
                        score[t - 1][preceding state]
                        + _log(transition[preceding state][current state])
                        + _log(emission[current state][observations[t]])
 
                    IF candidate is better than score[t][current state]
                        store candidate as the new score
                        store preceding state as the traceback pointer
 
    TERMINATION
        choose the state with the greatest score at the last position
        save that score as the best log probability
 
        IF that score is negative infinity
            REPORT that no valid path can produce the observations
 
    TRACEBACK
        put the chosen final state at the end of the path
        follow previous-state pointers backward
        reverse the path so it runs from start to end
        RETURN the path and best log probability
```

# Notebook Usage
Import and download any modules required.
Run the code block by block, including the tests provided.

# Biological, Computational, Algorithmic Considerations 

*Biological* 
1. Determining the definition of each hidden state
   * Example: CpG island detection (CpG, Genome) 
2. Nucleotide or Amino Acid Sequence Properties
   * Considering GC-rich regions, conserved motifs 
3. Evolutionary Variation
   * Detecting mutations such as SNPs or indels

*Computational*
1. Memory Complexity and Runtime
   * Example: More than 2 states and a long sequence can create a large matrix and cause a slower runtime. 
2. Numerical Underflow
   * Using logarithmic functions to provide statistical significance 
3. Scalability
   * FASTA files are large and have many reads 

*Algorithmic* 
1. The number of states
   * Too few: missing biological data
   * Too many: overfitting 
2. Unknown nucleotides
   * 'N' is often seen in sequencing data, could we assign a small emission probabilities
3. Edge Cases

# Future Projects 
* *Use of several classes*: An important consideration is that this project will extend beyond the Viterbi algorithm. Using the idea of inheritance will be helpful as we add additional algorithms to HMMs. 
* *Use of decorators*: Although the usage of decorators is not necessary, using them in practice would provide insight into their function in object-oriented programming. Utilizing known and existing decorators such as ```@dataclass```, ```@setter```, or ```@getter``` can be useful in validation of data. 

# Successes
* *Output:* We were able to successfully produce an output for each of our methods within the class. Initially, we thought about creating one Viterbi method within the ```class Hidden Markov Model:```; however for testing purposes, we wanted to construct separate methods for each step (Initialization, Recursion, Termination). This allowed us to debug and test the code as we live-coded one by one together.   
* *Live-Coding Collaboration:* We were able to conduct a live-coding session where we could understand the code and methods of the Hidden Markov Model- Viterbi Algorithm. This facilitated our learning experiences to find gaps in our learning and work collaboratively to understand difficult concepts.
* *Debugging:* One of our successes was being able to identify and fix errors as we went. We ran into quite a few issues, from indentation errors because of having multiple methods within the same class, to figuring out why our functions weren't returning the expected results. Working through these problems together helped us better understand what was happening in the code rather than just fixing the errors. It was also a good reminder that having three brains trying to solve a problem is often much easier than just one!

# Struggles
* *Final Output:* The final output has a 'G' in index 2 (count using zero-based indexing) rather than a 'I', despite the max number of 'I' being greater than 'G'. A possible consideration is that the Viterbi algorithm takes the cumulative logarithmic traceback value, but we are still confused by what it means.
* *Jupyter Notebook:* One challenge we ran into was not being able to separate the methods of our class into different code blocks in Jupyter. We initially wanted to do this to make the code easier to visualize and test, but learned that methods within the same class definition needed to stay together. We also looked into using inheritance, where we could have a basic HMM class and a separate Viterbi HMM class that builds on it. We thought this could be something worth exploring in future projects as we continue expanding our code. Another unexpected challenge was learning how to format Markdown cells within the notebook. This was another learning curve we hadn't anticipated. 


# Personal Reflections
## Group Leader - Aamna
I agree with Graziano and Kailey that working together was one of the most helpful parts of this project. A lot of these programming concepts are still very new to me, especially working with classes and translating mathematical formulas into code. However, I've noticed that I'm getting much better at debugging and understanding error messages, which is helping me code faster because I'm spending less time stuck on small issues. I also find live-coding really helpful because I can ask questions as we go, and sometimes just hearing someone else's thought process helps things click for me.

One thing I really appreciated about this project was seeing how much we've already learned from previous projects. We had already worked with dynamic programming and traceback in Smith–Waterman, so understanding why we were looking for the best overall path came much more easily this time. It was nice to just understand something without having to spend so much time figuring it out again. Working with hidden states and log probabilities was still new, but I feel like I'm starting to recognize how these different algorithms connect, which makes learning the next one a little less overwhelming.


## Other member - Graziano
I would say that the biggest takeaway of this project was how we can improve together as a team. We set time to go through coding together and trying to understand the pseudocode and the mathematical reasoning behind it, those steps made our project stronger overall. I want to bring up few points that working as group made the resolution more clear. First to understand why the third position came out as “G” even though we had higher score at that point. We research and  concluded that the main goal is the optimal path and not only the not highest node point. That would made the understanding of how the table was working easier to be followed. And also how we as a team managed to build the project with the OOP in mind, specially for the next project, which will be useful to have those reusable pieces to be called into the project 8. Working as team for sure alleviate the burden a bit.

## Other member- Kailey
The concept of the Viterbi Algorithm within Hidden Markov Models made sense to me, but viewing the mathematical formulas and all of the various variables was a bit overwhelming initially. During lecture, it was noted that the variables will change based on the instructor and the method being taught, and that was true. A few Youtube videos and articles in, there was large variation within the variables, but the formulas and concepts remained the same.

Collaboration was a key factor on this project. Due to a few time constraints, we were not able to meet until Tuesday, but it ended up being the most productive. We shared our pseudocode and background information and took the time to live-code together. The concept of object-oriented programming is still new and complex to me, but I found this collaborative session to be useful. I gained insight into how classes work, how a constructor functions and how to effectively test each bit of code. The meeting was long, but very informative. Each person was able to understand the concept better without having to spend extra time working on it independently.

# Generative AI Appendix
Graziano:
Description of which generative AI was used and its version. NotebookLM 2.0 and Claude Opus 5
The entire prompt that was used to generate the content. Can you help me to fix my pseudocode to a the github format? Can you explain step by step how the Viterbi algorithm works mathematically, do not provide code samples or code output. I only want the reasoning behind the process.
An explanation of how it was used (e.g., to generate ideas). I used it as a tool to learn the concepts described on the articles and materials, asking for further clarification on the material and extra examples.
A justification for why generative AI was used. AI was used to bring a different explanation of the problem. It breaks down the process with simpler examples and guide was to understand the reasoning behind the formula.

Aamna:
I used Chatgpt throughout this project to help me better understand the Viterbi algorithm, debug errors in our code, and make sense of some of the outputs we were getting. I mainly used it as a way to ask questions when I got stuck and to understand why something wasn't working. I also used it to review our implementation against the assignment requirements and help with formatting and organizing parts of the README. We worked through the code together as a group, and I found it very helpful as an additional resource to check my understanding and work through issues as they came up.

Kailey: 
I used Claude Sonnet 5.5 for this project for learning how to format Markdown files (or blocks), restructuring the pseudocode and understanding logarithms. The format of Markdown code blocks is not as simple as a Word document where you can click a button and it will be bolded or highlighted. I had Claude create a table for me to reference for future usage. Restructuring the pseudocode was put into Claude to review for gaps from modifying the code. Also understanding logarithmic functions was not my strong suit at all, so asking Claude to explain it to me as if I were in 3rd grade proved to be helpful. 
