# Large Language Models Jailbreak Techniques On Chat-GPT

## Existing Solutions and Opinions

While reading some papers, we chose a few axes which we will discuss:

### 1. Prompt Automatic Iterative Refinement (PAIR) [1]

This paper presents the PAIR principle. It involves a loop that tries to create the best prompt to jailbreak the model using two LLMs. Here is how it works:
1. The system starts from a simple prompt explaining its job and outputs a prompt that is given to the attacker.
2. The prompt is sent to the target that gives a response.
3. A Judge algorithm gives each couple (prompt, response) a note, allowing the program to know whether the jailbreak has succeeded or not.
4. If it is jailbroken, the loop stops. Otherwise, the attacker modifies the prompt based on the previous one, the response, and the note they have been given, and the loop goes back to step 2.

This method is very efficient but has some problems performing against some LLMs because they are too restrictive on the words that can be used and enter defense mode very easily.

### 2. Trees of Attack [2]

In this paper, they present a method based on the PAIR method, but they use a tree of thoughts to attack the LLM and use three LLMs instead of two: 
1. **Attacker**: Generates the jailbreaking prompts using tree-of-thoughts reasoning.
2. **Evaluator**: Assesses the generated prompts and evaluates whether the jailbreaking attempt was successful or not.
3. **Target**: The LLM that we are trying to jailbreak.

The steps followed are:
1. (Branch) The attacker generates improved prompts.
2. (Prune: Phase 1) The evaluator eliminates any off-topic prompts from our improved prompts.
3. (Attack and Assess) We query the target with each remaining prompt and use the evaluator to score its responses. If a successful jailbreak is found, we return its corresponding prompt.
4. (Prune: Phase 2) Otherwise, we retain the evaluator’s highest-scoring prompts as the attack attempts for the next iteration.

This algorithm, as it is based on the PAIR one, has better results on every LLM.

### 3. AutoDAN [3]

Researchers propose AutoDAN, a novel method for generating meaningful jailbreak prompts against aligned LLMs. AutoDAN utilizes a hierarchical genetic algorithm designed for structured discrete data optimization. Key contributions include:
- Introducing AutoDAN as an efficient and stealthy jailbreak attack.
- Specialized functions for structured discrete data optimization.
- Demonstrating outstanding performance in jailbreaking LLMs with notable effectiveness in transferability and universality, surpassing baselines by 60% attack strength while being immune to perplexity defenses.

Although the presented research could be used by adversaries to produce outputs from LLMs that are not aligned with human preferences, it will also stimulate the development of more effective defense strategies, leading to more robust, safe, and aligned LLMs in the long term.

### 4. ArtPrompt [4]

The paper addresses the critical issue of safety in LLMs by highlighting vulnerabilities stemming from the assumption that corpora used for safety alignment are solely interpreted by semantics. To demonstrate this, the authors introduce a novel ASCII art-based jailbreak attack called ArtPrompt and propose the Vision-in-Text Challenge (VITC) as a benchmark to evaluate LLMs' capabilities in recognizing prompts beyond semantics.

**Summary:**
- The objective is to send a message to the LLM using ASCII art. The drawing represents a word that would usually be blocked by the LLM.
- Results indicate that five state-of-the-art LLMs struggle with this task, leading to vulnerabilities.
- ArtPrompt will remain effective in attacking multimodal language models, as the cloaked prompts generated by ArtPrompt are still in text format, which can confuse the model.

### 5. Disguise and Reconstruction Attack (DRA) [5]

The article evaluates how LLMs respond to harmful content by distinguishing two phases: query and completion. It was proven with statistics that the complete LLM model responds better to a context given in the completion part than in the query part. This justifies a real study into differences based on the formulation of the context in the completion part, highlighting the need to ask the model to reformulate the query purpose by itself to respond better to the desired work.

This mathematical functions shows the dependency between the generation of a word in the completion part, and the previous words already generated. We can see that the words in the predicted sequence, interfere with the future generated words and this is why we have to keep an eye on every words we will make the LLM say in its answer.

In another hand, DRA is a technique that consists in, as the name suggests, disguise the prompt by using many useless characters and “hiding” the prompt inside a bigger text for example and making sure the LLM reads the prompt as a whole so he doesnt seen the harmful input, however it will in the end focus only on the prompt we want.

Few algorithms were implemented by the researchers who made this article including a Payload Reconstitution algorithm as followed:

And a Context Manipulation Algorithm as followed:

We can see the effort put on the reconstitution of Q1 which leads to a reformulation of the harmful content in the Completion part.

Also we see that in the context, we avoid the LLM model to talk about things which could interfere with our harmful content, which helps us by getting better results based on the way the words are generated one to another, all depending on the previous ones on the completion words already generated.


## Datasets

### 1 - AdvBench [6]
It is a collection of 500 strings that reflect harmful or toxic behavior, encompassing a wide spectrum of detrimental content such as profanity, graphic depictions, threatening behavior, misinformation, discrimination, cybercrime, and dangerous or illegal suggestions. And 500 harmful behaviors formulated as instructions linked to the previous topics. In our case we would more likely use the last 500 prompts.

### 2 - Tensor Trust [7]
Over 126,000 prompt injection attacks and 46,000 prompt-based "defenses" against prompt injection, all created by players of an online game called Tensor Trust.
