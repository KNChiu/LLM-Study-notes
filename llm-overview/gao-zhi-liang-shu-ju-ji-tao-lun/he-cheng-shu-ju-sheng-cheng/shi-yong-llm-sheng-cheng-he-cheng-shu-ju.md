# 使用LLM生成合成數據

參考 [Nemotron-4 340B Technical Report](nemotron-4-340b-technical-report.md) 此篇附錄

## Prompts Used for Synthetic Prompt Generation

### 1. Topics Generation

**Prompt: Generate Macro Topics**

```
Can you generate {n_macro_topics} comprehensive topics that encompass various
aspects of our daily life, the world, and science? Your answer should be a list
of topics. Make the topics as diverse as possible.For example, 1. Food and drinks.
\n2. Technology.\n
```

**Prompt: Generate Subtopics based on Macro Topics**

```
Can you generate {n_subtopics} comprehensive topics that encompass various aspects
of {text1}? Your answer should be a list of topics. Make the topics as diverse as
possible.
```

**Prompt: Generate Math Macro Topics**

```
Can you generate {n_macro_topics} comprehensive topics that encompass the mathematics
knowledge taughted in {school_level}? Your answer should be a list of topics. Make
the topics as diverse as possible.
```

**Prompt: Generate Math Subtopics based on Macro Topics**

```
List {n_subtopics} mathemathics topics that encompass various aspects of "{text1}".
Your answer should be a list of topics. Make the topics as diverse as possible.
```

**Prompt: Classify if an entity is related to Math**

```
Does the concept "{text1}" belong to one of the following categories?
- Math concepts taught at elementary school, middle school, high school, and univiersity.
- Important mathematics axioms, theorems, algorithms, equations, or inequalities.
- Representative math problems, functions, and applications.

Your answer should start with "Yes" or "No".
```

**Prompt: Generate Python Macro Topics**

```
List {n_macro_topics} important concepts in the python language.
```

**Prompt: Generate Python Subtopics based on Macro Topics**

```
List {n_subtopics} important concepts related to "{text1}" in the python language.
```

**Prompt: Classify if an entity is related to Python Programming**

```
Does the concept "{text1}" belong to one of the following categories?
- Programming concepts like loops, functions, and data structures in python.
- Important functions, objects, or libraries in python.
- Mathematical concepts like linear algebra which can be implemented in python.
- Basic algorithms or problems in computer science likes Greedy Search and Dynamics
programming which can be addressed in python.

Your answer should start with "Yes" or "No".
```

***

### 2. Open Q\&A

**Prompt: Generate Open Q\&A questions based on Topics**

```
Can you generate {n_openlines} questions or requests related to {text1}? The questions
and requests should be as diverse possible. Your answer should be a list.
```

**Prompt: Revise Open Q\&A questions**

```
Question: {text1}

Can you revise the question above to include more contexts or details? The revised
questions can be any of the follows:
1. Adding some context to the original question. The context might state the
importance of the question, explain background knowledge, or add other reasonable
information.
2. Change the questions into a different format or style, e.g., imperative
statements, length requirements for the answer, etc.
3. Elongated questions that require to elaborate on specific topic or discuss a
certain point.
4. Any other related questions or statements.

The revised question should contain two, three, or four sentences. You should
generate {n_tasks} revised questions or statements in a list. Make them as
diverse as possible.
```
