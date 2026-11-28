# Day 3 – Campus Print Shop Assistant with LangChain


## Task
Build a print shop assistant using LangChain, test it with 10 customer
questions, and measure how its replies change when I update the prompt
or change the temperature.

## How it works
The system prompt gives the assistant the shop rates: ₹2 per black-and-white
printed side, ₹10 per colour printed side, and ₹30 for spiral binding per copy.
A LangChain prompt template combines these instructions with the customer's
question and chat history. ChatGroq generates the reply, and StrOutputParser
returns it as plain text.

## How I tested it
The file `tests.csv` contains 10 customer questions. Each question has a
`must_include` word or number. Each test starts with an empty chat history
and passes when the expected text appears in the reply.

| Experiment | Temperature | Pass rate |
|---|---:|---:|
| Original prompt | 0.3 | 9/10 (90%) |
| Updated prompt | 0.3 | 8/10 (80%) |
| Updated prompt, run 1 | 1.0 | 10/10 (100%) |
| Updated prompt, run 2 | 1.0 | 9/10 (90%) |

## What changed?
I added one billing rule: add the binding charge to each copy before
multiplying by the number of copies. After that change, the pass rate **went down**.

At temperature 1.0, the two runs scored **10/10 (100%)** and
**9/10 (90%)**. Questions whose reply text differed: **1, 2, 3, 4, 5, 6, 7, 8, 9, 10**.

## Agent or not?

| Print shop job | Classification | Reason |
|---|---|---|
| Calculate a bill | Plain code | A Python function can calculate the exact total from fixed rates and quantities. |
| Explain double-sided printing | One LLM call | The model can explain it in one reply using the shop facts. |
| Check whether an order can be ready by 4 PM | Agent | A future agent could check the print queue and printer availability before answering. |

## How to run
Open `Campus_Print_Shop_Agent_Day3.ipynb` in Google Colab. Enable notebook
access to `GROQ_API_KEY` in Colab Secrets, then run the cells from top
to bottom. The notebook creates `tests.csv` before running the tests.
