# 🖨️ Day 5 – Campus Print Shop Agent with Memory and RAG



## What does the agent do?

A student can ask, “How many sheets do I need for 21 double-sided pages?”
The agent searches the shop’s facts and finds the answer: **11 sheets per copy**.

This is an example of **Retrieval-Augmented Generation (RAG)**. The agent
looks up information relevant to the question and uses it to answer.
It also remembers what a customer said earlier in the same conversation.

## How I built it

| Part | Implementation |
|---|---|
| Business knowledge | I loaded 18 facts from `campus_print_shop_facts.txt` into ChromaDB. Each fact is stored separately so it can be retrieved on its own. |
| Search tool | `search_print_shop_facts` returns the three facts most relevant to a question. It covers prices, binding, paper usage, order details, and pickup policies. |
| Agent tools | I added the search tool alongside the Day 4 tools for checking prices and service availability. |
| Conversation memory | I used `InMemorySaver` and a separate `thread_id` for each customer. The agent can reuse earlier messages in that thread while the notebook session is running. |

## Checking customer memory

I used two conversations, `customer_1` and `customer_2`. In the first, I
introduced myself as Poojitha and then asked, “What is my name?” In the
second, I asked the same question without introducing myself. The expected
behaviour is that only the first conversation recalls my name.

## What happens when a fact is missing?

**Question:** What time does Campus Print Shop close on Sundays?

**Observed result: PASS.** The agent said the closing time was missing
from the shop’s published facts and suggested checking with the owner.
It did not invent a time.

## Final test results

The eight checks cover three service prices, double-sided paper usage,
binding charges for two copies, the unknown Sunday closing time, and
memory for two separate customers.

**Actual Day 5 pass rate:** [copy your result here]  
**Faculty target:** 80% or higher.

## Running the project

1. Open the Day 5 notebook in Google Colab and enable `GROQ_API_KEY` in Colab Secrets.
2. Upload `campus_print_shop_facts.txt` to the Colab Files panel.
3. Run the setup and tool definitions, load the facts into ChromaDB, and create the agent.
4. Run the search, memory, and final test cells to see the replies and pass rate.
