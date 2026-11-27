# Day 4 – Agentic AI with LangChain - Tools , Structured Outputs

## 1. Overview

On Day 4, I explored how to build AI-powered business assistants using LangChain. I applied the concepts to two different business scenarios: **Sweet Crumbs Home Bakery** and **PrintSmart Printing Shop**.

The main objective was to understand how an LLM can interact with customers, use tools to perform specific tasks, extract structured information and work with external applications such as Google Sheets.

Rather than building a chatbot that only generates text responses, I explored how to make it perform useful business operations.

## 2. Projects Covered

### 2.1 Sweet Crumbs Home Bakery

Sweet Crumbs is a home bakery business scenario where I worked on developing an AI-powered customer support assistant.

The assistant is designed to handle customer enquiries about bakery products and provide information based on predefined business details.

Some of the business information includes:

- Plum cake – ₹500
- Chocolate cake – ₹650
- Cupcake – ₹80
- Orders are bake-to-order.
- Customers should provide 24-hour advance notice.
- Eggless cake versions are available.

Through this project, I explored how to define an AI assistant's role, business facts and behavioural rules using a system prompt.

### 2.2 PrintSmart Printing Shop

PrintSmart is a printing shop business scenario that I used to explore LangChain tools and their practical applications.

I worked on developing an assistant that can:

- Answer questions about printing services.
- Calculate printing costs.
- Collect customer order details.
- Ask customers to confirm their orders.
- Save confirmed orders in Google Sheets.

This project helped me understand how an AI agent can combine an LLM with Python functions and external services.

## 3. Technologies and Libraries Used

| Technology | Purpose |
|---|---|
| Python | Implementing business logic and functions |
| LangChain | Building AI agents and defining tools |
| LangChain-Groq | Connecting the Groq model with LangChain |
| Groq GPT-OSS-20B | Processing customer messages and tool selection |
| Pydantic | Defining and validating structured information |
| Google Colab | Development and execution environment |
| Google Sheets | Storing business and customer information |
| gspread | Connecting Python with Google Sheets |

## 4. Python Implementation

I developed and executed my implementation in Google Colab and exported the Python file to keep a copy of the work.

The Python file contains the implementation of the business assistants and the concepts explored during the exercise.

It covers the following areas:

- Initializing the language model.
- Defining business-specific information.
- Creating Python functions for business operations.
- Converting functions into LangChain tools.
- Defining structured data using Pydantic.
- Exploring structured output from an LLM.
- Creating an agent using LangChain.
- Connecting the application with Google Sheets.

The Python file serves as the implementation reference for the Day 4 exercise, while this README explains the concepts and workflow behind it.

## 5. Google Sheets Integration

I worked with two separate Google Sheets for the two business scenarios.

### 5.1 Sweet Crumbs – Business Sheet

I maintained a separate Google Sheet for Sweet Crumbs to keep its business information separate from the printing shop records.

**Purpose:**
- Maintain bakery-related information.
- Keep the bakery business data organized separately.
- Use it as a supporting resource for the Sweet Crumbs project.

### 5.2 PrintSmart – Print Orders Sheet

I created a Google Sheet named **Print Orders** to maintain customer printing order records.

The sheet contains the following columns:

| Column | Description |
|---|---|
| Name | Customer name |
| No. of pages | Number of pages to print |
| Print type | Black and white or color |
| Paper size | A4, A3 or A5 |
| Both_side | Single-sided or double-sided |
| Pickup date | Customer's collection date |
| Total | Final printing cost |

I used the `gspread` library to connect Python with Google Sheets.

The order logging functionality calculates the total price and inserts the confirmed customer order into the spreadsheet.

Keeping two separate spreadsheets helps maintain independent records for the two business scenarios.

## 6. Creating LangChain Tools

One of the important concepts I learned was converting normal Python functions into tools using LangChain's `@tool` decorator.

A tool allows an AI agent to execute a specific function when required instead of relying entirely on the LLM to generate an answer.

For PrintSmart, I implemented three tools.

### 6.1 get_printing_price

This tool calculates the total printing cost based on the customer's requirements.

The pricing structure is:

| Printing service | Price per page |
|---|---:|
| B&W single-sided | ₹1 |
| B&W double-sided | ₹2 |
| Color printing | ₹10 |

I also used a Boolean parameter called `both_side`:

- True represents double-sided printing.
- False represents single-sided printing.

### 6.2 get_printing_options

This tool provides information about the printing services offered by PrintSmart, including supported paper sizes, accepted document formats and collection options.

### 6.3 log_print_order

This tool saves confirmed customer orders into Google Sheets.

It receives the customer details, validates the information, calculates the total price and inserts a new row into the Print Orders spreadsheet.

I learned that an agent can use tools to perform actual operations rather than simply describing what it would do.

## 7. Structured Data Using Pydantic

I explored Pydantic to represent customer order information in a predefined structure.

For PrintSmart, I created a `PrintOrder` model containing:

- Customer name.
- Number of pages.
- Printing type.
- Paper size.
- Both-side preference.
- Pickup date.

I learned about:

- `BaseModel` – Defining structured data.
- `Field` – Adding descriptions and validation constraints.
- `Literal` – Restricting values to predefined choices.
- `ge=1` – Ensuring the number of pages is at least one.
- `date` – Representing a valid date.

Pydantic helps ensure that extracted customer information follows the expected structure before it is used by the application.

## 8. Exploring Structured Output

I explored LangChain's `with_structured_output()` method.

It allows the language model to return information following a predefined Pydantic schema.

This is useful when converting informal customer messages into structured Python objects.

For example, a customer might provide their requirements in a natural sentence instead of filling out individual fields.

The model can extract the relevant information into the expected structure.

I also understood that creating a structured-output extractor does not automatically connect it to an agent. It needs to be invoked separately or integrated into the workflow.

## 9. Building the AI Agent

I used LangChain's `create_agent()` to create an agent and registered the printing-related tools.

The agent uses the language model to understand customer requests and decide which tool is appropriate.

I also structured the system prompt into four sections:

**ROLE:** Defines the assistant's identity and responsibility.

**FACTS:** Contains the actual business information, such as prices and available services.

**RULES:** Defines how the agent should behave, when to use tools and when to ask for clarification.

**FORMAT:** Defines how the agent should communicate with customers.

This helped me understand how system prompts guide an agent's behaviour and tool selection.

## 10. Customer Booking Workflow

For PrintSmart, I designed the booking process to follow a clear sequence:

1. Customer requests a printing order.
2. Agent identifies the required information.
3. Agent asks for missing details.
4. Agent calculates the printing cost.
5. Agent displays the complete order summary.
6. Customer confirms the order.
7. Agent calls the order logging tool.
8. The order is saved in Google Sheets.
9. Agent informs the customer about the booking result.

For example, if a customer requests 50 pages of black and white double-sided printing:

- Number of pages: 50
- Printing type: Black and white
- Both sides: Yes
- Paper size: A4
- Price per page: ₹2
- Total: ₹100

The order should be saved only after the customer explicitly confirms it.

## 11. Important Concepts Learned

| Concept | My understanding |
|---|---|
| LangChain Tools | Allow agents to execute specific Python functions. |
| `@tool` | Converts Python functions into LangChain tools. |
| Docstrings | Help the LLM understand a tool's purpose. |
| Pydantic | Defines and validates structured information. |
| `with_structured_output()` | Produces output following a predefined schema. |
| `create_agent()` | Creates an agent capable of using registered tools. |
| System Prompt | Defines the agent's role, facts and behavioural rules. |
| Tool Calling | Allows an LLM to request execution of external functions. |
| Google Sheets Integration | Allows Python applications to store business records. |
| gspread | Provides Python access to Google Sheets. |

## 12. Challenges and Key Learnings

During implementation, I understood that simply creating tools does not guarantee that an agent will call them correctly.

Some important things I learned:

- Tool names and docstrings should clearly describe their purpose.
- Tool parameters should match the actual business requirements.
- Pydantic validation should not require values that are calculated later.
- The agent must distinguish between collecting order details and confirming an order.
- Google Sheets column order must match the values passed to the logging function.
- Business rules should be clearly defined in the system prompt.
- A successful tool execution should be verified before informing the customer that an order was booked.

I also understood the difference between an LLM deciding to call a tool and the Python function actually executing that operation.

## 13. Project Resources

The following resources are included with this Day 4 work:

| Resource | Description |
|---|---|
| Google Colab Python file | Contains the implementation and experiments from the Day 4 exercise. |
| Sweet Crumbs Google Sheet | Separate spreadsheet for the bakery business scenario. |
| Print Orders Google Sheet | Stores customer printing order records for PrintSmart. |

## 14. Final Learning Outcome

By completing Day 4, I gained practical experience in building business-oriented AI assistants using LangChain.

I explored how to:

- Build business-specific AI assistants.
- Convert Python functions into LangChain tools.
- Write meaningful tool descriptions.
- Define structured data using Pydantic.
- Explore structured output from an LLM.
- Integrate Python applications with Google Sheets.
- Build agents with multiple tools.
- Design customer workflows with explicit confirmation.
- Use tool execution to perform business operations.
