# ReAct Agent from Scratch

This repository contains a simple Python implementation of a ReAct Agent using the OpenAI SDK. It allows an LLM to interact with external Python functions to answer questions that require calculation or specific data retrieval.
---
# How It Works: The ReAct Loop
The agent follows a cognitive cycle inspired by the ReAct (Reason + Act) framework. Instead of guessing a complex answer, the LLM is prompted to think step-by-step.

## The 4 Stages
### Thought: 
The LLM describes its reasoning process about the user's question.

### Action: 
The LLM decides to use a specific tool (e.g., calculate) and provides the input.

### PAUSE: 
The Python script stops the LLM, parses the requested action using Regex, and runs the actual Python function.

### Observation: 
The result of the Python function is fed back to the LLM as a new prompt, allowing it to move to the next step or provide the final Answer.
--- 


# Project Structure
## 1. The Agent Class
This class manages the "Short-term Memory" of the AI.

`__init__`: Stores the system prompt that defines the ReAct behavior.

`__call__`: Allows the agent instance to be called like a function. It appends the user message, executes the LLM call, and saves the assistant's response to the history.

execute: The direct interface with the LLM (`Gemini 2.5 Flash` via OpenRouter).

## 2. Tool Integration
Tools are defined as standard Python functions and mapped in the known_actions dictionary:

calculate(what): Uses Python's `eval()` to solve math.

average_dog_weight(name): A mock database for dog breed weights.

## 3. The query Loop

This is the "Engine." it runs a while loop for a maximum of max_turns (default 5). It uses a Regular Expression (action_re) to detect if the LLM's output contains an instruction to perform an action.
--- 


# The Building Block for LangGraph
This code represents the "manual" version of what frameworks like LangGraph automate. Understanding this logic is crucial for moving to LangGraph for several reasons:

## State Management: 
In this code, self.messages is the "State." LangGraph formalizes this into a State Graph where each step (Node) updates a shared state object.

## Nodes and Edges: 
The while loop and if actions: check are the "Control Flow." In LangGraph, the LLM is a Node, the Tool execution is another Node, and the Regex logic becomes a Conditional Edge that decides whether to loop back or finish.

## Persistence: 
While this code loses memory once the script ends, LangGraph adds "Checkpointers" to save the state of the agent so it can resume conversations later.

## Human-in-the-loop:
Because this loop "PAUSES" after an action, it mimics how advanced frameworks allow humans to approve an action before the agent continues.