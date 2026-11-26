# PrintSmart – AI Printing Shop Assistant

## About the Project

As part of my AI Agents learning, I explored how to build an AI-powered assistant for a real-world business. For this, I chose a printing shop called **PrintSmart**.

The main idea is to create an assistant that can understand customer questions, provide information about printing services, and calculate printing costs based on the shop's pricing.

## What I Learned About AI Agents

While working on this project, I understood the difference between a chatbot and an AI agent.

- A **chatbot** mainly responds to user questions.
- An **AI agent** can understand a goal, decide what actions are needed, use tools, and complete tasks.

I also learned that an agent generally consists of four important parts:

1. **Brain:** The LLM that understands the request and makes decisions.
2. **Instructions:** The system prompt that defines the assistant's role, facts, and rules.
3. **Tools:** Functions that allow the agent to perform specific tasks.
4. **Memory:** Helps maintain the conversation context.

I also understood the basic agent workflow:

**Think → Act → Observe → Repeat**

This helps an agent decide what to do next based on the result of its previous action.

## Understanding LangChain

One of the important topics I explored was **LangChain**.

LangChain is a Python framework that helps us build applications using Large Language Models (LLMs). Instead of writing all the logic for prompts, model communication, memory, and tools manually, LangChain provides reusable components to make development easier.

I understood that LangChain acts as a common interface between our application and different LLM providers.

For example, we can work with models from providers such as Groq, OpenAI, and Gemini. When switching providers, we generally change the model integration and its configuration, while keeping much of the prompt and application logic the same.

### Important LangChain Components

**1. Chat Models**

Chat models connect our application to an LLM. They receive messages and generate responses.

I used `ChatGroq` to connect to a model through Groq.

```python
from langchain_groq import ChatGroq

llm = ChatGroq(
    model="openai/gpt-oss-20b",
    temperature=0.3
)
```

I also learned about `invoke()`, which is used to send input to the model and get a response.

**2. Prompt Templates**

Prompt templates help us create reusable instructions instead of writing the complete prompt every time.

We can define placeholders and pass dynamic values whenever we need to generate a response.

For example:

```python
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are an assistant for {business}."),
    ("human", "{question}")
])
```

Here, `{business}` and `{question}` are placeholders that get filled with actual values.

**3. Output Parsers**

The model may return its response in a message object. Output parsers help convert the response into a format that is easier to use in our application.

I used `StrOutputParser()` to extract the response as plain text.

```python
from langchain_core.output_parsers import StrOutputParser

parser = StrOutputParser()
```

**4. Message History and Memory**

I explored `MessagesPlaceholder()` to include previous conversation messages in the prompt.

This allows the assistant to understand the conversation context instead of treating every question as completely new.

For example, if a customer first asks about printing prices and then asks, "What about 50 pages?", the previous conversation can help the assistant understand what the customer means.

**5. Chains**

A chain connects multiple LangChain components into a single workflow.

I used the pipe operator (`|`) to connect the prompt, language model, and output parser.

```python
chain = prompt | llm | StrOutputParser()
```

The flow is:

**User Input → Prompt Template → LLM → Output Parser → Final Response**

This makes the process easier to organize and execute.

### Why I Found LangChain Useful

Before exploring LangChain, I understood that we could directly communicate with an LLM using its API. However, as an application becomes more complex, managing prompts, conversation history, tools, and multiple steps manually can become difficult.

LangChain provides reusable building blocks and a common interface that make these tasks easier to manage. It does not make the LLM itself smarter; it helps us organize the application around the model.

## My Implementation – PrintSmart

I customized the assistant's instructions according to the services offered by PrintSmart.

The shop supports:
- A4, A3, and A5 document printing.
- PDF files only.
- Black-and-white and color printing.
- Single-sided and double-sided printing.

### Printing Prices

| Service | Price |
|---|---:|
| B&W single-sided | ₹1 per page |
| B&W double-sided | ₹2 per sheet |
| Color printing | ₹10 per page |

I also added rules to ensure that the assistant does not invent prices, promise unconfirmed completion times, or provide incorrect delivery information.

## Testing the Assistant

I learned that simply getting a response from an LLM does not mean the application is working correctly.

To test the assistant, I created a `tests.csv` file containing customer questions and the expected information that should appear in the answers.

I used Python to run these test cases and calculate the pass rate.

I also explored:

- Improving the system prompt and comparing test results.
- Changing the model temperature to `1.0`.
- Running the same tests multiple times to observe differences in responses.

This helped me understand why testing and prompt improvements are important when building reliable LLM applications.

## Choosing the Right Approach for a Task

Another important thing I learned is that not every task requires an AI agent.

For example:

- **Plain Python code:** Calculating printing prices because the steps are fixed.
- **One LLM call:** Answering customer questions about printing services.
- **AI Agent:** Handling a complete order that may involve multiple steps, decisions, and tool usage.

Choosing the simplest approach that solves the problem is important because agents can be slower, more expensive, and introduce additional risks.

## Conclusion

This exercise gave me practical exposure to LangChain, prompt engineering, LLM integration, conversation history, testing, and the basic concepts behind AI agents.

I now have a better understanding of how these components work together to build an AI-powered application.

My next step is to explore how to connect Python functions as tools and allow the assistant to perform actual tasks instead of only responding to customer questions.
