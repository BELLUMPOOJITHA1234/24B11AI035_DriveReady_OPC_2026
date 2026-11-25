# 🧠 Day 2 | Prompts, Memory, and Python for AI Agents

**Drive Ready – AI Agents | One Person Company Initiative | Aditya University**

> A customer says, “I need an eggless cake.” A moment later, they ask, “Can you make it chocolate?” How does the assistant know what **“it”** means?

On Day 1, I learned how to send a question to a language model. Day 2 explores how to continue that conversation. A **prompt** gives the assistant instructions, **memory** carries details from earlier messages, and **Python** brings the two together.

## 🎯 Task

Build a simple chat assistant that follows a set of instructions and uses earlier messages to answer a follow-up question. The example below continues the **Sweet Crumbs Home Bakery** project.

## 1. Prompts — Give the assistant clear instructions

A **prompt** is the text we send to a model. In a chat application, each message has a role.

| Message role | Purpose | Bakery example |
|---|---|---|
| System | Gives the assistant its job and rules | “You help Sweet Crumbs customers. Keep replies short.” |
| User | Contains the customer’s message | “I need an eggless cake.” |
| Assistant | Contains the model’s reply | “All our cakes have an eggless option. Which flavour would you like?” |

A good system prompt includes facts the assistant can use. For example, **“Orders need 24 hours of notice”** helps it answer a timing question. **“If a price is missing, say you don’t know”** tells it how to handle a question it cannot answer.

## 2. Memory — Carry details into the next turn

The model can use an earlier detail when our program includes it in the next request. One simple way to do this is to keep a Python list called `history`.

1. The customer sends a message. Python adds it to `history`.
2. Python sends the system instructions and the conversation so far to the model.
3. The model replies. Python saves that reply in `history` too.
4. When the customer asks a follow-up, the next request includes the earlier exchange.

This is **short-term conversation memory**. In this example, restarting the notebook clears the history.

## 3. Python — Put the conversation together

| Python concept | How the assistant uses it |
|---|---|
| String | Stores text such as a customer’s question. |
| Tuple | Pairs a message role with its text, such as `("human", "I need a cake.")`. |
| List | Keeps the messages in the order they happened. |
| Function | Reuses the same steps each time the customer asks a question. |

## 💻 Example implementation

To try this in Colab, first run the Day 1 setup cells that connect to Groq and create `llm`. Then run the following code once to start a fresh conversation.

```python
system_prompt = (
    "You help customers at Sweet Crumbs Home Bakery. "
    "All cakes have eggless versions. Orders need 24 hours of notice. "
    "Keep replies short. If a detail is unknown, say so."
)

history = [("system", system_prompt)]

def chat(question):
    messages = history + [("human", question)]
    answer = llm.invoke(messages).content

    # Save the completed exchange for the next question.
    history.extend([("human", question), ("ai", answer)])
    return answer
```

The line `llm.invoke(messages)` makes the API call. By sending the full `messages` list, the program gives the model both the new question and the conversation that explains it.

## 🧪 Try a two-message conversation

Run these two lines in order:

```python
print(chat("I need an eggless chocolate cake for six people."))
print(chat("Can you repeat my requirements?"))
```

**Expected behaviour:** The second reply should mention **eggless**, **chocolate**, and **six people**, even though the second question contains none of those details. The exact wording can vary.

To test where the memory lives, run `history = [("system", system_prompt)]` and ask the second question again. The earlier order details are now absent from the conversation sent to the model.

## 🌱 Takeaway

A prompt tells the assistant **how to respond**. Conversation history tells it **what has already been said**. Python decides **what to save and what to send next**.

**BELLUM POOJITHA** · `24B11AI035`
