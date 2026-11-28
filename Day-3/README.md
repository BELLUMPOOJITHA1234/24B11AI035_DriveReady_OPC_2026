# 🖨️ Day 3 – Campus Print Shop Assistant with LangChain
  
Drive Ready – AI Agents | Aditya University

> A student asks, “How much for two copies of my assignment, with spiral binding on both?” Can the assistant remember the page details and charge for binding twice?

On Day 2, I built a chatbot for a campus print shop. On Day 3, I rebuilt the assistant using LangChain and created 10 tests to check its answers. I also explored which print shop tasks need a simple Python function, one model call, or an agent.

## 🎯 Task 1 – Build the assistant for my business

The Campus Print Shop assistant helps students check prices, estimate printing costs, and compare options before confirming an order. I wrote a system prompt that gives the assistant the shop’s prices and tells it how to answer customers.

| Service | Shop rate |
|---|---:|
| Black-and-white printing | ₹2 per printed side |
| Colour printing | ₹10 per printed side |
| Spiral binding | ₹30 per copy |

The prompt also includes two shop rules. Double-sided printing saves paper because both sides of a sheet can be used. Pickup times must be confirmed by the shop owner.

### How the LangChain assistant works

1. **Prepare the question.** `ChatPromptTemplate` combines the shop instructions with the student’s question.
2. **Include the conversation.** `MessagesPlaceholder` adds earlier messages so the student can ask follow-up questions such as “What was my page count?”
3. **Generate the reply.** `ChatGroq` sends the prepared messages to the model.
4. **Display the answer.** `StrOutputParser` returns the reply as plain text.

The chain in Python is `prompt | model | StrOutputParser()`.

## 🧪 Task 2 – Test 10 customer questions

I created `tests.csv` with two columns. `question` contains a customer question, and `must_include` contains a word or number that should appear in the answer.

| Example customer question | Must include |
|---|---|
| How much does one colour printed side cost? | `10` |
| How many sheets are needed for 20 pages printed double-sided? | `10` |
| How much do two spiral-bound copies cost if each has 19 black-and-white pages and one colour page? | `156` |
| Can you confirm that my order will be ready at 4 PM? | `owner` |

Each test starts with an empty chat history so its result is independent of the previous question. The notebook prints the full reply beside each `PASS` or `FAIL`. A pass means the expected text appeared. Reading the full reply helps check whether the explanation also makes sense.

## ✏️ Task 3 – Improve one prompt rule

I added one instruction: **calculate the price of one copy including binding, then multiply by the number of copies.** I kept the questions and temperature the same for this comparison.

For example, one copy with 19 black-and-white pages, one colour page, and spiral binding costs **₹38 + ₹10 + ₹30 = ₹78**. Two bound copies cost **₹156**. This test checks whether the assistant charges for binding on both copies.

## 🌡️ Task 4 – Compare temperature and test results

I ran the original and updated prompts at temperature `0.3`. Then I kept the updated prompt, changed the temperature to `1.0`, and ran the same tests twice.

| Experiment | Temperature | Tests passed | Pass rate |
|---|---:|---:|---:|
| Original prompt | 0.3 | [ ]/10 | [ ]% |
| Updated prompt | 0.3 | [ ]/10 | [ ]% |
| Updated prompt – Run 1 | 1.0 | [ ]/10 | [ ]% |
| Updated prompt – Run 2 | 1.0 | [ ]/10 | [ ]% |

**Effect of the prompt change:** The pass rate [went up / went down / stayed the same].

**Effect of temperature:** Across the two runs at `1.0`, the pass rate [changed / stayed the same]. The questions whose reply wording changed were [question numbers, or “none”].

## 🤖 Task 5 – Agent or not?

| Print shop job | Classification | Why it fits |
|---|---|---|
| Calculate the bill for two bound copies | Plain code | The prices and quantities are known. A Python function can calculate the exact amount. |
| Explain single-sided and double-sided printing | One LLM call | The model can give a simple explanation using the shop facts in its prompt. |
| Check whether an order can be ready by 4 PM | Agent | An agent with access to shop tools could check the print queue and printer availability before estimating a pickup time. |

The pickup example is a future use case. The current assistant directs the student to the shop owner for confirmation.

## ▶️ How to run

1. Open `Campus_Print_Shop_Agent_Day3.ipynb` in Google Colab.
2. Add `GROQ_API_KEY` in **Colab Secrets** and enable notebook access.
3. Run the cells from top to bottom. The notebook creates `tests.csv` before running the tests.
4. Read the replies, pass rates, and temperature comparison in the cell outputs.

## 🌱 What I learned

A good test asks a question that matters to the business. For my print shop, “Does the assistant charge binding for both copies?” is more useful than “Can it greet a customer?” Checking the bill against a known answer gives me a practical way to judge whether a prompt change helped.
